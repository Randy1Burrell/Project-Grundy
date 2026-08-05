# Enterprise HDD Qualification

**Status:** Complete  
**Qualification completed:** 2026-08-04

## Acceptance criteria

- SMART overall-health result passed
- Extended/long self-test completed without error
- No reallocated, pending, offline-uncorrectable, or reported-uncorrectable sectors
- No material SMART error log entries
- No interface CRC errors recorded in the reviewed reports

## Results

Device names reflect the test session and are not stable identifiers.

| Test device | Model recorded | Serial | Capacity | Hours at qualification | Result |
|---|---|---|---:|---:|---|
| `/dev/sdb` | ST6000NM0095 | ZAD7LNK4 | 6 TB | 47,697 | Pass |
| `/dev/sdc` | ST16000NM000G | ZL24LMWA | 16 TB | 14,368 | Pass |
| `/dev/sdd` | ST6000NM0095 | ZAD8BGEK | 6 TB | 47,745 | Pass |
| `/dev/sde` | ST16000NM001G | ZL23S8FK | 16 TB | 11,797 | Pass |
| `/dev/sdf` | ST16000NM001G | ZL2PJM2V | 16 TB | 11,770 | Pass |
| `/dev/sdg` | ST16000NM000G-2KH103 | ZL24M844 | 16 TB | 14,235 | Pass |
| `/dev/sdh` | ST16000NM001G | ZL2E1XLV | 16 TB | 11,769 | Pass |

## Final-drive evidence

For serial `ZL24M844`, the extended offline test completed without error at 14,235 hours with no first-error LBA. The reviewed report also recorded zero reallocated, pending, offline-uncorrectable, reported-uncorrectable, and UDMA CRC errors. Temperature was approximately 35°C, with a recorded maximum near 36°C.

## Interpretation

All seven drives were qualified for production based on the captured evidence. Qualification reduces initial risk but does not predict future failure or replace redundancy, monitoring, snapshots, or backups. Seagate raw `Command_Timeout` fields may encode multiple counters; retain normalized values and error logs when evaluating changes over time.

## Ongoing schedule

- Short SMART test: weekly
- Extended SMART test: monthly, staggered by disk
- ZFS scrub: monthly, not overlapping extended tests
- Review: after every alert, cabling change, controller change, or unexpected shutdown

Store future raw outputs in [SMART-Reports](SMART-Reports/README.md) using the disk serial and ISO date.

