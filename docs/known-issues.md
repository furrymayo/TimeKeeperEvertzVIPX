# Known Issues

Track current blockers, bugs, and workarounds.

## Active Issues

| ID | Issue | Impact | Workaround | Status |
|----|-------|--------|------------|--------|
| - | No active issues | - | - | - |

## Potential Improvements

| Item | Description | Priority |
|------|-------------|----------|
| Security | No authentication on API endpoints | Low (internal network use) |
| Dependencies | npm audit shows 13 vulnerabilities (4 low, 1 moderate, 8 high) | Medium |
| Code | `uuidv4()` function referenced but not defined in visible code | Investigate |

## Resolved Issues

| Date | Issue | Resolution |
|------|-------|------------|
| 2025-12-31 | Port hardcoded in main.js | Added environment variable support via dotenv |
