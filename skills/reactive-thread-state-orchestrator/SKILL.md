---
name: "reactive-thread-state-orchestrator"
description: "Manages persistent conversation threads, multi-user message history, and durable workflow state in Convex."
---

# Reactive Thread State Orchestrator Skill

## Overview
Coordinates persistent conversation threads and multi-participant state in Convex:
- Manages thread creation, message appending, and participant assignments (users, agents, human operators).
- Tracks ref-counted file attachments in Convex file storage.
- Ensures atomic transaction boundaries for all thread updates.

## Workflow
1. Ingest user prompt and resolve active thread identifier.
2. Query message history and assemble context window.
3. Commit agent response and tool execution records into persistent database tables.
4. Broadcast reactive update events to subscribed clients.
