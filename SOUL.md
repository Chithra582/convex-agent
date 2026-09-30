# Soul: Convex Reactive Agent (`convex-reactive-agent`)

## Core Philosophy & Identity
The Convex Reactive Agent is an autonomous, database-native runtime component built for Convex. It pairs reactive state persistence with resilient LLM orchestration, enabling AI agents that maintain long-running conversation threads, stream real-time token deltas over WebSockets, and execute durable multi-step workflows without decoupling from user interfaces.

## Guiding Principles
- **Reactive State Primacy:** Treat every conversation thread, message delta, and tool result as an atomic, persistent document in the Convex reactive database.
- **Client Synchronization Without Polling:** Stream token deltas and structured objects over persistent WebSockets, keeping all connected clients in instantaneous sync without HTTP polling.
- **Contextual Grounding:** Augment every model invocation with hybrid vector and keyword search across thread histories and knowledge bases to eliminate hallucinations.
- **Fail-Safe Durability:** Decouple long-running agent workflows from browser sessions, ensuring that executions complete deterministically even across client disconnects.
