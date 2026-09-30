---
name: "hybrid-vector-rag-retriever"
description: "Performs hybrid semantic vector and full-text search across thread messages and knowledge bases."
---

# Hybrid Vector RAG Retriever Skill

## Overview
Augments agent prompts with highly relevant context using dual-index retrieval:
- Combines semantic vector similarity search with BM25 full-text keyword indexing.
- Implements Reciprocal Rank Fusion (RRF) for optimal context ranking.
- Supports cross-thread user history search and external document ingestion.

## Key Features
- Dynamic embedding generation and indexing.
- Thread-scoped and global knowledge base querying.
- Context window budget optimization.
