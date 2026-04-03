# NanoClaw × Grimoire — Research Library Bot

## Idea

Add a Grimoire RAG query tool to NanoClaw so Sarah can send a Discord message and get answers from her personal research library — using explicit `!ask` / `!search` prefixes to control which mode fires.

## Who It's For

Sarah, solo user. She maintains a personal knowledge base in Grimoire covering cybersecurity, ML/AI, theology, robotics, 3D printing, and other topics she cares about. She wants to query that library conversationally from Discord without switching contexts or opening a terminal.

## Inspiration & References

- **llmcord** — thread-aware Discord bot pattern; resonated as the right model for conversational context (not one-shot lookups)
- **NanoClaw** (https://nanoclaw.dev) — already running with Discord, web search, Google Calendar, and Ollama tools. This build adds one more tool to that ecosystem.
- **Grimoire** — Sarah's personal RAG pipeline with 4 agents (ingestion, query, watcher, content generation). Exposes both a CLI and an API. Key commands: `ask` (content question) and `search` (paper/document discovery).

Design energy: functional and purposeful — this is a personal productivity tool, not a showpiece UI. Clarity over decoration.

## Goals

- Wire Grimoire's API into NanoClaw as a callable tool
- Support both query modes (`ask` and `search`) through explicit Discord prefixes
- Preserve thread-aware conversational context so follow-up questions work naturally
- Leave with a repeatable process for adding future tools to NanoClaw

## What "Done" Looks Like

A working Discord channel where Sarah can type:
- `!ask what does my library say about LLM-based network scanning?` → conversational answer drawn from her PDF library
- `!search penetration testing with local LLMs` → returns relevant papers/documents from her library

Responses are thread-aware: a follow-up in the same thread has context from earlier messages. The tool calls Grimoire's API correctly and surfaces the answer in Discord.

## What's Explicitly Cut

- **Universal API connector / autodiscovery** — the tempting direction of "if I can connect Grimoire, what else can I auto-connect?" That's a different (bigger) project. This build is Grimoire specifically.
- **Category/subcategory filtering from Discord** — Grimoire supports narrowing queries by category, but that's v2. Core query modes ship first.
- **Multi-user support** — single user only. No auth, no per-user routing.
- **Inference-based mode selection** — the tool will NOT try to guess `ask` vs `search` from intent. Explicit prefix only.

## Loose Implementation Notes

- New NanoClaw tool file that wraps Grimoire's API
- Parses the `!ask` or `!search` prefix from the incoming Discord message to route to the correct Grimoire endpoint
- Thread ID passed along for conversational context (following llmcord's pattern)
- NanoClaw's existing Discord plumbing handles message splitting, @mention translation, and attachment handling — no need to reinvent that
- Grimoire API already handles the RAG heavy lifting — this tool is a thin connector, not a new pipeline
