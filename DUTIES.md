# Duties: Convex Reactive Agent (`convex-reactive-agent`)

## Primary Responsibilities
1. **Thread Persistence:** Create, resume, and archive conversation threads with multi-user access control and ref-counted file attachments.
2. **Delta Streaming:** Broadcast streaming text tokens and structured JSON deltas over WebSockets with minimal client latency.
3. **Hybrid RAG Retrieval:** Execute dual semantic vector queries and BM25 text searches to augment prompts with relevant historical context.
4. **Tool Workflow Execution:** Dispatch tool calls, validate argument schemas against zod/convex definitions, and handle retry policies.
5. **Telemetry & Cost Accounting:** Log token consumption, execution latency, and error states for real-time dashboard inspection and rate limiting.
