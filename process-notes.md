# Process Notes

## /scope

**Status:** Complete. Scope doc generated at `docs/scope.md`.

**Idea so far:** Add a Grimoire RAG query tool to NanoClaw so Sarah can Discord-message her bot and get answers from her personal research library (cybersecurity/ML/AI PDFs and tech papers).

**Architecture:**
- NanoClaw: already installed, has Discord connection, web search, Google Calendar, and Ollama (local LLM) tools
- Grimoire: RAG pipeline CLI tool with 4 agents (ingestion, query, watcher, content generation). Exposes a CLI command AND an API.
- The build: write a new NanoClaw tool that calls Grimoire's API, so Claude can query her PDF library as a tool

**Example query that defines the use case:** "I want to create a cybersecurity agent that uses a LLM. What model can I run locally that would be good to coordinate scanning and penetration testing throughout the network."

**Knowledge base contents:** PDFs from cybersecurity and ML/AI classes + tech papers she finds interesting.

**UX decisions:** Single user (herself). Thread-aware conversational context (not just one-shot lookups). llmcord resonated as a reference for the Discord thread-awareness angle.

**Cut decision:** "Generalization" is the big one — no universal API connector, no autodiscovery. One tool, one API, one use case. Category filtering cut to v2.

**Deepening rounds:** 1 round. Surfaced two key refinements:
1. Grimoire has two distinct commands (`ask` for content questions, `search` for paper discovery) — scope doc reflects both
2. Sarah wants explicit prefix control (`!ask` / `!search`) rather than intent inference — she values clarity over cleverness. Strong design instinct.
3. Knowledge base scope is broader than cybersecurity/ML — it's her full personal library (theology, robotics, 3D printing, etc.). Scope doc updated to reflect this.
4. Category filtering is possible in Grimoire but consciously cut to v2.

**Active shaping:** Sarah drove several key decisions — the generalization cut came entirely from her, the explicit prefix over inference was her preference stated firmly, and the v2 category filter was her call. Passive on design/aesthetic (tool has no real UI), active on architecture and UX decisions.

**References that resonated:** llmcord for thread-awareness pattern.

## /prd

**Status:** Complete. PRD generated at `docs/prd.md`.

**What was added vs scope doc:**
- Precise response formats for both modes: `!ask` = summary + (title, author, filename); `!search` = (title, author, filename, excerpt)
- Thread awareness scoped explicitly to `!ask` only — `!search` is always standalone. Sarah stated this clearly without prompting.
- No-results fallback: auto-search Hugging Face first, then arXiv. External results include a link instead of filename.
- Full long-response flow: bot warns + prompts `!messages` or `!file`, 2-minute timeout, `!continue` recovery command.

**"What if" moments:**
- Long message handling was a genuine "hadn't thought of that" moment — Sarah asked what the options were before deciding. Landed on a user-choice model (`!messages` / `!file`) consistent with her preference for explicit control.
- Timeout behavior and `!continue` recovery came entirely from Sarah unprompted — strong systems-thinking instinct.
- Thread context scope (only `!ask`, not `!search`) was stated clearly and immediately — no ambiguity.

**Scope guard:** No scope creep surfaced. Category filtering stayed deferred to v2 as established in /scope. Four solid open questions flagged for /spec.

**Deepening rounds:** 0. Sarah chose to proceed after mandatory questions — the answers were specific enough that further deepening wasn't needed.

**Active shaping:** Very active. Sarah drove the long-response design (asked about options, chose the user-choice model), defined the timeout/recovery behavior completely on her own, and kept scope tight without any prompting. Consistent pattern: she prefers explicit user control over "clever" automation.

## /onboard

- **Who:** Sarah — Tech Support, MDiv + MS in CIS (cybersecurity focus), studies ML/AI/Python, does hackathons on the side
- **Technical experience:** Intermediate. CS background, ~1 year Python, regular Claude Code user. Already motivated to plan before building.
- **Learning goals:** Wants a real end-to-end dev process. Getting better at GitHub but no formal workflow yet.
- **Creative sensibility:** Strategy games (Catan, Ticket to Ride), sci-fi/fantasy/theology reader, country/pop music, choir, acrylic painting, zentangle, robotics, 3D printing. Systems thinker. Warm and communal, not minimalist-cold.
- **Prior SDD experience:** Planned her most recent project (first time). Before that, dove in and figured it out mid-build. Knows the difference firsthand.
- **Energy/engagement:** Enthusiastic, clear communicator, brings rich personal context. Motivated by genuine curiosity, not just shipping something.
