---
name: "websocket-delta-stream-router"
description: "Dispatches real-time text and object deltas over reactive WebSockets to synchronized clients."
---

# WebSocket Delta Stream Router Skill

## Overview
Powers real-time, low-latency streaming without traditional HTTP chunked transfer overhead:
- Computes text and structured object deltas over persistent WebSocket connections.
- Keeps client UIs reactively updated without polling or client re-renders.
- Enables streaming from asynchronous background server functions.

## Capabilities
- Incremental JSON object streaming.
- Adaptive delta batching under high concurrency.
- Client reconnection reconciliation.
