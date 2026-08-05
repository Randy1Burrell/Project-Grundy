# Forza UPS Graceful Shutdown Runbook

**Target:** Grundy Proxmox VE host  
**Design:** NUT standalone server on Proxmox, UPS connected by USB  
**Implementation state:** Not yet verified on the live host

## Objective

When utility power remains unavailable long enough to threaten safe runtime, the Forza UPS tells NUT to initiate a Proxmox host shutdown. Proxmox gracefully stops Ubuntu and TrueNAS, then powers off. If utility returns after the UPS has switched off its outlets, the UPS restores output and the BIOS setting `Restore on AC Power Loss = Always On` boots the host.

## Why NUT on Proxmox

The host owns every guest and remains available even if Ubuntu or TrueNAS fails. NUT's standalone pattern runs a hardware driver, `upsd`, and `upsmon` on the same machine. Keep UPS signalling independent of the VMs it must stop.

## Important behavior choice

The safest initial policy is NUT's standard **on battery + low battery** shutdown. Do not begin with a fixed timer until real runtime and shutdown duration have been measured. A later timed policy can use `upssched`, but a timer that is too short creates needless outages and one that is too long may exhaust the battery.

## Prerequisites

- Physical/local console access for the first test
- A maintenance window and verified backups
- UPS data cable connected directly to the Proxmox host
- Proxmox and both guests healthy
- QEMU guest agent enabled where supported
- TrueNAS starts before Ubuntu and shuts down after Ubuntu
- Server, storage enclosure (if any), switch/router needed for remote access all powered by the UPS
- No unrelated high-draw devices such as printers connected to battery-backed outlets

Record before proceeding:

```text
Forza model:
UPS serial:
Rated VA/W:
Battery age:
USB vendor:product ID:
Proxmox node name:
TrueNAS VM ID / shutdown time:
Ubuntu VM ID / shutdown time:
Measured load and estimated runtime:
```

## 1. Baseline normal guest shutdown

Inspect guests and their startup settings:

```bash
qm list
pct list
qm config <truenas-vmid> | grep -E '^(name|onboot|startup|agent):'
qm config <ubuntu-vmid>  | grep -E '^(name|onboot|startup|agent):'
```

In **VM → Options → Start/Shutdown order**:

1. Enable start at boot for both VMs.
2. Give TrueNAS the lower startup-order number.
3. Give Ubuntu the higher startup-order number and a delay long enough for storage readiness.
4. Set realistic shutdown timeouts; TrueNAS must have enough time to stop services and flush ZFS.

Perform a normal host shutdown/restart from the Proxmox UI before involving the UPS. Confirm both guests stop cleanly and start in the intended order. Fix this layer first if they do not.

## 2. Discover the Forza USB interface

Connect the USB data cable and run:

```bash
lsusb
dmesg --ctime | tail -n 80
apt update
apt install nut nut-client nut-server usbutils
nut-scanner -U
grep -i forza /usr/share/nut/driver.list
```

Save `lsusb`'s `vendor:product` ID. `nut-scanner -U` may need root and may propose a driver.

Driver selection:

1. Use `usbhid-ups` if the UPS exposes the USB HID Power Device Class and the driver claims it.
2. If discovery or the device protocol indicates Megatec/Q1/Qx, use `nutdrv_qx`.
3. Treat legacy `blazer_usb` as a last compatibility fallback; current NUT guidance favors `nutdrv_qx` for the Qx family.
4. Do not copy a driver choice based only on the Forza brand. Exact models can use different USB chipsets.

Test candidates one at a time. Stop the current driver before changing it:

```bash
upsdrvctl stop
/lib/nut/usbhid-ups -a grundy-ups -DDD
```

If required, substitute the driver binary path shown by `command -v usbhid-ups` or the `nutdrv_qx` driver. `Ctrl-C` ends the foreground diagnostic. Do not leave multiple drivers competing for the USB device.

## 3. Configure NUT standalone mode

Back up the package defaults:

```bash
install -m 0600 /etc/nut/nut.conf /root/nut.conf.pre-grundy
install -m 0600 /etc/nut/ups.conf /root/ups.conf.pre-grundy
install -m 0600 /etc/nut/upsd.conf /root/upsd.conf.pre-grundy
install -m 0600 /etc/nut/upsd.users /root/upsd.users.pre-grundy
install -m 0600 /etc/nut/upsmon.conf /root/upsmon.conf.pre-grundy
```

Set `/etc/nut/nut.conf`:

```ini
MODE=standalone
```

Set `/etc/nut/ups.conf`, using the discovered driver:

```ini
[grundy-ups]
    driver = usbhid-ups
    port = auto
    desc = "Forza UPS protecting Grundy Proxmox"
```

For `nutdrv_qx`, change only the driver initially:

```ini
    driver = nutdrv_qx
```

If more than one similar USB UPS can be attached, add stable `vendorid`, `productid`, and preferably `serial` match fields based on discovery. Do not pin a volatile USB device number.

Set `/etc/nut/upsd.conf` for local-only access:

```ini
LISTEN 127.0.0.1 3493
LISTEN ::1 3493
```

Generate a unique password locally—do not reuse an administrator password or commit it:

```bash
openssl rand -base64 32
```

Set `/etc/nut/upsd.users`:

```ini
[upsmon-local]
    password = REPLACE_WITH_GENERATED_SECRET
    upsmon primary
```

In `/etc/nut/upsmon.conf`, preserve distribution defaults and ensure these effective lines exist only once:

```ini
MONITOR grundy-ups@localhost 1 upsmon-local REPLACE_WITH_GENERATED_SECRET primary
MINSUPPLIES 1
SHUTDOWNCMD "/sbin/shutdown -h now"
POWERDOWNFLAG /etc/killpower
HOSTSYNC 60
FINALDELAY 30
```

Use the same generated secret in both files. Restrict credentials:

```bash
chown root:nut /etc/nut/upsd.users /etc/nut/upsmon.conf
chmod 0640 /etc/nut/upsd.users /etc/nut/upsmon.conf
```

`HOSTSYNC` matters primarily when remote NUT secondary systems exist; 60 seconds leaves room for later expansion. `FINALDELAY` adds a final 30-second window after a forced-shutdown state. It must remain well below the battery margin.

## 4. Start and validate without shutting down

```bash
systemctl enable --now nut-driver@grundy-ups.service nut-server.service nut-monitor.service
systemctl --no-pager --full status nut-driver@grundy-ups.service nut-server.service nut-monitor.service
upsc grundy-ups@localhost
upsc grundy-ups@localhost ups.status
journalctl -u nut-driver@grundy-ups.service -u nut-server.service -u nut-monitor.service --since today
```

Package unit names can differ. If the templated driver unit does not exist, inspect available units and use the package-provided driver service:

```bash
systemctl list-unit-files 'nut*'
upsdrvctl start
```

Expected normal status includes `OL` (online). Useful fields, when the UPS supplies them, include `battery.charge`, `battery.runtime`, `ups.load`, `input.voltage`, and `ups.status`. Missing cosmetic values are acceptable; reliable `OL`, `OB`, and `LB` state transitions are essential.

Check that port 3493 is loopback-only:

```bash
ss -lntp | grep ':3493'
```

## 5. Non-destructive on-battery test

Do **not** unplug the UPS output or server. Leave the loads plugged into the UPS and disconnect only the UPS input from the wall for 30–60 seconds.

Watch from the console:

```bash
watch -n 2 'upsc grundy-ups@localhost ups.status; upsc grundy-ups@localhost battery.charge 2>/dev/null; upsc grundy-ups@localhost battery.runtime 2>/dev/null'
```

Expected:

1. Status changes from `OL` to `OB`.
2. Proxmox and network remain powered.
3. Reconnect utility power well before low battery.
4. Status returns to `OL`; no shutdown occurs.

Review logs:

```bash
journalctl -u nut-monitor.service --since "10 minutes ago"
```

Stop here if state changes are absent, unstable, or incorrect.

## 6. Validate the shutdown trigger without a real outage

NUT supports forced-shutdown testing, but it invokes the configured shutdown command. First replace `SHUTDOWNCMD` temporarily with a logger-only command in `/etc/nut/upsmon.conf`:

```ini
SHUTDOWNCMD "/usr/bin/logger -t nut-test 'NUT shutdown path reached'"
```

Restart monitoring, then trigger a forced-shutdown event:

```bash
systemctl restart nut-monitor.service
upsmon -c fsd
journalctl -t nut-test --since "5 minutes ago"
```

Restore the real command immediately and restart monitoring:

```ini
SHUTDOWNCMD "/sbin/shutdown -h now"
```

```bash
systemctl restart nut-monitor.service
grep -n '^SHUTDOWNCMD' /etc/nut/upsmon.conf
```

Confirm the UPS returns to a normal non-FSD state after restarting NUT. This test validates the monitor path but does not validate VM shutdown or UPS outlet cutoff.

## 7. Controlled end-to-end outage test

Schedule downtime and monitor locally. Ensure recent backups and stop nonessential workloads.

1. Confirm `ups.status` is `OL` and battery charge is full.
2. Record guest and host state.
3. Disconnect only UPS utility input.
4. Allow the UPS to reach its real low-battery condition, or use a carefully configured short-duration `upssched` policy solely for the maintenance test.
5. Observe Proxmox request guest shutdown. Ubuntu should stop before TrueNAS under reverse startup order.
6. Confirm the host powers off cleanly before the UPS battery is exhausted.
7. Confirm the UPS cuts output only if the driver/model supports the kill-power sequence.
8. Restore utility input. Confirm output returns, BIOS starts the host, TrueNAS starts first, Ubuntu follows, pools are healthy, and services recover.

Validation after boot:

```bash
last -x | head -n 20
journalctl -b -1 -e
zpool status  # run inside TrueNAS
qm list
systemctl --failed
upsc grundy-ups@localhost
```

Pass criteria:

- No guest is force-stopped by timeout.
- TrueNAS reports no unexpected pool fault attributable to shutdown.
- Proxmox halts with battery margin remaining.
- UPS and BIOS recovery behavior is understood and documented.
- Ubuntu containers become healthy without manual repair.

## 8. Optional timed shutdown after runtime measurement

Only add `upssched` if waiting for the UPS `LB` signal leaves insufficient safe shutdown time or if policy requires shutting down after a defined outage. Choose a delay from measured load, battery health, worst-case guest shutdown time, and a conservative reserve. A reasonable example is five minutes, but it is not a recommendation until measurements exist.

In `upsmon.conf`:

```ini
NOTIFYCMD /sbin/upssched
NOTIFYFLAG ONBATT SYSLOG+EXEC
NOTIFYFLAG ONLINE SYSLOG+EXEC
```

In `/etc/nut/upssched.conf`:

```ini
CMDSCRIPT /usr/local/sbin/grundy-upssched
PIPEFN /run/nut/upssched/upssched.pipe
LOCKFN /run/nut/upssched/upssched.lock
AT ONBATT * START-TIMER shutdown-on-battery 300
AT ONLINE * CANCEL-TIMER shutdown-on-battery
```

Create `/usr/local/sbin/grundy-upssched` as root-owned mode `0750`:

```sh
#!/bin/sh
case "$1" in
  shutdown-on-battery)
    /sbin/upsmon -c fsd
    ;;
esac
```

Test cancellation by removing utility power briefly, restoring it before 300 seconds, and confirming no shutdown. Then conduct a maintenance-window end-to-end test.

## 9. Rollback

If NUT behaves unexpectedly:

```bash
systemctl disable --now nut-monitor.service nut-server.service nut-driver@grundy-ups.service
```

Restore the `/root/*.pre-grundy` configuration backups, then disconnect the USB data cable if necessary. This disables automatic signalling but does not affect UPS power delivery. Never leave an unverified shutdown command enabled.

## Maintenance

- Monthly: inspect `upsc`, service status, logs, battery age, load and runtime estimate.
- Quarterly: perform a short `OL → OB → OL` input-loss test.
- Annually and after battery replacement: perform a controlled shutdown test.
- After Proxmox/NUT upgrades: recheck unit names, USB ownership, driver status and `POWERDOWNFLAG` behavior.
- Alert on `OB`, `LB`, stale data, lost communication and battery replacement indicators.

## References

- [NUT user manual](https://networkupstools.org/docs/user-manual.chunked/index.html)
- [NUT `upsmon` manual](https://networkupstools.org/docs/man/upsmon.html)
- [NUT `usbhid-ups` manual](https://networkupstools.org/docs/man/usbhid-ups.html)
- [NUT `nutdrv_qx` manual](https://networkupstools.org/docs/man/nutdrv_qx.html)
- [Proxmox VE administration guide](https://pve.proxmox.com/pve-docs/pve-admin-guide.pdf)

