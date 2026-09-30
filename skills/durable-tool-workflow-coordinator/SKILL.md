---
name: "durable-tool-workflow-coordinator"
description: "Coordinates multi-step tool calling, rate limiting, and token usage attribution across agent workflows."
---

# Durable Tool Workflow Coordinator Skill

## Overview
Executes robust, multi-step tool calling operations across complex workflows:
- Validates tool arguments against strict Convex schemas.
- Interfaces with the Convex Rate Limiter to protect upstream LLM quotas.
- Tracks per-user, per-model, and per-agent token usage for billing and governance.

## Safeguards
- Schema verification before external tool invocation.
- Automatic retries on transient network errors.
- Circuit breaker trip mechanisms on repeated tool failures.
