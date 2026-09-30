# Agent Explainability & Transparency Report

- **Agent Name:** convex-reactive-agent
- **OpenGAP Specification:** 0.1.0
- **Agent ID:** convex-reactive-agent
- **Domain:** Developer Tools / Reactive Agent Runtime & State Component
- **Passport Validation Tier:** Tier-1 Certified Autonomous Agent

---

## 1. Overview & Architectural Purpose

The **Convex Reactive Agent** (`convex-reactive-agent`) is an autonomous database-backed agent runtime component engineered for Convex. Unlike traditional stateless LLM wrappers that require complex external Redis queues or ephemeral HTTP streams, this agent embeds directly into Convex's reactive relational database architecture.

The component manages long-running agent threads, provides real-time token delta streaming over persistent WebSockets, coordinates multi-step tool calls, and performs on-demand hybrid vector/text RAG retrieval. Connected browser clients automatically subscribe to reactive updates, guaranteeing zero state drift and instant synchronization.

---

## 2. How the Agent Decides (Decision-Making Logic)

### 2.1 Reactive Thread Lifecycle & Message Sequencing
- **Decision:** Determines how to ingest, order, and persist incoming user messages, agent replies, and tool outputs within conversation threads.
- **Rules:**
  - Evaluates message timestamps and assigns monotonically increasing sequence indices within the thread document.
  - Resolves concurrent messages from multi-user sessions and human co-agents without race conditions.
  - Automatically triggers thread-level context compaction when cumulative message tokens exceed model threshold limits.

### 2.2 WebSocket Streaming & Delta Batching
- **Decision:** Controls the chunking frequency and serialization format for streaming text deltas and structured objects over WebSockets.
- **Rules:**
  - Emits incremental token diffs rather than full message payloads to minimize network bandwidth and client compute.
  - Batches high-frequency deltas dynamically during high-throughput generation bursts to prevent UI thread lockups.
  - Commits finalized message snapshots to disk upon completion of model streaming.

### 2.3 Hybrid Vector Search & Context Retrieval
- **Decision:** Decides which documents, past thread messages, and external knowledge chunks to inject into the LLM prompt.
- **Rules:**
  - Runs parallel cosine similarity searches on embedding vectors and BM25 full-text index searches on user queries.
  - Merges and re-ranks retrieval results using Reciprocal Rank Fusion (RRF) to balance keyword precision with semantic depth.
  - Caps injected context tokens to respect model constraints and preserve space for reasoning.

### 2.4 Durable Tool Execution & Rate Limiting
- **Decision:** Evaluates proposed tool calls, verifies argument schemas, and checks user rate limits before execution.
- **Rules:**
  - Validates tool arguments against strict Convex argument validators before invoking handlers.
  - Queries the Convex Rate Limiter component to verify token quota availability; delays or rejects calls exceeding allowances.
  - Executes tool actions asynchronously within durable workflow steps, allowing long-running tasks to survive disconnects.

---

## 3. Data Sources & Inputs Used

| Data Input | Source | Purpose | Data Handling & Privacy |
| :--- | :--- | :--- | :--- |
| User Messages & Prompts | Client application via Convex mutations | Instructs agent and supplies conversational queries | Stored in encrypted database tables |
| Thread History & Vector Embeddings | Convex database & Vector Index | Contextual prompt augmentation and similarity search | Embeddings generated via HTTPS, stored locally |
| External Tool Results | Registered API endpoints & server actions | Supplies external live data for multi-step tasks | Validated against schema, sanitized before prompt |
| Usage & Token Metrics | LLM provider metadata headers | Monitors token usage, rate limits, and billing attribution | Stored for telemetry audits, no PII retained |

---

## 4. Known Limitations & Failure Modes

1. **Upstream LLM Provider Rate Limiting:**
   - *Limitation:* Sudden upstream provider throttling can pause streaming delta updates.
   - *Mitigation:* Integration with Convex Rate Limiter component, exponential retry backoff, and multi-model failover routing.

2. **Large Thread Memory Pressure:**
   - *Limitation:* Extremely long chat threads with extensive file attachments can slow down context assembly.
   - *Mitigation:* Hybrid search filtering to load only top-k relevant messages, automated summarization, and file ref-counting.

3. **Out-of-Order Message Ingestion in Multi-Agent Scenarios:**
   - *Limitation:* Rapid simultaneous responses from human agents and AI agents can create conflicting message states.
   - *Mitigation:* Deterministic sequence numbering inside Convex database transactions ensuring serialized ordering.

4. **Stale Vector Embedding Drift:**
   - *Limitation:* Outdated thread messages may surface during semantic search after requirements have changed.
   - *Mitigation:* Temporal weighting in RAG ranking and thread-scoped vector indexing.

---

## 5. Verification, Safety & Human Oversight

- **Human-in-the-Loop Co-Presence:** Threads natively support human agents intervening, pausing, or modifying autonomous agent workflows in real time.
- **Reactive Playground Auditing:** Developers can inspect all intermediate states, tool payloads, and prompt templates using the Convex Agent Playground.
- **Strict Zod/Convex Schema Enforcement:** All tool parameters and return values must pass rigid type validation before and after execution.
- **Encrypted Local Storage:** Database records and vector embeddings are secured with enterprise-grade encryption at rest and in transit.
