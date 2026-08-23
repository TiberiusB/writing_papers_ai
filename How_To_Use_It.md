# How to start a writing session

1. **Create the post folder**  
   Under `posts/`, create a folder named by date and topic (e.g. `2026-02-12-my-topic`). Ensure the subfolders `sources/web`, `sources/local`, and optionally `notes`, `drafts`, `figures` exist.

   **File structure for your writing project**

    Create a simple folder structure for each article. The following list includes all files and folders that are created or updated during a writing session (by the human or by each agent in the pipeline).

```
writing_papers_ai/
  posts/
    2026-02-07-my-topic/
      sources/              (store source files, PDFs, etc.)
        web/
        local/
      notes/                 (optional working notes)
      drafts/                (optional draft versions)
      figures/               (images, diagrams)
      research_log.md       (Researcher: sources, quotes, CLAIMS CHECKLIST)
      outline.md            (Outliner: structure, three layers, impact/hooks)
      draft.md              (Drafter: first draft; Reviewer: refined draft)
      human_input_log.md    (any role: log of human input per step, when provided)
      claims_checklist.md   (Fact-checker: completed checklist with Verification)
      fact_check_report.md  (Fact-checker: weak/missing evidence, recommendations)
      review_notes.md       (Reviewer: what was checked and changed)
      publish.md            (Editor: final polished article)
      edit_notes.md         (Editor: summary of edits, open questions)
      methodology_and_human_contribution.md  (Human-input: methodology + human vs AI contribution)
```
NOTE: There is a template folder under **posts** named **post_tempalte_file_structure**. This is an empty file structure. Make a copy of it under the same **posts** folder and rename it with the topic of your paper. You'll instruct the AI agents to work within that folder. Once you finish writing you can move that folder in your documents. Best practice is to always leave the template folder there, for later use. Every time you start writing a new paper you'll need to clear all chats, to avoid contamination from one writing session to the next (i.e. start fresh). 

2. **Define topic, audience, and purpose**  
   In a short brief (or in the first chat), set: topic, target audience, purpose (e.g. teach, compare, explain), and constraints (length, tone, number of sources).

3. **Run the pipeline in order**  
   - Open a chat, pin the **Researcher** prompt (from `prompts/researcher_prompt.md`), and provide the topic/audience/constraints. Ask for output in `research_log.md`. When you give input, the agent will append to `human_input_log.md`.  
   - Then **Outliner**: provide `@research_log.md`, pin the Outliner prompt, ask for `outline.md`.  
   - Then **Drafter**: provide `@outline.md` and `@research_log.md`, pin the Drafter prompt, ask for `draft.md`.  
   - Then **Reviewer**, **Fact-checker**, **Editor**, each with their prompts and the relevant files.  
   - Finally **Human-input**: provide `human_input_log.md` and (if available) `publish.md`, `review_notes.md`, `fact_check_report.md`, `edit_notes.md`; ask for `methodology_and_human_contribution.md` and optionally a short methodology section for the deliverable.

4. **Consolidate**  
   Copy or save each agent’s output into the corresponding file in the post folder. Use `publish.md` as the final article and keep `methodology_and_human_contribution.md` and `human_input_log.md` for transparency and feedback (to humans).

5. **Optional**  
   Use the Methodology template (Section G in the Manual below) to plan scope, sources, and retrospective, and run a lightweight retrospective after publishing.

## Using MCP servers (Zotero, Google Drive, MediaWiki)

- **Zotero MCP server**:  
  Use this mainly in the **Researcher** and **Fact-checker** steps.
  - In a Researcher chat, you can say things like:  
    “Search my Zotero library for recent work on networked organizations” or  
    “List key papers tagged `commons` in the Sensorica group library.”  
  - As long as:
    - Zotero is running when you use the MCP server, and
    - Cursor has the Zotero MCP server configured and enabled,  
    you should be able to search your Zotero library and retrieve items from Cursor.
  - To confirm that it works, try a prompt like: “Search my Zotero library for articles about commons-based peer production” and then ask the agent to add the most relevant items (with citation info) into `research_log.md`.

- **Google Drive MCP server (piotr-agier)**:  
  Use this when important source material (notes, PDFs, Docs, Sheets, Slides) lives in Google Drive. The server supports search, read, download, and folder navigation ([piotr-agier/google-drive-mcp](https://github.com/piotr-agier/google-drive-mcp)).
  - In the Researcher chat, you might ask:  
    “Search my Google Drive for PDFs about sensor networks and list the most relevant ones” or  
    “Open the Google Doc ‘Sensorica governance notes’ and summarize the key points into `research_log.md`.”
  - For data-heavy pieces, you can ask the agent to read a Google Sheet (e.g. `getGoogleSheetContent`) and extract the rows/columns you need into a local markdown table in `research_log.md` or a file under `sources/`. You can also use `readGoogleDoc` (format `markdown` or `text`) and `downloadFile` to pull content into your post folder.

- **MediaWiki MCP server (ProfessionalWiki)**:  
  Use this when part of your knowledge base is in a MediaWiki wiki (for example an organizational wiki or Wikipedia).
  - In the Researcher chat, you can say:  
    “Search the wiki for pages about commons-based peer production and summarize the main concepts” or  
    “Get the page ‘Peer-to-peer_economics’ and extract definitions and key arguments into `research_log.md`.”
  - Because the MediaWiki MCP server exposes tools like `get-page`, `search-page`, and `get-category-members`, you can ask the agent to systematically explore categories (e.g. all pages in a “Governance” category) and turn that into structured notes that feed the outline and draft.

- **docs-mcp-server (arabold)**:  
  Use this when a post discusses a specific software library, framework, protocol, or API and you want claims grounded in the actual current documentation rather than the model's training memory.
  - In the Researcher chat, you might ask:  
    “Index the documentation for [library] at [docs URL] as library '[name]' version '[x.y]'” (this calls `scrape_docs`), then  
    “Search the '[name]' docs for [topic] and add the relevant facts and exact wording to `research_log.md` with the doc URL as the source.”
  - In the Fact-checker chat, if a claim in `draft.md` concerns that library's behavior, ask the agent to `search_docs` for the relevant section and confirm the claim matches the indexed documentation before marking it Verified.
  - Use `fetch_url` for a quick one-off page-to-Markdown conversion when you don't need a full library index, and `list_libraries` / `find_version` to check what is already indexed before re-scraping.

## Using LifeOS skills (optional, personal)

If you have LifeOS installed globally in Cursor (see `How_To_Set_Up.md`), you have access to a personal skill library (`~/.cursor/skills/`) in addition to this project's own prompts and rules. This is optional, per-user tooling: nothing in this pipeline requires it, and posts written without it work exactly the same way.

Where it can help, and how to keep the pipeline coherent:

- **Research skill**: Runs a multi-agent, URL-verified web research workflow (it auto-triggers on the word "research"). In the Researcher chat, you can lean into this deliberately, e.g. “Use the Research skill to do extensive research on [topic], then fold the verified findings into `research_log.md` in the usual SOURCES / CLAIMS CHECKLIST format.” Always ask explicitly for the second step: the skill's own report is not a substitute for `research_log.md`, and downstream agents (Outliner, Drafter, Fact-checker) only read the project's files.
- **ArXiv skill**: Useful in the Researcher step for finding and retrieving academic papers on a topic, in the same way as Zotero searches; extract quotes and citations into `research_log.md` as usual.
- **BiasCheck / DetectAI skills**: Can be run as an extra pass during Review or Editing (e.g. “Run BiasCheck on `draft.md`”) to surface framing issues or AI-writing tells before publication; record any resulting changes in `edit_notes.md` or `review_notes.md` as you would for any other review finding.
- **Precedence**: This project's role prompts (`prompts/`) and `.cursor/rules/writing-methodology.mdc` always win over a global skill's own format or workflow when you are working inside `writing_papers_ai/`. If a global skill's auto-trigger produces output in its own format, treat it as raw input to be transcribed into the pipeline's files (with evidence traceability preserved), not as a final deliverable.
