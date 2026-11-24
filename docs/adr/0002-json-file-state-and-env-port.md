# 0002. In-memory state persisted to `timekeeper-data.json` (VIP-X targets included); listen port from the environment

- **Status:** accepted
- **Date:** 2018-11-29 (JSON persistence, upstream e7e3bbb); 2023-10-27 (VIP-X targets in the data file, 6a5da86); 2026-10-05 (port via dotenv, f329c80, PR #1)
- **Deciders:** upstream (persistence); Caleb (VIP-X targets, dotenv)
- **Backfilled:** yes, 2026-11-24, from `main.js`, `docs/architecture.md` ("Data Persistence"), `docs/decisions.md` (2025-12-31 rows), commits e7e3bbb, d00a9a6, 6a5da86, f329c80 (PR #1), 96153d2 (PR #2)
- **Related:** ADR 0001, ADR 0003

## Context

The server keeps `Rooms`, `Timers`, `Messages` and `VIPXConfigs` in in-memory arrays (`docs/architecture.md`, "Data Persistence"). Upstream's first commit already saved state to `timekeeper-data.json` with `fs.writeFileSync` and loaded it on startup (e7e3bbb `saveFile()`); the file itself was added to the repo on 2019-02-15 (d00a9a6). The VIP-X work needed somewhere to keep each card's host and port: "User will have to put in IP/Port of VIPX cards in the 'timekeeper-data.json' file. However, once they do that, it shouldn't have to be updated again" (6a5da86).

## Options considered

1. **VIP-X targets in `timekeeper-data.json` (chosen)**: "runtime-configurable infrastructure settings, not secrets; users may need to modify without code access" (`docs/decisions.md`, 2025-12-31 row).
2. **VIP-X targets in `.env`**: rejected, "not secrets, may need runtime modification" (`docs/decisions.md`, 2025-12-31 row).
3. **Listen port via dotenv (chosen)**: "allows port and future config to be set via environment variables without code changes"; the alternatives were hardcoded values or a config file (`docs/decisions.md`, 2025-12-31 row).

Rationale for the JSON file itself is not recorded from the time (see ADR 0001 on the "(original)" rows).

## Decision

- All state lives in memory. It is rewritten to `timekeeper-data.json` after every change (`saveFile()` calls in `main.js`) and loaded at startup.
- VIP-X targets are the `VIPXConfigs` array (host and port per card) in the same file.
- The listen port is `process.env.TIMEKEEPER_PORT`, loaded with `dotenv`, defaulting to 4000 (`main.js`; f329c80). `.env` is gitignored and `.env.example` is tracked (96153d2).

## Consequences

- One process is the only writer. There is no locking and no schema version in the file.
- `timekeeper-data.json` is tracked in this public repo (sample rooms, a test timer, a loopback VIP-X target), and the server rewrites it at runtime. A site's real timers and card addresses therefore show up as local changes to a tracked file. Site addresses otherwise live in the untracked `docs/infrastructure.md` (96153d2).
- `docs/decisions.md` dates the dotenv rows 2025-12-31; the commit that implements them is 2026-10-05 (f329c80).
