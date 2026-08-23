# Manual: How to recreate this AI-enhanced writing tool

Thtis is a **manual** for setting up the environment, tools, and workflow so you can recreate and adapt this AI-enhanced writing setup (Cursor, Claude, Zotero, MCP, prompts, and file structure).

---


Tools:
- Markdown editor (ex. Cursor)
- Reference manager: Zotero (open source, runs local and synchronizes to shared online repo)
- Web Scraping: Scraper or Dataminer browser addon 
- Screenshots/figures (optional)

## Install Cursor and extentions

https://cursor.com/download 

After installing Cursor, install these **extensions** (Extensions view: `Ctrl+Shift+X` in Cursor):

- **Markdown All in One** — shortcuts, preview, TOC, folding for markdown
- **markdownlint** — linting for consistent markdown formatting
- **Code Spell Checker** — spell check in prose and markdown
- **Paste Image** — paste screenshots into markdown
- **Rainbow CSV** — view and edit CSV files (for data sources)
- **Zotero** — insert citations from Zotero into markdown (search for "Zotero" in the marketplace)
## Install Zotero on your computer. 
See more on https://www.zotero.org/ 

Once installed on your machine, connect yourself to the Zotero Sensorica group https://www.zotero.org/groups/6418034/sensorica
This is our shared *knowledge commons*. Everyone in the network can contribute to this structured repository of papers and use it.

## Install Zotero MCP server
See more https://github.com/54yyyu/zotero-mcp
This MCP in particular also installs an LLM on your computer to perform semantic indexing of files in the Zotero library.

**Prerequisites** (you already have these):

* Zotero installed and running
* Zotero local API enabled: Zotero → Preferences/Settings → Advanced → check “Allow other applications on this computer to communicate with Zotero”

**Install the Zotero MCP server (official repo: Python)**

From https://github.com/54yyyu/zotero-mcp, the project is Python-based. Pick one:

Option A – with uv:
```
uv tool install "git+https://github.com/54yyyu/zotero-mcp.git"
zotero-mcp setup  # Auto-configure (Claude Desktop supported)
```
Option B – with pip:
```
pip install git+https://github.com/54yyyu/zotero-mcp.git
zotero-mcp setup  # Auto-configure (Claude Desktop supported)
```
NOTE: 
* you may need to have install Python package and environment manager. Run the command *uv --version*
in the terminal to see if you have it. 
* you may also need to install git. Try the command *git --version* to see if it is installed. 


Also configures semantic search and can help with MCP clients through the *setup* command

After this, the zotero-mcp command should be on your PATH.

**Get your Zotero IDs** (for Cursor)

You need two IDs if you use both a group library and your personal library:
* Personal library (user) ID:
Go to https://www.zotero.org/settings/keys and log in.
Your user ID is in the URL of your library, e.g. https://www.zotero.org/users/-user-ID-.
* Or: Zotero → Preferences → Sync and note the user ID if shown.
Group library ID (e.g. Sensorica):
Open the group page, e.g. https://www.zotero.org/groups/6418034/sensorica.
The number in the URL is the group library ID (e.g. 6419034).


**Connect to Cursor (if you use Cursor)**

Edit ~/.cursor/mcp.json. Add 2 Zotero MCP servers, online/group, pointing to a shared library, and zotero-personal/user pointing at your personal library.

``` "zotero": {
      "command": "zotero-mcp",
      "env": {
        "ZOTERO_LOCAL": "true",
        "ZOTERO_LIBRARY_ID": "YOUR_LIBRARY_ID",
        "ZOTERO_LIBRARY_TYPE": "group"
      }
    },
     "zotero-personal": {
      "command": "zotero-mcp",
      "env": {
        "ZOTERO_LOCAL": "true",
        "ZOTERO_LIBRARY_ID": "YOUR_USER_ID",
        "ZOTERO_LIBRARY_TYPE": "user"
        }
	}
  ```
  NOTE: replace "YOUR_USER_ID" and "YOUR_LIBRARY_ID" above with your proper ID numbers. These numbers are not the same. Also note that with Cursor you don't need to connect to your local Zotero, so you don't need the *zotero-personal* and *YOUR_USER_ID* is not required. It is required for VSCodium though... 

**Get your Zotero user ID**

* Go to https://www.zotero.org/settings/keys
* Log in if needed
* Find your user ID (often shown as “Your userID”or in the URL for your library)
* If you can’t see it there, try:
  * Opening your library at https://www.zotero.org/users/XXXXXX and using the number in the URL
  * Or: Zotero → Preferences → Sync → your user ID if shown


**Connect to VSCodium** (if you use it)
```
VSCodium

   → node

       → build/index.js

           → Zotero local API
```
Open ~/.continue/mcpServers/new-mcp-server.yaml

Edit the .yaml file to the following:
```
name: Zotero MCP
version: 0.0.1
schema: v1

mcpServers:
  - name: zotero
    command: node
    args:
      - /usr/lib/node_modules/zotero-mcp/build/index.js
    env: {}
```
Replace /usr/lib/node_modules with the exact output of:
```
npm root -g
```

On some systems it may be:

* /usr/local/lib/node_modules
* /home/username/.nvm/...

Use the exact path you observed.


Restart VSCodium cleanly. Close VSCodium completely. 
Launch from terminal to ensure PATH correctness:
```
codium
```

more info https://chatgpt.com/share/6992c7a2-aa84-8004-b69f-26ed092b21ac

**Start the Zotero MCP server?** 

Once the MCP server is installed and connected to Cursor, you need to initialize the *semantic search database* to enable the semantic indexing, to access the semantic functionality.

You do not normally start the Zotero MCP server manually when using Cursor. Cursor (like VSCodium with MCP support) launches the MCP server process itself based on your mcpServers configuration. The MCP server is designed to run over stdio, not as a background daemon.

You are using: zotero-mcp with Zotero desktop.

Cursor will spawn:
```
node /path/to/zotero-mcp/build/index.js
```
You can manually enter in terminal: 
```
node $(npm root -g)/zotero-mcp/build/index.js
```
Expected behavior:
* It prints nothing
* It does not exit
* It waits (because it is listening on stdio)

That “blocking” state means it is running correctly.

* Stop it with: CTRL/C in your terminal


To update the semantic search database for the personal library, after adding zotero-personal, run:
```
# With zotero-personal env vars setZOTERO_LIBRARY_TYPE=user ZOTERO_LIBRARY_ID=YOUR_USER_ID zotero-mcp update-db
```

NOTE: replace YOUR_USER_ID with the actual number

Or configure semantic search for the personal library via zotero-mcp setup so its index is built.


### Integration between Cursor and Zotero
Make sure that Cursor can talk to Zotero. 

So as long as:
- Zotero is running when you use the MCP server, and
- Cursor has the Zotero MCP server configured and enabled,

You should be able to search your Zotero library and retrieve items from Cursor. If you’ve already added the MCP server in Cursor, try a chat prompt like “Search my Zotero library for …” to confirm it works.

Using the semantic capabilities, you'll also be able to 
1.  Conceptual Search:
* Find papers by meaning, not just keywords
* Search for topics like "activism protest movements"
* Discover related research conceptually
2. Advanced Queries You Can Now Use:
* "Find research similar to social movements and political activism"
* "Papers about peer-to-peer economics and governance"
* "Studies related to collaborative economy models"
* "Research on decentralized organization structures"
3. Cross-Topic Discovery:
* Find connections between different research areas
* Discover papers that share conceptual themes
* Explore your library by ideas rather than exact terms

## Install Google Drive MCP server (piotr-agier)

The **Google Drive MCP Server** by [piotr-agier](https://github.com/piotr-agier/google-drive-mcp) lets Cursor work with Google Drive, Docs, Sheets, Slides, and Calendar via a standardized MCP interface (search, read, download, create, update, etc.).

**Requirements:**
- Node.js 18 or higher (LTS recommended)
- A Google Cloud project with these APIs enabled: Google Drive API, Google Docs API, Google Sheets API, Google Slides API (and optionally Google Calendar API)
- OAuth 2.0 credentials of type **Desktop application** (Client ID only; no client secret)

**Google Cloud setup (summary):**
1. In [Google Cloud Console](https://console.cloud.google.com/), create a project and enable the APIs above.
2. Configure the OAuth consent screen (e.g. External, add your email as test user), and add the scopes you need (e.g. `drive`, `drive.readonly`, `documents`, `spreadsheets`, `presentations`, and optionally `calendar`, `calendar.events`).
3. Create credentials: **Create credentials → OAuth client ID → Application type: Desktop app**. Download the JSON, rename it to `gcp-oauth.keys.json`.

**Install and run (recommended: npx):**
```bash
# First-time auth (browser opens for Google sign-in; tokens saved to ~/.config/google-drive-mcp/tokens.json)
npx @piotr-agier/google-drive-mcp auth
```
You can also run the server directly; it will prompt for auth on first run:
```bash
npx @piotr-agier/google-drive-mcp
```

**Configure in Cursor:**
1. Go to **Cursor Settings → MCP → Add new MCP Server**.
2. Add a server with:
   - **Command:** `npx`
   - **Args:** `@piotr-agier/google-drive-mcp`
   - **Env:** `GOOGLE_DRIVE_OAUTH_CREDENTIALS` = path to your `gcp-oauth.keys.json` (e.g. `/home/you/config/gcp-oauth.keys.json`)

Example MCP config (e.g. in `~/.cursor/mcp.json` or via Cursor UI):
```json
{
  "mcpServers": {
    "google-drive": {
      "command": "npx",
      "args": ["@piotr-agier/google-drive-mcp"],
      "env": {
        "GOOGLE_DRIVE_OAUTH_CREDENTIALS": "/path/to/your/gcp-oauth.keys.json"
      }
    }
  }
}
```

After setup, agents can search Drive, read Docs/Sheets, download files, and (if configured) manage Calendar events. For full options (Docker, token path, scopes, troubleshooting), see the [project README](https://github.com/piotr-agier/google-drive-mcp).

## Install docs-mcp-server (technical documentation indexing)

The **Docs MCP Server** by [arabold](https://github.com/arabold/docs-mcp-server) indexes official documentation (websites, GitHub repos, npm/PyPI packages, local files, zip archives) into a local, searchable, version-specific index. This reduces hallucinations when a post discusses a specific library, framework, or API: instead of relying on the model's training data (which may be outdated or wrong for the exact version in question), the agent can scrape and query the real, current docs.

**Requirements:**
- Node.js 22 or higher (for the `npx` option), or Docker as an alternative.

**Install and run (recommended: npx, no separate install step needed):**
```bash
# Index a library's documentation (one-time or when starting a new topic)
npx @arabold/docs-mcp-server@latest scrape react https://react.dev/reference/react

# Query the index from the CLI (useful to sanity-check before wiring up MCP)
npx @arabold/docs-mcp-server@latest search react "useEffect cleanup" --output yaml
```

**Configure in Cursor** (e.g. in `~/.cursor/mcp.json`):
```json
{
  "mcpServers": {
    "docs-mcp-server": {
      "command": "npx",
      "args": ["-y", "@arabold/docs-mcp-server@latest"]
    }
  }
}
```

Once configured, Cursor agents have access to tools like `scrape_docs` (index a new library/version), `search_docs` (query the index), `list_libraries`, `find_version`, `refresh_version`, `remove_docs`, and `fetch_url` (fetch any single page and convert it to Markdown). See `How_To_Use_It.md` for example prompts and the [project docs](https://github.com/arabold/docs-mcp-server) for Docker setup, embedding-model configuration, and advanced scraping options (e.g. `preserveHashes` for hash-routed SPA doc sites).

## Optional: LifeOS (personal AI skill layer)

LifeOS is a personal AI-assistant infrastructure layer that some contributors run globally in Cursor, independent of this repository: a runtime and constitution under `~/.cursor/LIFEOS/`, and a global skill library (auto-discovered by Cursor) under `~/.cursor/skills/` with dozens of skills (Research, ArXiv, ExtractWisdom, BiasCheck, DetectAI, Fabric, and more).

This is per-user, personal infrastructure, not part of the shared writing methodology. You do not need to install it to recreate or use this environment; skip this section if you do not already have it.

If you do have LifeOS installed, note two things when working in this repo:

- Some LifeOS skills auto-trigger on keywords (for example, its Research skill triggers on the word "research" and runs a multi-agent, URL-verified web research workflow). That can be a useful helper inside the Researcher or Fact-checker steps of this pipeline, but it produces its own report format, not `research_log.md`.
- This project's role prompts (`prompts/`) and rules (`.cursor/rules/writing-methodology.mdc`) always take precedence for work done in this repo. If a global skill contributes research, quotes, or facts, they still need to be folded into `research_log.md` (with source, quote, and claims-checklist entries) before the pipeline can treat them as evidence; a skill's own output format is not a substitute for the pipeline's files.

## Claude Configuration (best for technical writing)

High-level guidance (from Claude docs): be explicit, provide context, and use clear formatting instructions in the system prompt. The "Prompting best practices" page highlights explicit instructions, structured prompts, and using examples to steer output. It also notes that models follow system prompts well, so you can use them to control behavior and formatting.

Source: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices

Recommended Claude setup for technical writing:
- Model: Use a strong reasoning model (e.g., Claude Opus or the latest high-capability model available to you).
- Temperature: 0.2 to 0.4 (lower for precision and consistency) - may not be available on Cursor.
- Top_p: default (unless you have a strong reason to modify).
- Max tokens: high enough for long drafts (e.g., 8k+).
- System prompt: explicit role, output format, evidence requirements, and citation expectations.


# Install Fabric prompt library in Cursor
https://github.com/danielmiessler/Fabric

Normally, when you install Fabric you'll be asked to chose an AI model. If you chose Claude, this will require you to buy some tokens from Anthropic, so that Cursor can talk to Fabric via Fabric's REST API. Without an AI model assigned to Fabric Cursor cannot talk to Fabric. You can buy tokens from Antropic https://platform.claude.com/dashboard

**Some tests**
* Step 1: Start Fabric's REST API Server
```
fabric --serve
```
This should start the server on port 8080 and show Fabric patterns. The problem is that this cannot work in Cursor, as Cursor does not run AI models from localhost.
You can test to see if the server is running by opening a browser at http://localhost:8080/swagger/index.html

* Verify API Endpoints Work
Test that patterns appear as models

curl http://localhost:8080/api/tags# 

Test a chat completion
```
curl -X POST http://localhost:8080/api/chat \  -H "Content-Type: application/json" \  -d '{    "model": "summarize",    "messages": [{"role": "user", "content": "This is a test message"}]  }'
```


There are ways around if you don't want to buy extra tokens and continue to use the normal Cursor AI subscription.

* **Option 1**. Using Ollama, a local open source AI, **does not work** with Cursor (Cursor rejects local models that serve on localhost).
* **Option 2. Expose Prompt Patterns as Commands in Cursor** — works.

Commands are detected by Cursor in **Settings → Rules, Skills and Commands**. 

You can place *commands* also in the *home/.cursor/commands* folder on your desktop's drive.

The Fabric *Prompt Patterns* are found in 
/home/messeru/.config/fabric/patterns

You can create a shortcut for the *Pattern* folders in Fabric and place it in the *home/.cursor/commands* folder

Now you can now send commands in the Claude chat like 
```
/summarize_paper
```
## Example system prompt (technical blog writing)
To do: update this section based on the Fabric prompt library

```
You are a technical writing assistant. Write accurate, concise, and well-structured technical content.
Always distinguish facts from hypotheses or opinions. If you cannot verify a claim, mark it as unverified.
Prefer clear prose; use minimal bullet lists unless they improve clarity.
When summarizing sources, provide links and note what is directly supported.
If asked to make edits, update files instead of just suggesting changes.
```
### Example task prompt (per article)

```
Topic: <topic>
Audience: <level> (e.g., intermediate software engineers)
Purpose: teach, compare, or explain
Constraints: 1,200-2,000 words; include at least 3 sources; avoid jargon
Deliverables:
1. outline.md
2. research_log.md with sources and quotes
3. draft.md
4. claims_checklist.md (claim -> source)
```

## Multi-agent orchestration (roles)

Use agents to parallelize work. A minimal set:
- *Researcher*: collect sources and summarize key points
- *Outliner*: build the structure
- *Drafter*: write the first version
- *Reviewer*: check for clarity, gaps, and logical flow
- *Fact-checker*: verify claims against sources
- *Editor*: final polish and style alignment
- *Human-input*: runs last. Monitors all human prompts in the chat and appends a section to the final deliverable that exposes the methodology used for writing the paper (human intervention throughout the process).

Role prompts for all agents, including Human-input, are in the `prompts/` folder. The Human-input agent uses `human_ai_collaboration.md` and related frameworks to categorize human cognitive contributions. 

### Example orchestration flow

1. Researcher: gather sources from the web and local files, fill `research_log.md`.
2. Outliner: produce `outline.md` based on research notes.
3. Drafter: generate `draft.md`.
4. Reviewer: refine structure and clarity.
5. Fact-checker: complete `claims_checklist.md`, flag any weak or missing evidence.
6. Editor: final polish and style alignment for `publish.md`.
7. Human-input: runs last. Monitors all human prompts in the chat and appends a section to the final deliverable that exposes the methodology used for writing the paper (human intervention throughout the process) in `human_ai_collaboration.md`.



## Research approach (web + local)

Minimum baseline:
- For each claim, require a primary source or authoritative documentation from Zotero or web.
- Store sources in `sources/` with a short summary.
- Keep a "claims checklist" to trace every statement to evidence.

Suggested research process:
1) Collect 10-20 candidate sources.
2) Identify 5-10 that are authoritative and current.
3) Extract key quotes and facts into `research_log.md`.
4) Build outline only after sources are vetted.

## Methodology template (refinable)

You can use this as your starting template and tweak after each post.

### A Scope and success criteria
- Topic statement: ______________________
- Target audience: ______________________
- Success criteria (reader outcomes):
  - ___________________________________
  - ___________________________________
- Constraints (length, tone, complexity): ______________________

### B Research and evidence
- Primary sources (3+):
  1) ______________________
  2) ______________________
  3) ______________________
- Secondary sources (optional):
  1) ______________________
  2) ______________________
- Research notes saved to: `research_log.md`
- Claims checklist saved to: `claims_checklist.md`


### C Publication
- Final edits in `publish.md`
- Title: ______________________
- Summary (2-3 sentences): ______________________
- Keywords/tags: ______________________


