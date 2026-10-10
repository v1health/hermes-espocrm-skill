# Evidence-based Tests and Reporting

A green `node --check` or `php -l` tests syntax only. It does not establish that the dashboard renders correctly, that counts are accurate, or that the browser fetched the current revision.

## Required test matrix

| Area | Cases |
| --- | --- |
| Transport | 200, 401/403, 500, timeout, invalid JSON, empty body |
| Counts | valid 0, valid nonzero, missing field, invalid status, >page cap, late refresh |
| Data freshness | recent timestamp, stale, future timestamp/clock skew, invalid date |
| Access | least-privilege API account, denied BU, unauthorized record, filter change |
| JS | actual AMD module behavior, no duplicate request, cache invalidation, Promise ordering |
| Visual | desktop/tablet/mobile, 100% zoom, computed style, focus, links, no clipping/overflow |
| Deploy | exact SHA, mode readable by runtime UID, cache, browser and PHP error logs, rollback |

Tests should call the actual class/method under test, not a separate simplified copy of the same logic. For integration tests with representative data, avoid production writes and include no customer information.

## Release report template

```markdown
# EspoCRM Change Review
- Environment: STAGING / PRODUCTION
- Code HEAD and working-tree status:
- Baseline file hash:
- Candidate file hash:
- Files changed:
- Problem observed:
- Root cause verified (specific line + runtime evidence):
- Tests and observed outputs:
- Screenshots and viewports:
- Console/API/PHP errors:
- Data-integrity and authorization impact:
- Known limitations:
- Fresh verified rollback backup:
- What was NOT changed:
- Production was modified? YES/NO
- Approval status: STOP / APPROVED
```

If a check was not possible, explicitly state `not verified` rather than `PASS`.
