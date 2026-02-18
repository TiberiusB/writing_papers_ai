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


So as long as:
* Zotero is running when you use the MCP server, and
* Cursor has the Zotero MCP server configured and enabled,

You should be able to search your Zotero library and retrieve items from Cursor. If you’ve already added the MCP server in Cursor, try a chat prompt like “Search my Zotero library for …” to confirm it works.

