# Architecture

**Last Updated**: 2025-12-31

## System Overview

TimeKeeper is a real-time countdown timer management system designed for broadcast environments.

```
┌─────────────────────────────────────────────────────────────────┐
│                        TimeKeeper Server                         │
│                         (Node.js/Express)                        │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │   REST API   │  │  Socket.IO   │  │   VIPX Integration   │  │
│  │  (Express)   │  │  (Real-time) │  │   (TCP/UMD Protocol) │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
│         │                 │                      │              │
│         └────────┬────────┴──────────────────────┘              │
│                  │                                               │
│         ┌────────▼────────┐                                     │
│         │   Data Layer    │                                     │
│         │ (In-memory +    │                                     │
│         │  JSON file)     │                                     │
│         └─────────────────┘                                     │
└─────────────────────────────────────────────────────────────────┘
          │                 │                      │
          ▼                 ▼                      ▼
    ┌──────────┐     ┌──────────┐          ┌──────────────┐
    │ REST     │     │ Browser  │          │ Evertz VIPX  │
    │ Clients  │     │ Viewers  │          │ Cards        │
    └──────────┘     └──────────┘          └──────────────┘
```

## Components

### 1. Express REST API
- CRUD operations for Rooms, Timers, and Messages
- Endpoints documented in `README.md`
- JSON request/response format

### 2. Socket.IO Real-time Layer
- Pushes timer/message updates to connected viewers
- Room-based broadcasting (viewers join specific rooms)
- Events: `TimeKeeper_Timers`, `TimeKeeper_Messages`, `TimeKeeper_Rooms`

### 3. VIPX Integration
- Sends UMD-formatted data over TCP to Evertz VIPX cards
- Updates every 500ms via `TimeKeeper_CheckTriggers` interval
- Supports multiple VIPX devices (configured in `timekeeper-data.json`)

### 4. Data Persistence
- In-memory arrays: `Rooms`, `Timers`, `Messages`, `VIPXConfigs`
- Persisted to `timekeeper-data.json` on changes
- Loaded on server startup

## Data Flow

### Timer Creation
1. Client POSTs to `/api/timer/add`
2. Server creates timer object with generated ID
3. Timer added to in-memory array
4. Socket.IO broadcasts to relevant room(s)
5. Data saved to JSON file

### VIPX Updates (500ms interval)
1. `TimeKeeper_CheckTriggers` iterates all timers
2. Calculates remaining time for each
3. Formats as UMD data (`%[ID]D%1S%[color]C[time]%Z`)
4. Sends to all configured VIPX devices via TCP

### Automatic Cleanup
- `TimeKeeper_ReviewTimers` runs every 60 seconds
- Deletes timers/messages expired > 5 minutes
