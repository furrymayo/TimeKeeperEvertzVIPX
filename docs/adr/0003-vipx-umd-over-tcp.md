# 0003. Show timers on Evertz VIP-X cards with UMD text commands over TCP

- **Status:** accepted
- **Date:** 2023-10-27
- **Deciders:** Caleb
- **Backfilled:** yes, 2026-11-24, from commit 6a5da86, `main.js` (`formatUMDData`, `sendDataToVipxCard`, `TimeKeeper_CheckTriggers`), `CLAUDE.md` (Overview, Quick Reference), `docs/architecture.md` ("VIPX Integration")
- **Related:** ADR 0001, ADR 0002

## Context

The fork exists to put TimeKeeper's countdowns on Evertz VIP-X multiviewer cards, in addition to the browser viewers. Commit 6a5da86 made timer IDs "follow the required protocol for VIPX cards" and showed each timer's id on the web display "so user can input that in the VIPX control". The project describes the transport as UMD data over TCP; the card's UMD port is 9801 (`CLAUDE.md`, Overview and Quick Reference).

## Options considered

None recorded. Rationale not recorded at the time.

## Decision

Every 500 ms, `TimeKeeper_CheckTriggers` (`setInterval(..., 500)` in `main.js`) works out each timer's remaining time as `DD:HH:MM:SS` and sends two UMD text commands to every configured card:
- `%<id>D%1S<color><time>%Z`: the countdown, coloured green above two minutes, amber under two minutes and red at zero (`%170C`, `%255C`, `%85C`);
- `%<id>D%2S<label>%Z`: the timer's label.

The `<id>` is the timer's numeric id, which the operator enters in the VIP-X control (`formatUMDData`; 6a5da86). Targets come from `VIPXConfigs` (ADR 0002).

## Consequences

- `sendDataToVipxCard` opens a new TCP connection for each message to each card, writes, and closes it. That is two connects per timer per card every 500 ms. Connection errors are logged, nothing retries, and nothing waits for a reply.
- The cards update on the 500 ms tick, not on the second boundary, so the shown second can lag by up to one tick.
- Adding a card is a config change, not a code change (6a5da86).
- Timer ids must stay numeric because the VIP-X command uses them (`parseInt(timerID)` in `formatUMDData`).
