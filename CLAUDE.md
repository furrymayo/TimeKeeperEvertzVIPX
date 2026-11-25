# TimeKeeper (Evertz VIPX Integration)

**Last Updated**: 2025-12-31
**Status**: Active
**Primary OS**: Windows

## Overview
Node.js server providing a REST API and real-time WebSocket interface for managing countdown timers, messages, and rooms. Integrates with Evertz VIPX broadcast equipment to display timer data via UMD protocol over TCP.

## Current State
- Core functionality operational: timers, rooms, messages via API and Socket.IO
- VIPX integration active: sends UMD-formatted countdown data to configured devices
- Server runs on port 4000 (configurable via `TIMEKEEPER_PORT` env var)

## Quick Reference
| Item | Value |
|------|-------|
| Server port | 4000 (default) |
| VIPX UMD port | 9801 |
| Data file | `timekeeper-data.json` |
| API base | `http://localhost:4000/api/` |
| Viewer client | `http://localhost:4000/` |

## File Map
| Need to know... | See |
|-----------------|-----|
| API endpoints (Rooms, Timers, Messages) | `README.md` |
| System design & data flow | `docs/architecture.md` |
| VIPX hosts & network config | `docs/infrastructure.md` |
| Why we made X decision | `docs/decisions.md` |
| Architecture decisions (ADR index) | `docs/adr/README.md` |
| Current blockers/issues | `docs/known-issues.md` |
| How to do [task] | `docs/runbooks/[name].md` |
| Domain concepts (UMD protocol, etc.) | `docs/reference/[topic].md` |

## Working rules

- A design choice (protocol, format, framework or major library, security approach, data/config model, deployment topology) adds an ADR in `docs/adr/` in the same PR, plus its row in `docs/adr/README.md`. Never rewrite an accepted ADR; supersede it with a new one.

## Key Files
| File | Purpose |
|------|---------|
| `main.js` | Server application (Express + Socket.IO + VIPX) |
| `timekeeper-data.json` | Runtime data (rooms, timers, VIPX configs) |
| `views/index.html` | Viewer client |
| `views/static/js/timekeeper.js` | Client-side JavaScript |

## Recent Activity
- 2025-12-31: Standardized project structure, added environment config support
