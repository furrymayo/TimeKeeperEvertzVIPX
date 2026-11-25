# 0001. Build on a fork of the upstream TimeKeeper server (Node.js, Express, Socket.IO rooms)

- **Status:** accepted
- **Date:** 2023-10-27 (first VIP-X commit on the fork, 6a5da86); the inherited stack dates from upstream's first commit, 2018-11-29 (e7e3bbb)
- **Deciders:** Caleb (fork); the stack itself was chosen upstream
- **Backfilled:** yes, 2026-11-24, from the GitHub fork relationship (parent `josephdadams/TimeKeeper`), `git log` (e7e3bbb, 171d8fa, 6a5da86), `package.json`, `main.js`, `README.md`, `docs/architecture.md`, `docs/decisions.md`
- **Related:** ADR 0002, ADR 0003

## Context

This repo is a GitHub fork of the open-source TimeKeeper project. The first 19 commits (2018-11-29 to 2023-09-14) are upstream's; Caleb's work starts with 6a5da86 on 2023-10-27, which added the Evertz VIP-X output (`git log`). Upstream TimeKeeper is "a Node.js server supporting a generic API to create and display timers, countdowns, and messages to client browsers" (`README.md`). Rooms let different viewer screens show different timers, "helpful if you are running clocks in multiple venues" (`README.md`, Rooms).

## Options considered

No alternatives to forking are recorded. Rationale not recorded at the time.

`docs/decisions.md` has three "(original)" rows for the inherited stack: Socket.IO over raw WebSockets or SSE, room-based routing over a single global broadcast, and a JSON file over SQLite or MongoDB. Those rows were written in the 2026-10-05 scaffold commit (f329c80), long after upstream made the choices, so they are a later reading, not the reasons recorded at the time.

## Decision

Extend the upstream codebase rather than write a new server. The inherited stack stays as it is (`main.js`, `package.json`):
- Node.js, with Express (`express` ^4.18.2, `body-parser` ^1.20.2) serving the REST API for Rooms, Timers and Messages.
- Socket.IO (`socket.io` ^4.7.2) pushing `TimeKeeper_Timers`, `TimeKeeper_Messages` and `TimeKeeper_Rooms` to viewers. A viewer joins its room; a Timer or Message with `roomID` `room-0` goes to every viewer (`main.js` `socket.join(roomID)`, `io.to(...)`; `README.md`).
- `axios` for the `http` trigger type (`README.md`, Timers).

## Consequences

- Upstream's design decisions (API shape, room model, JSON persistence) come with the code. Caleb's changes sit on top of them, mainly the VIP-X output (ADR 0003).
- Socket.IO went from ^2.1.1 to ^4.6.2 upstream (171d8fa, 2023-06-12), before the fork's work began.
- Dependencies are caret ranges in `package.json`. `package-lock.json` is tracked but also listed in `.gitignore`.
