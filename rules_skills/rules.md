SProposing three rules: one shared methodology rule and two file-scoped rules for the most evidence-sensitive artifacts.

---

## Proposed Rules

### Rule 1: Shared methodology (always apply)
**File:** `.cursor/rules/writing-methodology.mdc`  
**Scope:** `alwaysApply: true` (when working in this project)

Core conventions that apply across all agents, without duplicating the role prompts.

---

### Rule 2: Research log format
**File:** `.cursor/rules/research-log-format.mdc`  
**Scope:** `globs: **/research_log.md`

Applies when working on `research_log.md`. Keeps the Researcher’s output structured and usable by downstream agents.

---

### Rule 3: Draft evidence
**File:** `.cursor/rules/draft-evidence.mdc`  
**Scope:** `globs: **/draft.md`

Applies when working on `draft.md`. Enforces evidence discipline for the Drafter and others who edit the draft.

---

## Draft content for each rule

---

### Rule 1: `writing-methodology.mdc`

```markdown
---
description: Core conventions for the multi-agent writing methodology
alwaysApply: true
---

# Writing Methodology Conventions

- **Evidence traceability**: Every substantive claim must be traceable to `research_log.md` (SOURCES and CLAIMS CHECKLIST). Do not invent facts, quotes, or claims.
- **UNVERIFIED**: When a claim cannot be supported by available sources, mark it as UNVERIFIED in the claims checklist or flag it in the draft (e.g., "Evidence is limited" or "This remains an open question").
- **human_input_log.md**: When the human provides input during a step (topic, feedback, constraints), append to `human_input_log.md` with: Step, Human input (summary or quote), Cognitive contribution type (Goal-setting / Ethical constraint / Reframing / Selection / Correction / Synthesis / Evaluative override / Other).
- **File structure**: Work within `posts/YYYY-MM-DD-topic/` folders. Key files: `research_log.md`, `outline.md`, `draft.md`, `claims_checklist.md`, `fact_check_report.md`, `review_notes.md`, `edit_notes.md`, `publish.md`, `human_input_log.md`, `methodology_and_human_contribution.md`.
- **Formatting**: Use plain Markdown (headers, paragraphs). Avoid bold or italics (asterisks are difficult in plaintext). Blank line between paragraphs and sections.
```

---

### Rule 2: `research-log-format.mdc`

```markdown
---
description: Format expectations when editing research_log.md
globs: "**/research_log.md"
alwaysApply: false
---

# Research Log Format

- **SOURCES**: Each source must have: Author/Organization, URL, Date, Type, Authority Level (Primary/Secondary/Tertiary), Summary, Key Quotes (exact wording with page/section if available), Key Facts/Data (with source attribution).
- **CLAIMS CHECKLIST**: Table with columns: Claim | Supporting Source(s) | Evidence Type | Notes. Map each significant claim to at least one source. Mark unsupported claims as UNVERIFIED.
- **Quotes**: Use exact wording. Include page numbers, section refs, or timestamps when available.
- **Diversity**: Prefer diverse sources (not all from same author/organization). Prioritize primary over secondary sources.
```

---

### Rule 3: `draft-evidence.mdc`

```markdown
---
description: Evidence discipline when editing draft.md
globs: "**/draft.md"
alwaysApply: false
---

# Draft Evidence Rules

- **Single source of truth**: Use only evidence from `research_log.md`. Do not add claims, quotes, or data not documented there.
- **Attribution**: For every quote or fact, add attribution (e.g., "According to Source 2 …" or "[Author, Title]").
- **UNVERIFIED and gaps**: If a claim is UNVERIFIED or tied to a research gap, phrase it tentatively (e.g., "One open question is …", "Evidence here is limited …").
- **Section alignment**: Section titles should match `outline.md` so the Fact-checker can map draft sections back to the CLAIMS CHECKLIST.
```

---

## Summary

| Rule | Purpose | Scope |
|------|---------|-------|
| **writing-methodology** | Shared conventions: evidence, UNVERIFIED, human_input_log, file structure | Always |
| **research-log-format** | Research log structure and CLAIMS CHECKLIST format | When editing `research_log.md` |
| **draft-evidence** | Evidence traceability and attribution in drafts | When editing `draft.md` |

These rules stay short and focused, and they reinforce the parts of the workflow where traceability and consistency matter most.

To add them, create the `.mdc` files in `.cursor/rules/` under your writing project (or user-level) and paste the content above. If you want to implement them via Agent mode, I can create the files for you.



# More about rules

Here’s how Cursor rules relate to our setup and whether we need them.

---

## How Cursor rules work

**Rules** live in `.cursor/rules/` as `.mdc` files with YAML frontmatter:

- **Always-apply rules** (`alwaysApply: true`) — used in every conversation.
- **File-scoped rules** (`globs: **/*.ts`) — used only when matching files are open.
- **Short** — typically under 50 lines, one concern per rule, with concrete examples.

They act as persistent context: when Cursor loads a conversation, it pulls in rules (and other context) so the AI gets your conventions without you repeating them.

---

## Rules vs our role prompts

| Aspect | Role prompts (`prompts/`) | Cursor rules (`.cursor/rules/`) |
|--------|---------------------------|----------------------------------|
| **When loaded** | Only when you explicitly pin/copy them into a chat | Automatically when a conversation starts (if they apply) |
| **Scope** | One per agent role; you switch them manually | Per-file or always-on; Cursor decides based on context |
| **Length** | Long, full instructions (e.g. Researcher ~150 lines) | Short, focused constraints (target: <50 lines) |
| **Purpose** | Define identity, goals, steps, output format | Enforce conventions, standards, file patterns |

Our
---

## Do you need rules?

You don’t strictly need rules for our pipeline to work. Role prompts already define each agent well.

Rules can still help by:

1. **Enforcing shared conventions** — Same standards across all agents without repeating them in every prompt (e.g. how to treat sources, how to mark unverified claims).
2. **Scoping context** — When you’re in a `research_log.md` or `outline.md`, a file-scoped rule can remind the model to stay in that agent’s lane or use that file’s format.
3. **Reducing prompt length** — Move generic rules (e.g. “no bold/italics”) into a shared rule and keep role prompts leaner.
4. **Consistency across chats** — If you forget to pin a prompt or switch chats, always-apply rules still apply.

---

## Do you need rules for every agent?

No. Better approach:

- **One shared rule** for the methodology (e.g. always-on in the writing project) with:
  - File structure expectations
  - Citation / evidence rules
  - How to treat UNVERIFIED claims
  - `human_input_log.md` conventions

- **Optional file-scoped rules** for specific artifacts:
  - `**/research_log.md` — must follow SOURCES / CLAIMS CHECKLIST layout
  - `**/outline.md` — must use Thematic / Pragmatic / Logical-Emotional structure
  - `**/draft.md` — only evidence from `research_log.md`

You don’t need 7 separate rules (one per agent). Agents are selected by which prompt you pin; rules should describe *what* good outputs look like and *how* to handle evidence, not duplicate role identity and workflows.

---

## Summary

| Question | Answer |
|----------|--------|
| Need rules? | Optional; role prompts are enough to run the pipeline. Rules improve consistency and enforce shared standards. |
| Rules for every agent? | No. Prefer 1 shared rule and a few file-scoped rules for key outputs. |
| Main benefit | Rules apply automatically and can reduce repetition in prompts and enforce project-wide conventions. |

A minimal setup would be one always-apply rule for “writing methodology conventions” plus one or two file-scoped rules for `research_log.md` and `draft.md` if you want tighter format enforcement when working on those files.