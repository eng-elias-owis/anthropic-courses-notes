# Introduction to Claude Cowork — Course Notes

> Source: https://anthropic.skilljar.com/introduction-to-claude-cowork
> Extracted: March 2026 (real content from actual lessons)

## Course Overview

A 10-lesson practical course (+ 1 quiz) covering Claude Cowork — an agentic task execution layer built into Claude Desktop. Cowork shifts the model from *conversation* to *delegation*: you describe an outcome, Claude plans and executes it, and delivers a finished file to your drive. The course covers setup, the core task loop, persistent context via projects, plugins/skills, scheduled automation, file tasks, research at scale, permissions, usage management, and troubleshooting.

---

## Lesson Notes

### Lesson 1: What is Cowork?

**Key Concepts:**
- Cowork is built on the same architecture as Claude Code — now applied to knowledge work (analysis, research, writing, documents)
- The core shift: **conversation → delegation**. You describe an outcome; Claude plans, executes, and delivers a finished file
- Three enabling pillars: **Plan** (Claude shows approach before starting), **Execute** (runs in isolated environment on your machine), **Connect** (reaches email, shared drives, tools you're already signed into)
- Core capabilities: Connectors (email, Slack, Drive, etc.), File operations (read/write/create real files), Plugins (domain expertise bundles), Scheduled tasks (recurring workflows), Subagents (parallel workstreams), Local computation (run code on files in place)
- Cowork stores outputs as real files on your drive — not text to paste somewhere else

**Practical Tips:**
- Reach for Cowork when you want a finished file, the work touches files/connected tools, there are many steps, or you want to let it run while you do something else
- Stick to chat when you want an answer or draft to refine yourself, everything fits in a single paste, or you want to think through something turn-by-turn
- Use the test: *"Does this task need to touch my files, my connected tools, or produce a real output file?"* — if yes, Cowork fits

---

### Lesson 2: Getting Set Up

**Key Concepts:**
- Cowork runs in **Claude Desktop** on paid plans; access via the mode selector (Chat / Cowork / Code)
- **Working folder**: give Claude a directory — it can read, create, and edit files there directly (no uploading/downloading); always asks permission before making changes
- **Connectors**: link to Slack, Google Drive, Gmail, Calendar, and others — set up once and reference naturally in prompts
- **Claude in Chrome extension**: lets Cowork reach pages without a connector (internal dashboards, web apps behind logins) — grants permissions per site
- **Projects**: wrap a folder with persistent instructions and memory that carry across every session

**Practical Tips:**
- Point Cowork at a dedicated working folder for each area of work
- Set up connectors once upfront so you can reference tools naturally: "check what the team said in Slack" instead of manually copying threads
- Install Claude in Chrome for pages that don't have a connector (portals, internal dashboards)

---

### Lesson 3: The Task Loop

**Key Concepts:**
- The core loop: **Describe → Claude proposes plan → You review/approve → Claude executes → Open finished file**
- Claude asks clarifying questions for anything left out of the prompt; you pick from offered options or type your own answer
- **Progress panel** shows each step: which files are being read, what's being built; large tasks are broken into parallel parts
- **Subagents**: for tasks with many independent pieces, Cowork spins up separate workstreams running simultaneously, each with its own focused context
- **Isolated environment**: code runs and files are built separate from your main system; deletion is always gated — Cowork asks before permanently deleting anything
- Output lands in the folder you pointed Cowork at (or in Gmail/Drive if you specified)

**Practical Tips:**
- Treat Cowork output as a first draft from a capable colleague — good work that's still yours to shape before sending
- You can steer while it runs — type in the chat to redirect without waiting for it to finish
- Start with tasks that have clear boundaries (organizing a folder, synthesizing a set of documents); build intuition before scaling up
- A strong prompt gives Claude three things: *what to look at*, *what you want back*, and *where it should go*

---

### Lesson 4: Giving Cowork Context

**Key Concepts:**
- Each Cowork task starts fresh — Claude doesn't carry conversation memory between tasks
- **Projects** are how context persists: a named workspace backed by a real folder, with instructions and memory that carry into every task
- Cowork project memory is scoped *inside that project* (unlike Chat memory, which applies across all conversations)
- Projects can be created from scratch, imported from a Chat project (one-way sync), or wrapped around an existing folder
- **Instructions panel** (per project): read every time a task starts from that project — use it for names/roles, file locations, output preferences, project-specific rules
- **Global instructions** (Settings → Cowork → Global Instructions): apply to every task everywhere — use for role, default formats, standing rules like "ask before deleting"

**Practical Tips:**
- Instructions don't need to be polished — a few lines is enough to make shorthand work ("send this to Rachel", "file it where the Q3 report went")
- Put evolving context (running notes, glossaries) in a file inside the project folder — Claude reads everything there
- Test what Cowork knows by asking: *"Tell me what you know about how I work here"* — reveals gaps in Instructions
- For a new project, add at minimum: key people + roles, where related files live, one output preference

---

### Lesson 5: Plugins — Cowork as a Specialist

**Key Concepts:**
- **Plugins** give Cowork domain expertise — bundled knowledge and workflows for a specific role/function
- A plugin is a folder containing: **Skills** (markdown files with workflow instructions), **Connectors** (system integrations), **Subagents** (specialized parallel workers)
- Skills can be invoked with `/skill-name` in the prompt or drawn on automatically
- Open-source plugins exist for: sales, marketing, product, finance, legal, operations, customer support, data, and more
- Skills work across all Claude surfaces (chat, Claude Code, Cowork) — a plugin is the Cowork-specific way of bundling skills with connectors
- Everything inside a plugin is plain text / markdown — no build step, editable directly

**Practical Tips:**
- Browse plugins in Cowork's Customize area; install with one click — active immediately
- After installing, open the plugin folder and read a skill file to understand what it actually does (it's written like a brief to a teammate)
- To customize a skill, open its file and edit it; to add a new skill, add a folder under `skills/`
- Find plugins at: [github.com/anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins) and financial-services plugins on GitHub

---

### Lesson 6: Scheduled Tasks

**Key Concepts:**
- Scheduled tasks run any Cowork task automatically on a cadence: hourly, daily, weekly, or custom
- The task can be a typed prompt, a plugin skill, or any refined workflow
- Set up via `/schedule` in any Cowork conversation, or from the scheduled tasks area in the sidebar
- There's an **approval step** before it starts running repeatedly
- Runs while the desktop app is open; if app was closed or computer was asleep, runs as soon as you're back (with a delay notification)
- Scheduled tasks created inside a project appear alongside that project's other scheduled work
- Can manage from the scheduled tasks area: review past runs, edit instructions/cadence, pause, or trigger on demand

**Practical Tips:**
- Pattern: **skills encode *what* to do; scheduled tasks decide *when*** — e.g., a briefing skill scheduled for 8am weekdays means the briefing is always waiting
- You don't need a skill to schedule something — any prompt that works well is a candidate
- Before scheduling, run the task once manually; once it works well, scheduling is one more step
- Think of recurring manual work first (weekly status pulls, daily folder checks, recurring reports) as scheduling candidates

---

### Lesson 7: File & Document Tasks

**Key Concepts:**
- Cowork produces **real, native files**: PowerPoint with editable charts, spreadsheets with working formulas, Word docs with track changes — saved to your drive
- Charts/elements in Cowork-made files are fully editable (same as files made by hand)
- Brand and template rules **compound**: once you've made a brand-guidelines skill, every file Cowork produces can reference it
- Prompt pattern for file tasks: **Input** (where source material is) + **Transformation** (what to do with it) + **Output** (what format to produce)

**Practical Tips:**
- "Clean up my files" → vague, gets a clarifying question. "Sort my Downloads by file type into dated subfolders" → concrete, gets work done
- Template prompts to adapt: organize files by type/date, rename with consistent format, build tracking spreadsheets from notes, combine transcripts into formatted reports, turn meeting notes into branded slide decks
- Always specify input location, the transformation, and the output format in your prompt
- Browse the Cowork use-case gallery for more task ideas

---

### Lesson 8: Research & Analysis at Scale

**Key Concepts:**
- Chat fits research when material fits in one conversation; Cowork fits when work has **volume** (too many/large files), **parallelism** (same analysis across N items), or **in-place computation** (run code on files where they live, write results back)
- **Research synthesis**: Cowork holds all sources at once and finds cross-references/contradictions you'd miss reading sequentially
- **Transcript analysis**: finds patterns, agreements, and *disagreements* across large batches; parallel reading reveals who the outliers are and what they have in common
- **Data analysis**: statistical work on files on your drive — outlier detection, time-series, cross-tabulation; results written back in place
- **Knowledge synthesis**: point at your own notes/journals/project files to surface patterns, recurring questions, or contradictions between decisions

**Practical Tips:**
- The most useful prompts aren't "summarize everything" — they're the sharp question you'd answer yourself if you had time
- Sharpen prompts: "Summarize these transcripts" → "What did most people agree on? Who disagreed, and what do they have in common?"
- "Analyze this data" → "Which accounts are at risk based on the last three months? What's the common pattern?"
- "Review these papers" → "Where do these papers contradict each other? Which claims need the most caveats?"
- Ask for the **signal**: outliers, contradictions, what would change your decision

---

### Lesson 9: Permissions, Usage, & Choosing Your Model

**Key Concepts:**
- Safety boundaries: isolated execution (separate from OS), controlled folder access (no grant = no access), network policies respected, deletion always gated with explicit approval
- Cowork conversation history is stored **locally on your machine** — check plan docs for audit logging / compliance features if needed
- Cowork uses **more allocation than chat** — multi-step, long-running work is more compute-intensive
- **Model tiers**: Opus (most capable, most compute — complex multi-step), Sonnet (sensible default for everyday tasks), Haiku (quickest and lightest)
- Reviewing outputs is a critical habit: Cowork's confidence is the same whether the work is right or wrong — your review catches the difference

**Practical Tips:**
- Batch related work into one session — starting fresh has overhead; multiple related tasks in one session is more efficient
- Use chat for tasks that don't need files, connected tools, or a real output file — it's faster and less resource-intensive
- Match model to task: don't default to Opus for everything; use Haiku or Sonnet for simpler tasks
- Build the habit: before sending a Cowork output onward, **open the file, check a number, follow one thread of reasoning**
- Monitor your usage in Cowork settings, especially while building habits

---

### Lesson 10: Troubleshooting & Next Steps

**Key Concepts:**
- Common issues: "preparing workspace" delay (normal, especially first time/after update), task stopped mid-run (usually app was quit — sleeping computer is fine, quitting pauses the task), usage limits hit, output file not found (check working folder)
- If file is missing: check working folder Cowork was using, verify folder access in settings, ask Cowork directly where it wrote the file
- Recommended progression: Install plugin → Run something real → Make a skill → Schedule it → Share it

**Practical Tips:**
- Sleeping the computer is fine — Cowork sessions survive and resume on wake; *quitting the app* pauses the task
- If you hit usage limits: batch related work, use chat for tasks that fit, match model to task needs
- The goal sequence: have a skill you trust running on a schedule
- Next time you run a task you'll run again, save that session as a skill
- On Team/Enterprise plans, talk to your admin about making skills available more broadly
- For help: [Getting started with Cowork](https://support.anthropic.com), [Cowork use-case gallery](https://anthropic.com), [github.com/anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins), [AI Fluency course](https://anthropic.skilljar.com)

---

### Lesson 11: Quiz on Claude Cowork

*Quiz covering course concepts — 8 questions. Key topics tested:*
- What subagents enable (parallel workstreams with independent contexts)
- How Cowork project memory differs from Chat memory (scoped to the project vs. all conversations)
- Relationship between skills and plugins (skills work everywhere; plugins bundle skills + connectors for a role in Cowork)
- What happens when a scheduled task was due while computer was asleep (runs as soon as you're back, with delay notification)
- How context carries between tasks (via projects + global instructions)

---

## Top 15 Practical Tips (Consolidated)

1. **Use the "does it need files/tools/output?" test** — if yes, use Cowork; if no, chat is faster and cheaper (Lessons 1, 9)
2. **Write prompts with 3 parts: Input + Transformation + Output** — "Sort my Downloads by file type into dated subfolders" beats "clean up my files" (Lesson 7)
3. **Ask for the signal, not a summary** — "What did most people agree on? Who disagreed?" beats "summarize these transcripts" (Lesson 8)
4. **Treat all Cowork output as a first draft** — open the file, check a number, follow a thread of reasoning before sending it onward (Lessons 3, 9)
5. **Set up connectors once upfront** — reference tools naturally in prompts instead of manually copying context (Lesson 2)
6. **Use the Instructions panel for shorthand** — a few lines on who's who and where things live makes "send this to Rachel" and "file it where the Q3 report went" work automatically (Lesson 4)
7. **Test context with a diagnostic prompt** — ask "Tell me what you know about how I work here" to surface gaps in your Instructions (Lesson 4)
8. **Start small and build intuition** — begin with tasks that have clear boundaries (organizing a folder, synthesizing documents) before scaling to complex workflows (Lesson 3)
9. **You can steer while it runs** — type in the chat to redirect without waiting for the task to finish (Lesson 3)
10. **Skills + scheduled tasks = automation** — a skill encodes *what* to do; a scheduled task decides *when*; together they eliminate recurring manual work (Lessons 5, 6)
11. **Batch related work into one session** — starting fresh has overhead; doing several related tasks together is more allocation-efficient (Lesson 9)
12. **Match model to task complexity** — use Haiku or Sonnet for everyday tasks; reserve Opus for genuinely complex multi-step work (Lesson 9)
13. **Sleep, don't quit** — sleeping the computer is fine (session resumes on wake); quitting the Claude Desktop app pauses the task mid-run (Lesson 10)
14. **Read a plugin's skill files before relying on it** — they're plain text written like a brief to a teammate; reading one tells you exactly what it will do (Lesson 5)
15. **Follow the goal sequence: Install plugin → Run real task → Make a skill → Schedule it → Share it** — the sequence matters more than the speed (Lesson 10)
