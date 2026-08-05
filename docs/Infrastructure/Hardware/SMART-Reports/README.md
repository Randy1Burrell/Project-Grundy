# SMART Reports

This directory stores raw or lightly redacted SMART evidence. Use one file per drive and observation:

```text
YYYY-MM-DD_<serial>_<short|long|full>.txt
```

Capture by stable identifier:

```bash
sudo smartctl -x /dev/disk/by-id/<disk-id>
sudo smartctl -l selftest /dev/disk/by-id/<disk-id>
```

Each report should record:

- date/time and timezone
- TrueNAS version and `smartctl` version
- model, serial, firmware, capacity and power-on hours
- overall health, test result and first-error LBA
- reallocated, pending, offline-uncorrectable, reported-uncorrectable and CRC counts
- temperature and relevant error log

Do not infer disk identity from `/dev/sdX` alone. Link summarized conclusions back to [Drive Qualification](../Drive-Qualification.md).

