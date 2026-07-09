# Introduction to Claude Cowork — Course Notes

> **Source:** https://anthropic.skilljar.com/introduction-to-claude-cowork
> **Extracted:** July 2026
> **Author:** Anthropic Academy
> **Estimated Time:** ~60 minutes total (10 lessons + 1 quiz)
> **Lessons:** 11 (10 instructional + 1 quiz)
> **Format:** Written lessons + short videos + an end-of-course knowledge check

---

## 📚 Course Overview

This course teaches you how to use **Claude Cowork** — an agentic task execution layer built into Claude Desktop that shifts the model from *conversation* to *delegation*. Instead of pasting text and getting text back, you describe an outcome; Cowork plans the work, executes it, and delivers a finished file to your drive.

The course is designed for knowledge workers who want to hand off real deliverables — research synthesis, document creation, file cleanup, recurring reports — to an AI that can read their files, run code on their data, connect to their tools, and work in the background while they do something else.

By the end of the course you will know how to:

- Decide when Cowork is the right tool versus standard Claude chat
- Set up Claude Desktop, working folders, connectors, and the Chrome extension
- Run the core task loop: describe → plan → approve → execute → review
- Give Cowork persistent context through projects and global instructions
- Install and customize plugins so Claude approaches your work like a specialist
- Schedule recurring tasks that run on a cadence
- Frame file, document, research, and data analysis tasks for high-signal output
- Manage permissions, model choice, and usage
- Troubleshoot common issues and chart a path to a skill you trust running on a schedule

### The Core Idea

> "The shift is from conversation to delegation. You describe a task. Cowork plans it, works through it, and delivers a finished file to your drive."

Cowork is built on the same architecture as Claude Code, the agentic system used to write and ship production software. That capability is now available for knowledge work: analysis, research, writing, and the documents you produce every day.

---

## 🎯 Key Concepts (at a glance)

| Concept | Summary |
|---|---|
| **Cowork** | Agentic task execution in Claude Desktop — plans, executes, and delivers finished files |
| **Conversation → Delegation** | The core shift: you describe an outcome, not a turn of dialogue |
| **Plan → Execute → Connect** | The three pillars that make Cowork different from chat |
| **Working folder** | A directory you point Cowork at; it reads, edits, and creates files there directly |
| **Project** | A named workspace wrapping a folder with persistent instructions and scoped memory |
| **Global instructions** | Cross-project preferences in Settings → Cowork → Global Instructions |
| **Connectors** | Integrations to Slack, Google Drive, Gmail, Calendar, and similar tools |
| **Claude in Chrome** | Browser extension that lets Cowork reach pages without a connector |
| **Plugin** | A folder bundling skills, connectors, and subagents for a specific role or domain |
| **Skill** | A markdown file that teaches Claude how to handle one workflow; the building block of plugins |
| **Subagent** | An independent workstream that runs in parallel with its own context |
| **Scheduled task** | A recurring Cowork task that runs hourly, daily, weekly, or on a custom cadence |
| **Isolated environment** | Cowork runs in a sandbox on your computer; it can't reach what it hasn't been granted |
| **Deletion is gated** | Permanent file deletion always requires your explicit approval |
| **Model tiers** | Opus (most capable, highest usage), Sonnet (default), Haiku (fastest, lightest) |
| **The Sharp Question** | Asking for signal — contradictions, outliers, patterns — not "summarize everything" |

---

## 📖 Table of Contents

1. [What is Cowork?](#lesson-1-what-is-cowork) — 5 min
2. [Getting set up](#lesson-2-getting-set-up) — 6 min
3. [The task loop](#lesson-3-the-task-loop) — 7 min
4. [Giving Cowork context](#lesson-4-giving-cowork-context) — 6 min
5. [Plugins: Cowork as a specialist](#lesson-5-plugins-cowork-as-a-specialist) — 6 min
6. [Scheduled tasks](#lesson-6-scheduled-tasks) — 5 min
7. [File & document tasks](#lesson-7-file-document-tasks) — 6 min
8. [Research & analysis at scale](#lesson-8-research-analysis-at-scale) — 7 min
9. [Permissions, usage, & choosing your model](#lesson-9-permissions-usage-choosing-your-model) — 6 min
10. [Troubleshooting & next steps](#lesson-10-troubleshooting-next-steps) — 5 min
11. [Quiz on Claude Cowork](#lesson-11-quiz-on-claude-cowork) — 5 min

Then: [Course Summary](#course-summary) · [Quick Reference: Commands & Concepts](#quick-reference-commands-concepts)

---

## Lesson 1: What is Cowork?

**Estimated time:** 5 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Explain what Cowork is and how it differs from chatting with Claude
- Describe the core capabilities that make Cowork useful for knowledge work
- Recognize which tasks are a good fit for Cowork versus standard chat

### Key Takeaways

- **Cowork is built on the same architecture as Claude Code** — the agentic system used to write and ship production software. That capability is now available for knowledge work: analysis, research, writing, and the documents you produce every day
- The shift is **from conversation to delegation**. Instead of pasting text and getting text back, you describe a task; Cowork plans it, works through it, and delivers a finished file to your drive
- Cowork works **where your work lives**. It reads and writes files on your computer, connects to the tools you use, and keeps working on longer tasks while you step away
- You **stay in control**: you see the plan before work starts and you can interrupt or redirect at any point

### Detailed Notes

#### What's Different from Chat

In chat: you ask a question, get an answer, paste a document, get a summary. That loop is valuable and continues to be valuable, but it leaves certain kinds of work on your side of the table — you still move information between tools, assemble the output, and handle the steps in between.

Cowork changes the shape of that exchange for tasks that benefit from it. You describe an outcome; Claude plans the steps, works through them, and produces the deliverable — a file on your drive, not text to paste somewhere else.

Three things enable this:

- **Plan** — For multi-step work, Claude shows you its approach before starting. You review it, adjust if needed, and approve
- **Execute** — Work runs for as long as it needs, in an isolated environment on your computer. File creation, analysis, long-running tasks — you can let it run and come back
- **Connect** — Cowork can reach the systems where your work lives: your email, shared drives, the tools you're already signed into. Context flows to Claude without manual copy-paste

#### What Cowork Can Do

- **Connectors** — Claude reaches into your existing tools: email, messaging, shared drives, and more. Context flows in automatically
- **File operations** — Read, edit, and create real files on your drive: presentations, spreadsheets, documents. You get a finished file saved to your drive
- **Plugins** — Domain expertise built into Cowork. Each plugin comes with knowledge and workflows for a specific function, so Claude approaches your task the way a specialist would. Plugins bundle skills, connectors, and more into a single package for your role
- **Scheduled tasks** — Run a workflow on a recurring cadence. Work you'd otherwise remember to do can happen automatically
- **Subagents** — Parallelize. When a task has many independent pieces, Cowork works them at the same time
- **Local computation** — Run code on files in place and write results back. No upload and re-download cycle

#### When to Reach for Cowork

Cowork and chat fit different shapes of work. A useful question: **does this task need to touch your files, your connected tools, or produce a real output file?** If yes, Cowork is a good fit.

Another signal: if you've tried something similar in chat and ran out of room or hit conversation limits, Cowork is built to handle that scale.

- **Lean toward Cowork** when you want a finished file you can open, the work touches files on your drive or tools you're connected to, there are many steps or many items to process, or you want to let it run while you do something else
- **Lean toward chat** when you want an answer or a draft to refine yourself, everything Claude needs fits in a single paste, or you want to think through something together, turn by turn

### Practical Tips

- The defining test: *"Does this task need to touch my files, my connected tools, or produce a real output file?"* — yes means Cowork
- Conversation limits in chat are a strong signal that the work has outgrown chat
- Delegation ≠ abandonment — Cowork still asks for plan approval and lets you steer mid-run

---

## Lesson 2: Getting set up

**Estimated time:** 6 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Access Cowork and point it at a working folder
- Connect the tools and pages where your work lives
- Understand what you need to have in place

### Key Takeaways

- Cowork runs in the **Claude Desktop app**, available on paid plans
- The mode selector at the top of the app moves between **Chat, Cowork, and Code**
- **Working folder** is the key difference from chat: you give Claude a directory and it can read, create, and edit files there directly — no uploading or downloading
- **Connectors** link Cowork to Slack, Google Drive, Gmail, Calendar, etc. — set up once, then reference naturally in prompts
- **Claude in Chrome** is the extension that lets Cowork reach pages without a connector (internal dashboards, vendor portals, web apps behind logins)

### Detailed Notes

#### What You Need

Cowork runs in the Claude Desktop app. It's available on paid Claude plans — check your plan details for current availability.

#### Opening Cowork

1. Open Claude Desktop
2. At the top of the app you'll find a **mode selector** that lets you move between Chat, Cowork, and Code
3. Click **Cowork**
4. If Cowork isn't visible as an option, check that you're on a paid plan and that the desktop app is up to date

#### Point Claude at a Folder

This is the key difference from chat: you give Claude a working directory, and it can read, create, and edit files there directly. No uploading, no downloading.

1. Click **Work in a folder** and select a directory on your computer
2. Claude reads every file inside (PDFs, spreadsheets, Word docs, whatever's there) and saves finished work back to the same place
3. It asks permission before making changes
4. For work you'll come back to, a **project** wraps that folder with instructions and memory that carry across every session (covered in Lesson 4)

#### Connect Your Tools

Connectors link Cowork to the services where your work lives — Slack, Google Drive, Gmail, Calendar, and others. You set these up once, and every task from then on can pull from them.

1. Find the connector list in Cowork's **Customize** area
2. Toggle on the tools you use
3. Once connected, you can reference them naturally in your prompts: *"check what the team said in Slack about this"* instead of copying the thread in
4. Claude can also **act in connected tools** — draft an email in Gmail, save a file to Drive

#### Add Claude in Chrome

Some of your work lives on pages that don't have a connector — an internal dashboard, a vendor portal, a web app behind your login. **Claude in Chrome** is how Cowork reaches those.

- Install it from the same Customize area
- Grant permissions per site
- Cowork can open pages, read content, and interact with them as part of a task
- On its own, the extension is a sidebar assistant that reads your current tab; the automation happens when Cowork drives it

### Practical Tips

- Set up connectors once upfront — the ROI compounds with every task afterward
- Grant Chrome permissions per site rather than globally where possible
- Reserve a dedicated working folder per area of work to keep context clean

### Put It Into Practice

Open Claude Desktop, switch to Cowork, and point it at a folder you'd like to work with. You'll run your first task in the next lesson.

---

## Lesson 3: The task loop

**Estimated time:** 7 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Describe a task to Cowork clearly enough to get a useful plan
- Steer the approach before work begins and while it runs
- Approach finished outputs with the right reviewing mindset

### Key Takeaways

- The core loop: **Describe → Claude proposes a plan → You review/approve → Claude executes → Open finished file**
- A good prompt names **what to look at, what you want back, and where it should go** — Claude asks follow-up questions for anything you leave out
- A **progress panel** shows each step: which files Claude is reading, what it's building
- Treat outputs as **drafts from a capable colleague** — good work that's still yours to shape

### Detailed Notes

#### The Core Loop

Claude proposes a plan and waits for your approval before taking action. You adjust if needed, approve, and Claude executes. This is the pattern for **every** Cowork task.

**1. Describe what you want back.** A prompt works well when it gives Claude what to look at, what you want back, and where it should go. You don't have to engineer a perfect prompt — Claude will ask follow-up questions for whatever you leave out.

> *"Three of our coverage companies reported earnings this week. The transcripts are in my folder along with our model and last quarter's note. Read the transcripts against our model, check what the team's been saying in #research-desk on Slack, and update the research note. Flag anything that changes our assumptions."*

**2. Answer a few questions.** Based on your prompt and what it found, Claude asks a few questions to get the output right — which approach to take, what to prioritize, how the finished work should look. Pick one of the options Claude offers, or type your own answer.

**3. Step away — or step in.** A progress panel shows each step: which files Claude is reading, what it's building. For large tasks, Claude breaks the work into parts and handles them at once. Leave it and come back, or type in the chat to redirect if you see something heading somewhere you didn't mean.

**4. Open your finished work.** The result lands where you pointed it — saved in your folder alongside the files Claude read. If the prompt had asked for something different (*"draft this as an email in Gmail"* or *"save to the shared Drive folder"*), it would show up there instead. **Treat the result as a draft.** Read it the way you'd read a first pass from a capable colleague: good work that's still yours to shape before you send it on.

#### A Few Things That Shape the Experience

- **Subagents** — When a task has independent pieces, Cowork can spin up separate workstreams that run at the same time, each with its own job and its own fresh context
- **Progress is visible** — A panel shows which step is active, which files are being read, what's being built. You can follow along or ignore it
- **You can steer while it runs** — Type in the chat to redirect if something heads somewhere you didn't mean. No need to wait for it to finish
- **Isolated environment** — Code runs and files get built in an isolated environment on your computer, separate from your main system, without touching anything you haven't granted access to
- **Deletion is gated** — Cowork asks before permanently deleting files. You approve or decline

#### Walkthrough: A Scattered Folder Becomes a Finished Deck

**The starting point:** A project folder with the usual accumulation — meeting notes, a checklist, a timeline spreadsheet, some saved emails, a comparison matrix. Different formats, loosely organized.

**The goal:** Turn this into a leadership-ready presentation.

**Describe the task:**

> *"Review everything in this folder and create a leadership presentation for the tooling review. Include the vendor evaluation results, timeline, business case, and open risks. Output a PowerPoint file."*

**Review the plan:** Cowork shows its plan: read the files, synthesize the proposal, build the business case, generate the deck, review the result. If you want something added — say, a PDF alongside the PowerPoint — you can say so here.

**Let it run:** Watch if you want, or go to a meeting. The work continues.

**Open the file:** The deck is a real PowerPoint file. Charts are editable elements you can click into and adjust. It's a draft you can refine, starting from a place much closer to finished than assembling it yourself.

### Practical Tips

- **Start small.** Begin with tasks that have clear boundaries — organizing a folder, synthesizing a set of documents. Build your intuition for what Cowork handles well, then scale up
- Resist the urge to "watch the whole thing" — the value is in the delegation
- When redirecting mid-run, type into the same chat — the steer goes into the active work

### Put It Into Practice

Use the task you identified in Lesson 1. Point Cowork at the relevant folder, describe what you want back, and run through the loop.

---

## Lesson 4: Giving Cowork context

**Estimated time:** 6 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Give Cowork context that carries across every session, without re-explaining each time
- Use a project's **Instructions** panel to tell Claude how to work
- Set **global instructions** for preferences that apply everywhere

### Key Takeaways

- Each Cowork task starts fresh — **Claude doesn't carry conversation memory from one task to the next**
- Context carries over through a **project**: a named workspace backed by a real folder, with instructions and memory that persist
- A **Cowork project's memory stays inside that project** — unlike chat memory, which is global
- **Global instructions** in Settings → Cowork apply to every task, in any project

### Detailed Notes

#### Why This Matters

Each Cowork task starts fresh — Claude doesn't carry conversation memory from one task to the next. The way context carries over is a **project**: a named workspace backed by a real folder on your machine, with instructions and memory that persist into every task you start inside it.

If you use projects in Chat, Cowork projects work similarly — but they live locally on your computer and are built around tasks. The memory is scoped differently too: **Chat remembers across all your conversations; a Cowork project's memory stays inside that project.**

Projects live in Cowork's sidebar. You can:

- Start one from scratch
- Import from a Chat project (files and instructions come over; sync stays one-way)
- Wrap a folder you already have on your machine

#### What Goes in Instructions

Once you're in a project, the right-hand panel has an **Instructions** section. What you put there is read in every task you start from that project. Some things that tend to be useful:

- **Who's involved** — names, roles, how to reach them, so *"send this to Rachel"* means something
- **Where things live** — *"contracts are in ./Contracts, old reports in Drive /archive/[year]"*
- **Output preferences** — *"drafts go in .docx, finals in PDF, save to the deliverables subfolder"*
- **Project-specific rules** — *"metric units throughout, cite the source for every number"*

It doesn't need to be polished. A few lines is enough.

**The effect:** shorthand starts working. Once Instructions says who's who and where things are, *"send this to the client"* and *"file it where the Q3 report went"* mean specific things.

Claude also reads everything in your project folder. For context you want Claude to **maintain** — running notes, a growing glossary, anything that evolves — put it in a file there.

#### Global Instructions

For preferences that don't change between projects — your role, default output formats, standing rules like *"ask before deleting"* — use **global instructions**.

- Set them in **Settings → Cowork → Global Instructions**
- These apply to **every** Cowork task, inside a project or not

#### Check It's Working

From inside your project, ask:

> *"Tell me what you know about how I work here."*

You'll see whether Instructions and memory are being picked up, and what's missing.

### Practical Tips

- Treat Instructions as you would brief a new teammate — names, conventions, where things live
- Put evolving context in a file inside the project folder; put stable context in Instructions
- Use the *"Tell me what you know"* prompt as a smoke test before kicking off real work

### Put It Into Practice

Create a project for something you're actively working on — use an existing folder if you have one. Add a few lines to Instructions: who the key people are, where related files live, one output preference. Start a task inside it and notice the folder already set in the input bar.

---

## Lesson 5: Plugins: Cowork as a specialist

**Estimated time:** 6 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Explain what a plugin is and what's inside it
- Install a plugin that matches your role
- Understand **skills** as the building blocks plugins are made of

### Key Takeaways

- **Plugins give Cowork domain expertise** — knowledge and workflows for a specific function so Claude approaches your task the way a specialist would
- A plugin bundles: **skills** (workflows), **connectors** (system reach), and **subagents** (parallel specialization)
- A plugin is just a **folder of plain text** files on your machine — every file is readable and editable, no build step
- **Skills** are the core building block inside plugins — a markdown file that teaches Claude how to handle one thing

### Detailed Notes

#### What a Plugin Is

Plugins give Cowork domain expertise. Each one comes with built-in knowledge and workflows for a specific function, so Claude approaches your task the way a specialist would.

A plugin is a bundle — several pieces packaged together for a role or domain:

- **Skills** — Instructions for handling specific workflows. Claude draws on them automatically, or you invoke them with `/` in the prompt. Example: how to structure a deal brief, `/prep-call`, `/weekly-report`
- **Connectors** — Reach the systems where the work happens. Example: your CRM, your docs, your messaging
- **Subagents** — Parallelize specialized work. Example: one agent per account in a book-wide review

Open-source plugins are available for most knowledge-work roles: **sales, marketing, product, finance, legal, operations, customer support, data**, and more. They work as-is and can be modified.

#### Installing a Plugin

1. **Browse.** Plugins live in Cowork's Customize area. Find one that matches what you do
2. **Install.** One click. The plugin is active immediately
3. **Customize.** Once installed, the plugin is a folder on your machine. Everything in it is readable and editable

#### What's Inside

A plugin is just a folder. The structure looks something like this:

```
sales-plugin/
├── plugin.json          ← manifest: name, description, dependencies
├── skills/
│   ├── deal-brief/      ← how to structure a deal brief
│   ├── territory/       ← how to build a territory report
│   └── prep-call/       ← /prep-call in the slash menu
└── agents/
    └── account-sweep.md ← subagent for per-account work
```

Every file is plain text. To change how a skill works, open its file and edit it. To add a new skill, add a folder under `skills/`. There's no build step — Cowork reads the folder directly.

#### About Skills

You'll notice skills show up a lot — they're the core building block inside plugins. A **skill** is a markdown file that teaches Claude how to handle one thing: a workflow, a format, a process.

- Skills aren't specific to Cowork — they work across Claude's surfaces, in chat, in Claude Code, anywhere Claude runs
- A **plugin** is the Cowork-specific way of bundling skills with the connectors they need to do a job

#### Learn More About Plugins

- Plugin directory — browse all plugins, Anthropic and community-built
- Use plugins in Cowork — Help Center guide to installing and setup
- How to customize plugins in Cowork
- Build a plugin from scratch
- Cowork plugins announcement
- Knowledge-work plugins on GitHub
- Financial-services plugins on GitHub

### Practical Tips

- Open one of the skill files in an installed plugin to see the format — it's just markdown you'd write for a teammate
- Match the plugin to your role first; customize the pieces you actually use

### Put It Into Practice

Open Cowork's Customize area and browse plugins. Install the plugin closest to your role. Find the installed plugin folder and open one of the skill files. See that it's readable text, written the way you'd brief a teammate.

---

## Lesson 6: Scheduled tasks

**Estimated time:** 5 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Set up a scheduled task to run work on a recurring cadence
- Understand how scheduled tasks behave when the app is closed or the machine is asleep

### Key Takeaways

- Scheduled tasks run any Cowork task automatically on a cadence — **hourly, daily, weekly, or custom**
- Set one up by typing **`/schedule`** in any Cowork conversation, or use the scheduled tasks area in the sidebar
- If the app was closed or the computer asleep at the scheduled time, **Cowork runs the task as soon as you're back and lets you know it was delayed**
- Skills + scheduled tasks compose: a **skill** encodes *what* to do; a **schedule** decides *when*

### Detailed Notes

#### Running Work on a Cadence

Scheduled tasks run any Cowork task automatically on a cadence you set — hourly, daily, weekly, or custom. The task can be anything: a prompt you've written, a plugin skill, or a workflow you've refined. Once you have something that works well, you can stop prompting for it each time.

**Example:** *"Every Monday at 9am, pull my calendar and draft a weekly priorities email."*

#### Setting One Up

1. Type `/schedule` in any Cowork conversation, or use the scheduled tasks area in the sidebar
2. Claude walks you through the **cadence**, the **folder**, and what the **output** should look like
3. There's an **approval step** — since you're signing off on something that will run repeatedly
4. Once approved, the task runs on its own while the desktop app is open
5. If your computer was asleep or the app was closed when a task was due, Cowork runs it as soon as you're back and lets you know it was delayed

If you create a scheduled task from inside a project, it appears alongside that project's other scheduled work — a quick way to see everything running on a cadence for one piece of work.

#### Managing Scheduled Tasks

From the scheduled tasks area you can:

- **Review** past runs
- **Edit** the instructions or cadence
- **Pause** a task
- **Trigger** it on demand

Any connectors and plugins you've set up are available to scheduled tasks too.

#### A Common Pattern

**Scheduled tasks and skills compose naturally.** A skill encodes what to do; a scheduled task decides when.

> A briefing skill scheduled for 8am weekdays means the briefing is waiting every morning.

But you don't need a skill to schedule something. **Any task that works well is a candidate.**

### Practical Tips

- Sleep ≠ quit — sleep preserves the session; quitting the app pauses the task
- Schedule the workflows that you already do manually and reliably — that's where the time savings compound

### Put It Into Practice

Think of something you currently do on a cadence — a weekly status pull, a daily folder check, a recurring report. You don't have to schedule it yet. Just note it: once you've run it successfully once in Cowork, scheduling it is one more step.

---

## Lesson 7: File & document tasks

**Estimated time:** 6 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Recognize common file and document tasks that fit Cowork well
- Write prompts that name the **input, the transformation, and the output**
- Use example prompts as templates for your own work

### Key Takeaways

- Cowork produces **real, native files** — presentations with editable charts, spreadsheets with working formulas, documents with track changes
- Templates and brand rules **compound** — once you've made a brand-guidelines skill, every file Cowork produces can reference it
- The prompt pattern: **Input → Transformation → Output**

### Detailed Notes

#### Working with Files

**Key takeaways from the video:**

- Cowork produces real files. Presentations with editable charts. Spreadsheets with working formulas. Documents with track changes. The files themselves are saved to your drive, ready to open
- **The output is native.** A chart in a Cowork-made deck is an editable chart. You can click in, adjust the data, change the formatting — same as anything you made by hand
- **Templates and brand rules compound.** Once you've made a brand-guidelines skill (Lesson 6), every file Cowork produces can reference it

#### Tasks You'll Reach for Early

**Organize what you have:**

- *"Sort my Downloads folder by file type into dated subfolders."*
- *"Rename all files in this folder using a consistent date-first format."*
- *"Create a formatted expense report from the receipts in this folder."*

**Create what you need:**

- *"Build a project tracking spreadsheet from these notes, with status columns and a summary view."*
- *"Turn these meeting notes into a slide deck. Use the brand-guidelines skill."*
- *"Combine the transcripts and notes in this folder into a formatted report."*

#### The Prompt Pattern

The most reliable file-task prompts have three named parts:

- **Input** — Where the source material is. *"my Downloads folder"*, *"these meeting notes"*, *"this folder"*
- **Transformation** — What to do with it. *"sort by type"*, *"turn into a slide deck"*, *"combine into"*
- **Output** — What format to produce. *"dated subfolders"*, *"a spreadsheet with a summary view"*, *"a formatted report"*

> *"Clean up my files"* has none of these elements. *"Sort my Downloads by file type into dated subfolders"* has all three. The first gets you a clarifying question. The second gets you work.

### Practical Tips

- If you find yourself re-writing the same prompt for the same kind of output, it's a skill waiting to be made
- Editable native output is the whole point — click into the chart, adjust the data, refine the doc

### Put It Into Practice

Take one of the prompts above. Replace the specifics with your own: your folder, your file types, your output format. Run it. For more task ideas, browse the Cowork use-case gallery.

---

## Lesson 8: Research & analysis at scale

**Estimated time:** 7 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Identify research and analysis tasks where Cowork's approach helps most
- Describe the kind of insight Cowork can surface at scale
- Frame tasks to find the **signal**, not just produce a summary

### Key Takeaways

- Chat fits when material fits in one conversation; **Cowork fits when the work has volume, parallelism, or in-place computation**
- The most useful prompts here aren't *"summarize everything"* — they're **the question you'd answer yourself if you had the time**
- Cowork's value at scale is **cross-referencing** — finding connections, contradictions, and outliers across many sources

### Detailed Notes

#### Where the Shape of the Work Matters

Both chat and Cowork can help with research and analysis. The difference is in the shape of the work that fits each. Chat is excellent when the material fits in one conversation: you can paste it, discuss it, iterate turn by turn.

**Cowork fits better when the work has one or more of these qualities:**

- **Volume** — Too many files to paste, or files too large to hold at once. Cowork reads them in place — no upload step, and you're not bounded by what fits in a single chat
- **Parallelism** — When the task is *"do the same analysis across N items,"* Cowork can process them simultaneously rather than one at a time
- **In-place computation** — Cowork can run code on files where they live and write results back to disk. No upload and re-download cycle. The output is already where you need it

None of these are things chat can't touch — they're things Cowork is built to handle more naturally when they're the shape of the work.

#### Examples of This Shape

**Research synthesis** — Combine notes, articles, papers, and saved research into a coherent report. The value at scale comes from the cross-referencing: Cowork holds all the sources at once and can find connections you'd miss reading sequentially.

> *"Read everything in the research folder and write a synthesis report. Where do the sources agree? Where do they contradict each other? Flag any claim that only one source makes."*

**Transcript analysis** — Extract themes, decisions, and action items from a collection of meeting notes, interviews, or recordings. A few transcripts fit fine in a chat. A large batch benefits from Cowork reading them in parallel — and in particular, **finding the places they disagree**.

> *"Read all transcripts in this folder. What did most people agree on? Who disagreed, and what do those people have in common?"*

You might learn that the outliers share a trait. **That's the insight buried at scale.**

**Data analysis** — Statistical work on your data files: outlier detection, cross-tabulation, time-series. When the data lives on your drive, Cowork can work on it in place and write the results right back where you need them.

> *"For each customer in this file, calculate month-over-month usage change. Flag accounts with three consecutive months of decline and write a summary to the same folder."*

**Knowledge synthesis** — Point Cowork at your own accumulated notes, journals, or project files and ask what patterns it finds.

> *"Read all my project notes from the last six months. What questions keep coming up? What did I decide in one project that contradicts what I decided in another?"*

#### The Framing: Ask for Signal

The most useful prompts here aren't *"summarize everything."* They're the question you'd answer yourself if you had the time. Some examples of sharpening:

- *"Summarize these transcripts"* → *"What did most people agree on? Who disagreed, and what do they have in common?"*
- *"Analyze this data"* → *"Which accounts are at risk based on the last three months? What's the common pattern?"*
- *"Review these papers"* → *"Where do these papers contradict each other? Which claims need the most caveats?"*

**The sharper question gets you something actionable.**

### Practical Tips

- The pattern *"what's the outlier? what's the contradiction? what would change my decision?"* consistently produces useful analyses
- For data analysis, name both the file location and the destination for results
- Volume is the entry point — when paste doesn't fit, that's your Cowork signal

### Put It Into Practice

Think of a body of input you've been putting off working through — a folder of call notes, a dataset, a collection of saved research. Ask Cowork the **sharp question**. What's the outlier? What's the contradiction? What would change your decision?

---

## Lesson 9: Permissions, usage, & choosing your model

**Estimated time:** 6 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Describe the **safety boundaries** Cowork operates within
- Manage your **usage allocation** effectively
- Build the habit of **reviewing outputs** before acting on them

### Key Takeaways

- Cowork's safety boundaries: **isolated execution, controlled file access, network policies respected, deletion is gated**
- Cowork **conversation history is stored locally** on your machine
- Cowork uses **more allocation than chat** because multi-step, long-running work is more compute-intensive
- Models come in three tiers — **Opus, Sonnet, Haiku** — trading capability against cost; match the model to the task

### Detailed Notes

#### Permissions and Safety

Cowork can read and write real files and connect to real systems. Here are the boundaries that shape how that works:

- **Isolated execution** — Work runs in an isolated environment on your computer, separate from your operating system. Cowork can't reach what it hasn't been granted
- **Controlled file access** — You decide which folders Cowork can see. No grant, no access
- **Network policies respected** — Cowork follows your organization's network rules. Restricted environments stay restricted
- **Deletion is gated** — Permanent deletion requires your explicit approval. You'll always see a prompt first

**One thing worth knowing up front:** Cowork conversation history is stored locally on your machine. Check your plan's documentation for current details on audit logging and compliance features — particularly if you have workloads with regulatory requirements.

#### Reviewing What Cowork Did

Before you send an output onward or act on a result, **review it.** The more polished an output looks, the more a second look is worth — Cowork's confidence is the same whether the work is right or wrong. **Your review catches the difference.**

**The habit:** open the file. Check a number. Follow one thread of reasoning.

For more on this habit and the other competencies that make working with AI effective, see the **AI Fluency** course.

#### Managing Usage

Cowork uses **more of your allocation than chat does** — multi-step, long-running work is more compute-intensive than a single turn. A few habits help you spend it well:

- **Batch related work** — Starting a fresh session has overhead. If you have several related tasks, do them in one session rather than separate ones
- **Use chat for tasks that fit it** — If a task doesn't need your files, your connected tools, or a real output file, chat is often faster and less resource-intensive. The question from Lesson 1 — does this need my files, tools, or a real output? — is also a good usage question
- **Monitor where you stand** — Cowork's settings include usage visibility. Check in periodically, especially as you're building habits

#### Choosing the Right Model

Which model you use also affects consumption. **Claude models come in three capability tiers:**

- **Opus** — Handles the most complex multi-step work and uses the most allocation
- **Sonnet** — Sits in the middle as a sensible default for everyday tasks
- **Haiku** — The quickest and lightest

These tiers trade off capability against cost. **Match the model to what the task needs** rather than defaulting to the highest option for everything.

For more on how the tiers differ and when each one fits, see **Choosing the right Claude model**. For how usage and allocation work across plans, see **plans and pricing**.

### Practical Tips

- Reach for Opus on the multi-step, planning-heavy tasks where mistakes are expensive
- Haiku is enough for many organize-and-summarize jobs; Sonnet is the daily driver
- Treat *"this looks polished"* as a flag to verify, not a reason to skip review

### Lesson Reflection

- Do you have any workflow where the audit and compliance characteristics of Cowork matter? If so, check your plan's current documentation
- Are you running tasks in Cowork that would fit just as well in chat? That's an easy place to save allocation
- When did you last open a Cowork output and actually check something in it before sending it onward?

---

## Lesson 10: Troubleshooting & next steps

**Estimated time:** 5 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Recognize and resolve common issues people encounter with Cowork
- Know what to do next: install, build, schedule, share
- Know where to go for help beyond this course

### Key Takeaways

- *"Cowork is preparing your workspace"* is normal — particularly the first time or after an update
- **Sleeping is fine; quitting the app pauses the task**
- If a task was due while the app was closed or computer asleep, **Cowork runs it as soon as you're back and tells you it was delayed**
- The end-state goal: **a skill you trust running on a schedule**

### Detailed Notes

#### Common Issues and How to Approach Them

| Symptom | Most likely cause | What to do |
|---|---|---|
| *"Cowork is preparing your workspace"* | Environment warming up | Give it a moment — expected, especially first time or after an update |
| Task stopped mid-run | The desktop app was quit mid-task | Sleeping the computer is fine; quitting pauses the task. Check that Claude Desktop is open, then check your connection |
| Running into usage limits | Long-running tasks on capable models consume more | Batch related work, use chat for tasks that fit it, match your model choice to what the task needs |
| Can't find a file Cowork said it made | Output landed in a different folder | Check the working folder Cowork was using; verify folder access in settings; ask Cowork directly where it wrote the file |

**Two crucial distinctions:**

- **Sleeping the computer is fine** — the session survives and picks back up when you wake it
- **Quitting the app pauses the task** — check that Claude Desktop is open

#### What to Do Next

The goal: **have a skill you trust running on a schedule.** The steps to get there:

1. **Install a plugin** — Browse the library, pick one that matches your role, install it. Open the plugin folder and read one of the skill files to see what a skill actually looks like
2. **Run something real** — Pick a task from your actual work — something that matters, even if it's small. What you're looking for: the moment you come back from doing something else and the finished file is there
3. **Make a skill** — If you produce branded output, build a brand-guidelines skill. Otherwise, the next time you run a task and realize you'll run it again, save that session as a skill
4. **Schedule it** — Put the skill on a schedule. If the app is closed when the task is due, it runs as soon as you reopen
5. **Share it** — Package the skill for a teammate, or on Team and Enterprise plans, talk to your admin about making it available more broadly

> Take these at whatever pace fits your work. **The sequence matters more than the timing.**

#### Where to Go From Here

- **Setup and troubleshooting** — Getting started with Cowork
- **Task ideas** — Cowork use-case gallery
- **Plugin source and customization** — github.com/anthropics/knowledge-work-plugins
- **Building lasting habits for working with Claude** — AI Fluency course

### Practical Tips

- Keep a running list of *"I keep doing this manually"* moments — each one is a candidate for a skill
- The first scheduled task should be something low-stakes but real; the goal is to feel the system work
- On Team and Enterprise, the admin is your friend for sharing what you build

### Lesson Reflection

- What's the first real task you'll hand to Cowork?
- Which plugin matches what you do? Is it installed?
- What workflow — one you already run manually — would save you the most time if it ran on a schedule?

**Congratulations:** You've completed the Cowork end user training. You know how to describe a task, review a plan, let work run, and review the output. You know where Cowork fits in your work alongside chat. You know how to teach it something once so you don't have to explain it again. **Now pick something real and hand it over.**

---

## Lesson 11: Quiz on Claude Cowork

**Estimated time:** 5 minutes
**Format:** 8-question knowledge check (the course material excerpt includes 5 of them)

### What the Quiz Tests

This end-of-course quiz checks your understanding of the core Cowork concepts: subagents, project memory, skills vs. plugins, scheduled-task behavior, and how context carries across tasks.

### Sample Questions and Correct Answers

#### Question 1 — Subagents

> *What do subagents enable when Cowork is handling a large task with independent pieces?*

**Correct answer: They work on separate pieces at the same time, each with its own focused context.**

Why the others are wrong:

- They don't run tasks on other computers in your network
- They don't automatically retry failed steps without telling you
- They don't store each piece of work permanently so it can be reused later (subagents are short-lived, scoped to one task)

#### Question 2 — Project Memory

> *How does a Cowork project's memory differ from memory in Claude chat?*

**Correct answer: A Cowork project's memory stays inside that project, while chat memory applies across all your conversations.**

Why the others are wrong:

- Cowork project memory isn't a size upgrade — the distinction is **scope**
- They don't work the same way and share information
- Cowork project memory *can* be updated during a task; it's not read-only

#### Question 3 — Skills vs. Plugins

> *Which statement correctly describes the relationship between skills and plugins?*

**Correct answer: Skills work across Claude's surfaces, and a plugin is the Cowork-specific way of bundling skills with connectors and other pieces for a role.**

Why the others are wrong:

- Skills aren't developer-only and plugins aren't end-user-only
- The direction is reversed: skills are cross-surface; plugins are the Cowork-specific bundle
- They're not two names for the same thing — skills are the building block, plugins are the package

#### Question 4 — Sleep and Scheduled Tasks

> *A scheduled task was due while your computer was asleep. What happens?*

**Correct answer: Cowork runs the task as soon as you're back and lets you know it was delayed.**

Why the others are wrong:

- The task isn't skipped
- It doesn't fail — you don't need to reschedule
- It doesn't run in the cloud — Cowork runs the task locally once you're back

#### Question 5 — Context Carrying Across Tasks

> *Each Cowork task starts fresh. How does context carry from one task to the next?*

**Correct answer: Through a project (folder, instructions, and memory for one piece of work) and global instructions that apply to every task.**

Why the others are wrong:

- Cowork does **not** automatically remember every conversation across tasks
- You don't export and import projects between tasks
- Context *does* carry — that's exactly what projects and global instructions are for

### Practical Tips

- The correct answer in every question is the one that scopes to the *right boundary* — subagent scope, project scope, skill scope, schedule scope
- If two options look plausible, re-read for the verbs: *"works at the same time"* vs. *"stores permanently"* is the difference between parallelism and persistence

---

## 📚 Course Summary

Across 10 instructional lessons and a closing knowledge check, this course makes one core argument: **the right model for knowledge work is delegation, not conversation**. The whole architecture of Cowork — the working folder, the plan-and-approve loop, the project-scoped memory, the plugins, the scheduled tasks — is in service of that idea.

### The Five Big Patterns

**1. Conversation → Delegation.** The single biggest shift in how you work with Claude. You stop drafting prompts and start writing outcomes. The output is a file on your drive, not a paragraph to copy-paste.

**2. Plan → Approve → Execute → Review.** Every Cowork task runs through this loop. The plan gate is the safety mechanism; the review step is the quality mechanism. Skipping either is where things go wrong.

**3. Project-scoped memory, not conversation memory.** Cowork doesn't remember across tasks — projects do. The unit of persistence is the project (a folder + instructions + memory), and instructions can be global or project-specific.

**4. Skills compound; plugins package them.** A skill is a small markdown file that encodes one workflow. A plugin bundles skills with the connectors and subagents they need for a role. The trajectory is: write a skill → save the workflow as a skill → install a plugin → schedule the skill → share the skill.

**5. The sharp question beats the broad summary.** Cowork's value at scale comes from cross-referencing many sources — finding contradictions, outliers, and patterns. Prompting *"what's the contradiction?"* or *"who disagreed, and what do they have in common?"* reliably surfaces insights that *"summarize this"* never will.

### How the Pieces Fit Together

```
                          ┌─────────────────────────────┐
                          │   You describe an outcome   │
                          └──────────────┬──────────────┘
                                         │
                          ┌──────────────▼──────────────┐
                          │  Cowork proposes a plan     │
                          │  (Plan gate)                │
                          └──────────────┬──────────────┘
                                         │ approve / adjust
                          ┌──────────────▼──────────────┐
                          │  Isolated execution         │
                          │  • reads your folder        │
                          │  • uses plugins/skills      │
                          │  • may spin up subagents    │
                          │  • may use connectors       │
                          └──────────────┬──────────────┘
                                         │
                          ┌──────────────▼──────────────┐
                          │  Finished file on your      │
                          │  drive (or in a connector)  │
                          │  → you review and refine    │
                          └──────────────┬──────────────┘
                                         │
                          ┌──────────────▼──────────────┐
                          │  Save the workflow as a     │
                          │  skill, schedule it, share  │
                          └─────────────────────────────┘
```

### The Decision Framework: Cowork vs. Chat

| Signal | Lean toward |
|---|---|
| I want a finished file I can open | **Cowork** |
| The work touches files on my drive | **Cowork** |
| The work touches connected tools (Slack, Drive, Gmail…) | **Cowork** |
| Many steps or many items to process | **Cowork** |
| I want to let it run while I do something else | **Cowork** |
| I want an answer or a draft to refine myself | **Chat** |
| Everything Claude needs fits in a single paste | **Chat** |
| I want to think through something together, turn by turn | **Chat** |

### The End State the Course Points Toward

> *"The goal: have a skill you trust running on a schedule."*

Five steps, in any order of pacing but in a fixed **sequence**:

1. **Install a plugin** that matches your role
2. **Run something real** — feel the moment the finished file is waiting
3. **Make a skill** — capture the workflow you just ran
4. **Schedule it** — put it on a cadence
5. **Share it** — with a teammate, or with your org on Team/Enterprise

### The Mindset Shifts

- From *"a clever prompt"* to *"a clear outcome"*
- From *"watching the run"* to *"approving the plan and coming back"*
- From *"remembering context"* to *"persisting context in a project"*
- From *"asking chat"* to *"delegating to a specialist (plugin)"*
- From *"doing it manually each time"* to *"scheduling the skill"*

---

## 📖 Quick Reference: Commands & Concepts

### Setup Commands & Locations

| Item | Where |
|---|---|
| Open Cowork | Claude Desktop → mode selector at top of app → **Cowork** |
| Point at a folder | **Work in a folder** in the Cowork input area |
| Manage connectors | Cowork → **Customize** area → toggle per service |
| Install Claude in Chrome | Cowork → **Customize** area → install extension |
| Manage plugins | Cowork → **Customize** area |
| Schedule a task | Type **`/schedule`** in any Cowork conversation, or use the sidebar |
| Global instructions | **Settings → Cowork → Global Instructions** |
| Project instructions | Right-hand panel inside a project → **Instructions** section |

### Key Slash Commands & Prompts

| Command | What it does |
|---|---|
| `/schedule` | Walks you through setting up a recurring Cowork task |
| `/prep-call`, `/weekly-report`, etc. | Skill-specific invocations (varies per plugin) |
| *"Tell me what you know about how I work here."* | Smoke-test whether project instructions and memory are picked up |
| *"Where did you write the file?"* | Recover when you can't find an output |

### The Core Task Loop (in order)

1. **Describe** — what to look at, what you want back, where it should go
2. **Plan** — Cowork proposes; you review and adjust
3. **Approve** — you give the go-ahead
4. **Execute** — Cowork works (you can step away or steer)
5. **Open & review** — finished file lands where you pointed

### The Prompt Pattern for File Tasks

```
[Input] — where the source material is
[Transformation] — what to do with it
[Output] — what format to produce
```

> *"Sort my Downloads by file type into dated subfolders"* has all three. *"Clean up my files"* has none. The first gets work; the second gets a clarifying question.

### The Sharp Question Transformations

| Broad prompt | Sharper prompt |
|---|---|
| Summarize these transcripts | What did most people agree on? Who disagreed, and what do they have in common? |
| Analyze this data | Which accounts are at risk based on the last three months? What's the common pattern? |
| Review these papers | Where do these papers contradict each other? Which claims need the most caveats? |

### Plugin Folder Structure (Reference)

```
<plugin-name>/
├── plugin.json          ← manifest: name, description, dependencies
├── skills/              ← workflow skills (markdown)
│   ├── skill-a/
│   └── skill-b/
└── agents/              ← subagent definitions
    └── subagent.md
```

Every file is plain text; no build step.

### Safety Boundaries (Recap)

- **Isolated execution** — sandbox on your computer, separate from OS
- **Controlled file access** — you grant folders; no grant, no access
- **Network policies respected** — your org's rules apply
- **Deletion is gated** — permanent deletes always require approval
- **Conversation history is local** — check plan docs for audit/compliance details

### Model Tiers

| Tier | Use when |
|---|---|
| **Opus** | Most complex multi-step work; highest allocation use |
| **Sonnet** | Sensible default for everyday tasks |
| **Haiku** | Quickest, lightest; good for organize/summarize |

### Usage Habits

- **Batch** related work in one session (avoid session-start overhead)
- **Use chat** for tasks that don't need files, tools, or real output
- **Monitor** usage visibility in Cowork's settings
- **Match** the model to the task — don't default to Opus

### Troubleshooting Quick Reference

| Issue | First thing to try |
|---|---|
| *"Preparing your workspace"* | Wait — first run or post-update is slower |
| Task stopped mid-run | Check that Claude Desktop is open; check connection (sleeping is fine, quitting pauses) |
| Usage limits hit | Batch work, downgrade to chat where appropriate, use a lighter model |
| Can't find output | Check the working folder; verify folder access; ask Cowork where it wrote the file |
| Scheduled task "missed" | It'll run as soon as you're back and tell you it was delayed |

### Links Referenced in the Course

- Course: <https://anthropic.skilljar.com/introduction-to-claude-cowork>
- Knowledge-work plugins on GitHub: `github.com/anthropics/knowledge-work-plugins`
- Financial-services plugins on GitHub
- Getting started with Cowork (Help Center)
- Choosing the right Claude model
- Plans and pricing
- AI Fluency course (for working-with-AI competencies)
- Cowork use-case gallery

---

*Notes generated from the Anthropic Academy course "Introduction to Claude Cowork" (https://anthropic.skilljar.com/introduction-to-claude-cowork). All credit for the course content belongs to Anthropic. These notes are for personal study and reference.*
