# Carry-Forward: Grimoire Discord Bot UX Decisions

Architecture-independent product decisions from the NanoClaw × Grimoire planning. Drop these into a fresh `Hermes × Grimoire` scope so you don't re-litigate them.

## Core shape
- Single user (you). No auth, no per-user routing.
- Discord-first. Conversational, not one-shot.
- Thin connector over Grimoire's API — don't rebuild RAG.

## Two modes, explicit prefixes
- `!ask <question>` — content question, conversational answer from the library.
- `!search <query>` — paper/document discovery, returns a list.
- **No intent inference.** Prefix is required. Clarity over cleverness.

## Response formats
- **`!ask`** — markdown prose, 1–2 paragraphs, source documents cited at the end. Grimoire returns this natively.
- **`!search`** — list of ~5 relevant documents. (Open question carried over: what each list item contains — title, author, filename, excerpt? Landed loosely on all four in the PRD draft; reconfirm against Hermes.)

## Thread awareness
- `!ask` is thread-aware: follow-ups in the same thread see prior context.
- `!search` is always standalone — no thread context.
- Pattern reference: llmcord.

## No-results fallback
- If Grimoire returns nothing, auto-search **Hugging Face first, then arXiv**.
- External results include a link instead of a filename.

## Long-response handling
- Bot warns when response will exceed Discord limits and prompts the user to pick `!messages` (split into multiple messages) or `!file` (attach as file).
- 2-minute timeout on that prompt.
- `!continue` recovers if the user misses the prompt window.

## Explicitly cut (v1)
- Universal API connector / autodiscovery — different project.
- Category/subcategory filtering from Discord — Grimoire supports it; deferred to v2.
- Multi-user / auth.
- Inference-based mode selection.

## Open questions to re-ask against Hermes
1. What does Hermes's tool/plugin model look like — is "thin connector tool" still the right shape, or does Hermes want something different (MCP server, native command, etc.)?
2. Who owns Discord message-splitting, @mention translation, and attachments in Hermes? (NanoClaw handled this for free; don't assume Hermes does.)
3. How is thread/conversation state passed to tools in Hermes? The `!ask` thread-awareness design depends on this.
4. Does Hermes have its own long-response flow already, or do you need the `!messages`/`!file`/`!continue` pattern?

## What to leave behind
Everything in this repo's `scope.md`, `process-notes.md`, and `.remember/remember.md` is welded to NanoClaw's architecture and isn't worth porting. `learner-profile.md` is about you, not the project — copy it separately if you want it.
