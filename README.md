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
- **docs-mcp-server** — A local MCP server that scrapes and indexes official documentation (websites, GitHub repos, npm/PyPI packages, local files) into a version-specific, searchable index, so agents can ground technical claims in the exact library/API version being discussed instead of relying on training-data memory. Useful in the Researcher step (indexing docs for a library or protocol the post covers) and the Fact-checker step (checking a technical claim against the indexed docs). We use [docs-mcp-server by arabold](https://github.com/arabold/docs-mcp-server); see `How_To_Set_Up.md` for installation and `How_To_Use_It.md` for usage examples.
- **Other MCP servers** — Cursor can use additional MCPs (e.g. Browser) for live pages, testing, or research; check the MCP Registry for more options that fit your workflow.
- **Browser capabilities** — Useful for testing web-facing content or looking up sources during research.
- **Fabric prompt library** — Optional. Expose Fabric patterns as Cursor commands (e.g. `/summarize_paper`) via `~/.cursor/commands` or Settings → Rules, Skills and Commands.
- **LifeOS (optional, personal)** — Some contributors run LifeOS, a personal-AI infrastructure layer, globally in Cursor (`~/.cursor/LIFEOS/`, plus a global skill library at `~/.cursor/skills/`). It is personal, per-user infrastructure, not part of this shared methodology, and nobody needs to install it to run this pipeline. If you do have it installed, its Research skill (multi-agent, URL-verified web research) and skills like ArXiv (academic paper search) or BiasCheck can be used as helper tools inside the Researcher, Fact-checker, or Reviewer steps, feeding their findings back into `research_log.md`, `claims_checklist.md`, etc. This project's role prompts and `.cursor/rules/` always take precedence over any globally auto-triggered skill: an agent working in this repo must still produce the pipeline's required files in their required format, even if a global skill contributes some of the underlying research. See `How_To_Set_Up.md` and `How_To_Use_It.md` for details.
- **Cursor chat per role** — One chat per role with the corresponding prompt pinned; reference files with `@research_log.md`, `@outline.md`, etc., to keep each agent scoped.

---


# Practical consideraitions in Cursor

## Crafting your initial prompt
The initial prompt is very important because this is where you try to make the AI agent understand what you want to do. 
Start by crafting your initial prompt, use the *initial_prompt_template.md* as a guide, find it in the **prompts** folder and make a copy of it under a different name.
Set up your AI chat in **Ask** mode and brainstorm a little with the AI to clarify your ideas.
If you want, you can change your AI in **Planning** mode, give it the *initial_prompt_template* and ask it to htink about how to approach the problem. Once you're satisfied with the planning, ask it to implement the plan into the *initial_prompt_template*, while respecting the structure.
You'll use this as your initial prompt.  


## Option 1 (preferable): Execute all roles / prompts sequentially in the same chat
In the initial prompt (see initial_prompt_template.md) ask the agent to stop after every step and allow you to review and intervene. This option is preferable because everything goes in the same context window, and the agent can better mix "learnings" from one step to the next.

## Option 2: Create one chat per role
Open multiple chat tabs or split panes (e.g., **Researcher**, **Outliner**, **Drafter**, **Reviewer**, **Fact‑checker**, **Editor**, **Human-input**).
Each chat becomes a “specialized agent” by pinning a role prompt at the top.

## Use consistent role prompts

Use the full role prompts in the `prompts/` folder (e.g. `researcher_prompt.md`, `outliner_prompt.md`, `drafter_prompt.md`, etc.) by pinning their content at the top of each chat. Short examples for quick reference:


## If you want a “manager” pattern

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
