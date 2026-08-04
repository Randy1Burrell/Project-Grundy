# Enterprise HDD Qualification

Status: Completed
Date: 2026-08-04

## Objective

Verify every enterprise hard drive before being placed into production.

## Acceptance Criteria

- SMART Status: PASS
- Extended SMART Test: PASS
- Reallocated Sectors = 0
- Pending Sectors = 0
- Offline Uncorrectable = 0
- No SMART Errors

## Results

| Device | Model | Serial | Size | Hours | Status |
|---------|-------|--------|------|-------|--------|
| sdb | ST6000NM0095 | ZAD7LNK4 | 6TB | 47,697 | PASS |
| sdc | ST16000NM000G | ZL24LMWA | 16TB | 14,368 | PASS |
| sdd | ST6000NM0095 | ZAD8BGEK | 6TB | 47,745 | PASS |
| sde | ST16000NM001G | ZL23S8FK | 16TB | 11,797 | PASS |
| sdf | ST16000NM001G | ZL2PJM2V | 16TB | 11,770 | PASS |
| sdg | ST16000NM000G | ZL24M844 | 16TB | 14,235 | PASS |
| sdh | ST16000NM001G | ZL2E1XLV | 16TB | 11,769 | PASS |

## Conclusion

All enterprise drives successfully passed extended SMART testing and are approved for production use.
