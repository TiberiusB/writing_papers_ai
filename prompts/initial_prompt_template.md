# Initial Prompt Template

Use this template at the start of a new writing exercise. Copy and fill in the bracketed sections, then paste into a new Cursor chat to begin. The methodology is described in @writing_papers_ai/README.md and @How_To_Use_It.md

---

## 1. What to do

We are going to write [a blog post / an article] using the multi-agent writing methodology.

Execute the first stage (Researcher) using @writing_papers_ai/prompts/researcher_prompt.md as prompt. Work within the post folder `posts/[YYYY-MM-DD-topic]/`. When done, stop and invite me to review your deliverables before moving to the next stage.

---

## 2. Topic and purpose

**Topic**: [Short description of the article topic]

**Scope**: [What to cover, e.g. "Short historical overview; evolution; key milestones and inflection points"]

**Purpose**: [What the reader should get, e.g. "Inform, inspire trust, encourage joining or adopting the model"]

---

## 3. Audience and tone

**Audience**: [Who will read this, e.g. "People interested in X, Y, Z; minimum education level"]

**Tone**: [e.g. "Informative, narrative, somewhat entertaining. Show X, Y. Avoid Z."]

---

## 4. Materials (sources)

Specify each source location with full path. Distinguish clearly between Zotero Group Libraries and Zotero My Library (they are different; e.g. "In the media" may be in My Library, not in a Group).

- **Websites**: [URLs and what each is for]
- **Zotero Group Library "[Name]"**, Collection **"[Name]"**: [What it contains, how to use it]
- **Zotero My Library** (not Group Library), Collection **"[Name]"**, sub-Collection **"[Name]"**: [What it contains. Must be consulted if listed.]

Add other sources as needed. Do not skip sub-collections; consult all listed materials.

---

## 5. Scope and constraints

- **Word count**: [e.g. 2000–4000 words]
- **Source coverage**: [Consult ALL documents in listed materials / Consult a representative sample] — specify which.
- **Retained sources**: Keep and document at least [X] sources in research_log.md. [Do not cap at 10 unless you explicitly want a short list.]
- **Evidence**: Every substantive claim must be traceable to research_log.md. Mark UNVERIFIED when evidence is missing.

---

## 6. Known facts and terminology (fill in to reduce corrections)

Provide upfront any domain expertise that Researcher, Outliner, and Drafter should respect. These will be included in research_log.md and outline.md so they propagate; do not rely on human_input_log alone.

**Key historical facts or framing**:
- [e.g. "X was proposed, Y was chosen instead. Z became the exchange firm."]
- [Add as needed]

**Preferred terminology**:
- [e.g. Use "benefits" not "value" in contribution accounting]
- [List handling: full list vs abbreviated, e.g. "Prefer abbreviated: A, B, C, etc."]

**Quote interpretations** (if key quotes will appear):
- "[Exact quote]": [Your interpretation so the Drafter uses it correctly]

**Exclusions**:
- [e.g. Do not include X in recent activity listings; do not mention Y]

Leave blank if not applicable. The more you provide here, the fewer corrections you will need later.

---

## 7. Workflow checkpoints

- **After Researcher**: Stop. Human reviews research_log.md and human_input_log.md before proceeding.
- **After Outliner**: Stop. Human MUST review outline.md section "Open Questions and Gaps" and provide answers in chat before Drafter runs. Do not proceed to Drafter until gaps are filled.
- **After Drafter**: Human may correct facts or framing; log in human_input_log.md and propagate to outline.md or research_log.md so downstream agents see the updates.
- **During Fact-checker**: If claims lack sources, human may supply URLs. Add to research_log.md or claims_checklist.md as appropriate.

---

## 8. Pipeline (run after each checkpoint)

1. **Outliner**: Use `prompts/outliner_prompt.md`. Input: `@research_log.md`. Output: `outline.md`.
2. **Drafter**: Use `prompts/drafter_prompt.md`. Input: `@outline.md`, `@research_log.md`. Output: `draft.md`.
3. **Reviewer**: Use `prompts/reviewer_prompt.md`. Input: `@draft.md`, `@outline.md`, `@research_log.md`. Output: refined `draft.md`, `review_notes.md`.
4. **Fact-checker**: Use `prompts/fact_checker_prompt.md`. Input: `@draft.md`, `@research_log.md`, `@outline.md`. Output: `claims_checklist.md`, `fact_check_report.md`.
5. **Editor**: Use `prompts/editor_prompt.md`. Input: `@draft.md`, `@outline.md`, `@research_log.md`, `@claims_checklist.md`, `@review_notes.md`, `@fact_check_report.md`. Output: `publish.md`, `edit_notes.md`.
6. **Human-input**: Use `prompts/human_input_prompt.md`. Input: `@human_input_log.md`, `@publish.md`, `@review_notes.md`, `@fact_check_report.md`, `@edit_notes.md`. Output: `methodology_and_human_contribution.md`.

---

## 9. File structure (ensure post folder exists)

```
posts/YYYY-MM-DD-topic/
  sources/
    web/
    local/
  research_log.md       (Researcher)
  outline.md            (Outliner)
  draft.md              (Drafter, Reviewer)
  human_input_log.md    (any role, when human provides input)
  claims_checklist.md   (Fact-checker)
  fact_check_report.md  (Fact-checker)
  review_notes.md       (Reviewer)
  publish.md            (Editor)
  edit_notes.md         (Editor)
  methodology_and_human_contribution.md  (Human-input)
```

Create the folder and `human_input_log.md` (with heading "# Human input log") if they do not exist. Create `sources/web` and `sources/local` subfolders if needed.

---

## 10. Rules

- Each agent appends to `human_input_log.md` when the human provides input during that step.
- Use only evidence from `research_log.md`; no invented facts. Mark UNVERIFIED when evidence is insufficient.
- Plain Markdown; avoid bold or italics in draft body.
- When human provides corrections or gap completions, the active agent must update research_log.md and/or outline.md so downstream agents receive the corrected information. Do not leave corrections only in human_input_log.md.
- Start fresh: clear chats when beginning a new paper to avoid contamination.

---

## 11. How to use this template

1. Copy this template.
2. Fill in sections 2–6 (Topic, Audience, Materials, Constraints, Known facts).
3. Create the post folder under `posts/` (e.g. `posts/2026-02-14-my-topic/`).
4. Paste the filled template into a new Cursor chat.
5. Run Researcher; stop for human review.
6. Run Outliner; stop; human fills "Open Questions and Gaps" in chat.
7. Run Drafter, Reviewer, Fact-checker, Editor, Human-input in order.
8. Supply missing sources during Fact-checker if needed.
