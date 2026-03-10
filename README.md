# Article and Blog Writing Environment + Claude Configuration

---

## Overview

### What this is about

This project is an **AI-assisted writing methodology** for technical papers and blog posts. It combines a clear, evidence-based process with **multi-agent orchestration**: different AI roles (Researcher, Outliner, Drafter, Reviewer, Fact-checker, Editor, Human-input) work in sequence on shared files, so that research, structure, drafting, review, fact-checking, and editing are each handled in a focused way. Every claim is meant to be traceable to sources; the human sets goals and constraints and can intervene at any step. At the end, the **Human-input** agent summarizes how the piece was produced and what the human contributed, so the final deliverable includes both the article (`publish.md`) and an explicit record of the methodology and human vs AI contribution.

This is part of Sensorica's [Working with AI program](https://www.sensorica.co/stewarding/help/education/use-ai). 

### How it works: multi-agent orchestration

Writing is done in a **fixed sequence of steps**. Each step is a role (optionally a dedicated Cursor chat with a pinned prompt). The roles and their main outputs are:

1. **Researcher** — Gathers sources (web, Zotero), extracts quotes and facts, fills `research_log.md` and an initial claims checklist.
2. **Outliner** — Builds structure from the research: `outline.md` with sections, three layers (Thematic, Pragmatic, Logical/Emotional), impact/hooks, and notes for the Drafter.
3. **Drafter** — Turns the outline into a first full draft in `draft.md`, using only evidence from `research_log.md`.
4. **Reviewer** — Refines `draft.md` for clarity, gaps, and flow; writes `review_notes.md`.
5. **Fact-checker** — Verifies claims against `research_log.md`, completes `claims_checklist.md`, and writes `fact_check_report.md` (weak/missing evidence).
6. **Editor** — Final polish and style alignment: produces `publish.md` and `edit_notes.md`.
7. **Human-input** — Runs last. Reads `human_input_log.md` (and related notes), categorizes human contributions, and produces `methodology_and_human_contribution.md` (and optionally appends a summary to the deliverable).

Whenever the human provides input in the chat during a step, that role appends an entry to **`human_input_log.md`**, so the Human-input agent can summarize who did what. All role prompts are in the `prompts/` folder.

### Additional tools

- **Cursor** — AI-native IDE that lets you orchestrate multiple specialized writing agents (Researcher, Outliner, Drafter, etc.) through pinned prompts and rules, with direct access to tools like Zotero, the web, and the browser, all inside a single coding environment.
- **Zotero** — Reference manager; store and organize sources. Optional **Zotero MCP server** in Cursor for searching the library and using semantic search (find papers by meaning, not only keywords). See the Zotero site at [`https://www.zotero.org`](https://www.zotero.org) and the Zotero MCP server at [`https://github.com/54yyyu/zotero-mcp`](https://github.com/54yyyu/zotero-mcp) for installation details.
- **Google Drive MCP server** — Lets agents search and open notes, PDFs, Docs, Sheets, and Slides in Google Drive directly from Cursor (for example to pull in source documents or spreadsheets during the Researcher step). We use the [Google Drive MCP Server by piotr-agier](https://github.com/piotr-agier/google-drive-mcp); it uses OAuth 2.0 (desktop app, no client secret) and supports search, read, download, and optional Calendar integration. See `How_To_Set_Up.md` for installation and Cursor MCP configuration.
- **MediaWiki MCP server** — Connects Cursor to any MediaWiki wiki (including Wikipedia or an internal wiki) as a structured research source, with tools like `get-page`, `search-page`, and `get-category-members`. We use the [MediaWiki MCP Server by ProfessionalWiki](https://github.com/ProfessionalWiki/MediaWiki-MCP-Server); after installation, add it as an MCP server in Cursor so that research agents can query wiki pages directly.
- **Other MCP servers** — Cursor can use additional MCPs (e.g. Browser) for live pages, testing, or research; check the MCP Registry for more options that fit your workflow.
- **Browser capabilities** — Useful for testing web-facing content or looking up sources during research.
- **Fabric prompt library** — Optional. Expose Fabric patterns as Cursor commands (e.g. `/summarize_paper`) via `~/.cursor/commands` or Settings → Rules, Skills and Commands.
- **Cursor chat per role** — One chat per role with the corresponding prompt pinned; reference files with `@research_log.md`, `@outline.md`, etc., to keep each agent scoped.

---


# Practical orchestration in Cursor

## 1 Create one chat per role
Open multiple chat tabs or split panes (e.g., **Researcher**, **Outliner**, **Drafter**, **Reviewer**, **Fact‑checker**, **Editor**, **Human-input**).
Each chat becomes a “specialized agent” by pinning a role prompt at the top.

## 2 Use consistent role prompts

Use the full role prompts in the `prompts/` folder (e.g. `researcher_prompt.md`, `outliner_prompt.md`, `drafter_prompt.md`, etc.) by pinning their content at the top of each chat. Short examples for quick reference:


## 3 If you want a “manager” pattern

Create a Manager chat that:

1) Assigns tasks to each role chat.
2) Tracks progress.
3) Pulls outputs and resolves conflicts.

Example manager prompt:
```
You are the Manager. Coordinate Researcher, Outliner, Drafter, Reviewer, Fact‑checker, Writer and Human input outputs. Merge changes and keep files consistent.
```



# Agents
There are different types of agents, based on the methodology that we use. 

## Human contribution / capacity
After a writing session, this agent extracts from the user input the wisdom, vision, intuition, skills that are specifically human, that contributed to the deliverable. This is essential for surfacing the new metacognitive skills that humans develop in the presence of AI, educating or sensitizing users to this emerging dimension of intellectual work and distinguishing between AI and its limitations.


# Things to be integrated later

To do: Integrate a better way to transfer ideas to the AI system, using Steven's input
