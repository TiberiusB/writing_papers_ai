
Here’s how Cursor Skills work and whether you need them.

---

## What Cursor Skills Are

**Skills** are instruction files (in `SKILL.md`) that teach the AI how to do specific tasks. They are:

- **Task-focused** — e.g. “Review PRs with team standards”, “Generate commit messages”, “Process PDFs”
- **Stored as directories** — `skill-name/SKILL.md` plus optional helpers
- **Auto-selected** — the model uses a skill when its description matches the user’s request
- **Location** — personal (`~/.cursor/skills/`) or project (`.cursor/skills/`)

Each skill needs a **description** that clearly says what it does and when to use it. That description is how the agent decides to load the skill.

---

## Skills vs Rules

| Aspect | Rules | Skills |
|--------|-------|--------|
| **Trigger** | File patterns (`globs`) or always-on | Semantic match on user request |
| **Purpose** | Conventions and constraints | Step-by-step task instructions |
| **Structure** | Single `.mdc` file | Directory with `SKILL.md` and optional files |
| **Length** | Short (target ~50 lines) | Can be longer (up to ~500 lines) |
| **Example** | “Don’t invent claims” | “How to run this workflow and what to produce” |

Rules set **what to avoid and what to enforce**. Skills describe **how to do a specific task**.

---

## Do You Need to Set Up Skills?

No. Skills are optional. Cursor works fine without them.

You already have:

- **Role prompts** — per-agent identity, goals, and output format  
- **Rules** — methodology conventions and format expectations  

Together, they define your pipeline. You don’t need skills for it to function.

---

## When Skills Are Useful

Consider skills when:

1. **Workflow complexity** — A multi-step task (e.g. “run Researcher → Outliner → Drafter”) that needs structured guidance.
2. **Team sharing** — You want project-level instructions in `.cursor/skills/` that everyone gets.
3. **Reusable tasks** — You do the same task often across projects (e.g. Zotero + research workflow).
4. **Extra detail** — You want long checklists, templates, or scripts beyond what fits in a prompt.

---

## For Your Writing Project

You **don’t need** skills right now. The combination of:

- Role prompts in `prompts/`
- Rules in `.cursor/rules/`
- Your pinned prompts in each chat  

is sufficient for the pipeline to run.

You might add a skill later if:

- You build a “Manager” agent that coordinates all roles.
- You want a single skill like “Run full writing pipeline” with a step-by-step guide.
- You formalize a “Research with Zotero” workflow for reuse in other projects.

---

## Summary

| Question | Answer |
|----------|--------|
| **What are Skills?** | Instructions that teach the agent how to do specific tasks; loaded when the request matches the skill’s description. |
| **Must you set them up?** | No. They’re optional. |
| **Why use them?** | To define complex workflows, share instructions with a team, or reuse detailed task instructions across projects. |
| **For your writing pipeline?** | Not necessary yet. Prompts + rules are enough. You can add skills later if a workflow becomes complex or reusable. |

---

## Global skill libraries (e.g. LifeOS) and this project

Skills are not only project-level. Some contributors also have a personal, global skill library installed for Cursor (for example, LifeOS at `~/.cursor/skills/`), which is auto-discovered across every project and every chat, not just this one. This matters for our pipeline in two ways:

1. **Auto-triggering.** A global skill can trigger on a keyword in your message even when you only meant to run one of our role prompts. The clearest example is a "Research" skill that triggers on the word "research" and runs its own multi-agent workflow with its own report format.
2. **Precedence.** Inside `writing_papers_ai/`, this project's role prompts (`prompts/`) and rules (`.cursor/rules/writing-methodology.mdc`) define what "done" means for each step (which files get produced, in which format, with evidence traced to `research_log.md`). A global skill's own output format never replaces that. If you want to use a global skill as a helper (e.g. LifeOS's Research skill to widen the Researcher's search, or BiasCheck during Review), ask for it explicitly and then ask the agent to fold the results into the pipeline's files, rather than letting the skill's own report stand in as a deliverable.

You still don't need a global skill library to run this methodology; role prompts and rules remain sufficient on their own. See `How_To_Set_Up.md` and `How_To_Use_It.md` for setup and usage details if you do have one installed.