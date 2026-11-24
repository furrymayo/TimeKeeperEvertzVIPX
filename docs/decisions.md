> **Legacy log (2026-11-24).** New decisions go in `docs/adr/` as ADRs (index: `docs/adr/README.md`). This file is kept for history and takes no new entries. Where the entries went: the two dotenv / VIP-X-config rows (2025-12-31) → ADR 0002. The three "(original)" rows describe upstream TimeKeeper's stack and were written in the 2026-10-05 scaffold, not when upstream decided; ADR 0001 (fork) and ADR 0002 (JSON file) cover them. The VIP-X UMD output was never logged here → ADR 0003.

# Decision Log

Track architectural and implementation decisions with context and rationale.

| Date | Decision | Rationale | Alternatives Considered |
|------|----------|-----------|------------------------|
| 2025-12-31 | Use `dotenv` for environment configuration | Allows port and future config to be set via environment variables without code changes | Hardcoded values, config file |
| 2025-12-31 | Keep VIPX configs in `timekeeper-data.json` | These are runtime-configurable infrastructure settings, not secrets; users may need to modify without code access | Move to .env (rejected: not secrets, may need runtime modification) |
| (original) | Store data in JSON file | Simple persistence without database dependency; suitable for small-scale deployment | SQLite, MongoDB |
| (original) | Use Socket.IO for real-time updates | Reliable WebSocket abstraction with fallback support; well-suited for browser clients | Raw WebSockets, SSE |
| (original) | Room-based message routing | Allows multiple displays to show different timer sets; venue flexibility | Single global broadcast |
