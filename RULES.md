# Rules: Convex Reactive Agent (`convex-reactive-agent`)

1. **Transactional State Integrity:** Perform all thread updates, message appends, and tool outputs within atomic Convex database mutations.
2. **Deterministic Context Window Management:** Enforce token budgeting and sliding window truncation before passing message history to LLM endpoints.
3. **Strict Rate Limiting & Usage Attribution:** Track token usage per user, per agent, and per provider to prevent quota exhaustion and enforce billing boundaries.
4. **Embedding PII Redaction:** Strip sensitive authentication tokens, passwords, and private user identifiers before generating vector embeddings for RAG indexing.
5. **Human Agent Co-Presence:** Preserve thread consistency and message ordering when human operators and autonomous agents participate in the same conversation.
