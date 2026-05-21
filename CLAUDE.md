# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

WikiLLM is a system for building LLM-powered personal knowledge bases and advancing project ideas. The system has two core skills:

### wikillm — Verified Knowledge

1. **Data Ingest**: Source documents (articles, papers, repos, datasets, images) are indexed into `raw/`
2. **Wiki Compilation**: LLM incrementally compiles raw data into wiki with summaries, backlinks, and interlinked articles
3. **Q&A**: LLM answers factual questions against wiki/ (search scope: raw/ + wiki/)
4. **Session Filing**: Verified Q&A conclusions are archived to wiki/queries/
5. **Linting**: Health checks for consistency, orphan pages, missing sources
6. **Extra Tools**: Naive search engine over the wiki

### ideallm — Advancing Ideas

1. **Idea Management**: Personal notes and ideas in `ideas/`, classified by confidence (verified/referenced/hypothesized/inspired)
2. **Project Q&A**: LLM answers project-level questions against wiki/ + ideas/
3. **Project Synthesis**: Combines ideas + wiki into project insights in `projects/`, organized by phase (exploring/planning/advancing/validating/concluded)
4. **Thinking Workspace**: Model drafts in `thinking/`, compiled into wiki/ or projects/ when topics conclude

## Directory Structure

```
raw/              # Source documents and unprocessed data (immutable facts)
ideas/            # Dynamic human input (per project, per confidence level)
thinking/         # Model draft workspace (per topic, temporary, deleted after compilation)
wiki/             # LLM-compiled knowledge base (searchable)
  concepts/       # Core concepts (compiled from raw → "what is")
  practices/      # Practice guides (compiled from raw → "how to")
  queries/        # Session artifacts (researched + source-verified → "facts")
  visual/         # Visualizations
  assets/         # Images and resources
projects/         # Project synthesis (compiled from ideas + wiki, organized by phase)
```

## Core Principles

- The LLM writes and maintains all wiki and projects data; manual edits are rare
- User explorations and queries are filed back into the system
- Trust levels are determined by directory location, not metadata tags
- The system focuses on markdown files and Obsidian-compatible formats
- Images are downloaded locally for easy LLM reference

## Three-Layer Isolation

| Layer | Directory | Searchable | Lifecycle |
|-------|-----------|-----------|-----------|
| Input | `raw/` | During compilation | Permanent |
| Draft | `thinking/` | Never | Compiled and deleted |
| Output | `wiki/`, `projects/` | During Q&A and linting | Permanent |
