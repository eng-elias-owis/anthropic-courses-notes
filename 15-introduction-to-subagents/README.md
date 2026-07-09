# Introduction to Subagents — Course Notes

> **Source:** https://anthropic.skilljar.com/introduction-to-subagents
> **Extracted:** July 2026
> **Author:** Anthropic Academy
> **Estimated Time:** ~70 minutes total (15 + 20 + 20 + 15)
> **Lessons:** 4
> **Format:** Video lessons + written notes

---

## 📚 Course Overview

This course teaches you how to **use and create subagents in Claude Code** to manage context, delegate tasks, and build specialized workflows that keep your main conversation clean and focused. Subagents are one of the most practical ways to get more out of longer Claude Code sessions. They let you delegate tasks to isolated assistants that do their work separately and return just the information you need — keeping your main context window clean and your conversations focused.

The course is designed for developers using Claude Code who want to **scale beyond a single conversation thread**. It walks you from "what is a subagent?" through the built-in agents, the `/agents` slash command for creating custom subagents, the four design patterns that make subagents actually effective, and finally a clear decision rule for when delegation is worth it and when it gets in the way.

### What Makes This Course Useful

- **Practical and pattern-driven** — every lesson ends with concrete patterns (output formats, tool access rules, decision rules) you can apply to your own subagents
- **Built on real Claude Code features** — uses the actual `/agents` command, the real `.claude/agents/*.md` config files, and the real built-in `Explore`, `Plan`, and `General purpose` subagents
- **Anti-pattern awareness** — explicitly covers what *not* to do (expert personas, sequential pipelines, test runners) so you don't waste time building the wrong thing
- **Connects to a real decision rule** — by the end you have a one-question heuristic that tells you when to delegate and when to keep work in your main thread

### The Core Idea

> "If you just need an answer and don't care about the journey, delegate it to a subagent. If you need to see and react to what's happening along the way, keep it in your main thread."

Subagents solve a fundamental problem with long Claude Code sessions: the main context window is finite, and once it fills up, Claude starts losing track of earlier parts of the conversation. A subagent spins up a **separate context window**, does its work there in isolation, and returns only a summary to your main thread. The intermediate steps — all the file reads, searches, and tool calls — never enter your main conversation. You get the answer without all the noise of the journey it took to find it.

---

## 🎯 Key Concepts (at a Glance)

| Concept | Summary |
|---|---|
| **Subagent** | A specialized assistant that runs in its own context window and returns a summary to the main thread |
| **Context isolation** | The subagent's intermediate work (file reads, searches, tool calls) stays out of your main conversation |
| **Built-in subagents** | `Explore` (codebase search), `Plan` (plan-mode research), `General purpose` (multi-step tasks) |
| **`/agents` command** | The slash command that opens the subagent management interface in Claude Code |
| **Project-level scope** | Subagent config saved to `.claude/agents/your-agent-name.md` — only available in the current project |
| **User-level scope** | Subagent config saved to your home directory — shared across all projects on your machine |
| **YAML frontmatter** | The `name`, `description`, `tools`, `model`, and `color` fields that define a subagent |
| **System prompt** | The markdown body below the frontmatter — the subagent's actual instructions |
| **Description-as-input-shaper** | The description controls both *when* the subagent is launched *and* what the input prompt says |
| **"Proactively" trigger** | Including this word in the description makes Claude delegate automatically |
| **Structured output format** | The single most important design pattern — gives the subagent clear stopping points |
| **Obstacle reporting** | A dedicated output section for workarounds, quirks, and environment issues |
| **Tool access restriction** | Only give a subagent the tools it needs; prevents accidental file modification |
| **Decision rule** | "Does the intermediate work matter?" — if no, delegate; if yes, keep in main thread |
| **Anti-patterns** | Expert personas, sequential pipelines, test runners — all hurt more than help |

---

## 📖 Table of Contents

1. [What are subagents?](#lesson-1-what-are-subagents) — 15 min
2. [Creating a subagent](#lesson-2-creating-a-subagent) — 20 min
3. [Designing effective subagents](#lesson-3-designing-effective-subagents) — 20 min
4. [Using subagents effectively](#lesson-4-using-subagents-effectively) — 15 min

Then: [Course Summary](#-course-summary) · [Quick Reference: Subagent Patterns](#-quick-reference-subagent-patterns)

---

## Lesson 1: What are subagents?

**Estimated time:** 15 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Define what a subagent is and how it differs from the main Claude Code thread
- Explain how a subagent's input and output flow between contexts
- List the three subagents that ship built-in with Claude Code
- Identify the three main benefits of using subagents

### Key Takeaways

- **Subagents are specialized assistants** that Claude Code can delegate tasks to. Each one runs in its own conversation context window, does its work, and returns a summary to the main thread
- The **intermediate steps stay isolated** — all the file reads, searches, and tool calls never clutter your main conversation
- A subagent receives two things: a **custom system prompt** (from your config file) and a **task description** (written by the parent agent based on what you asked for)
- When the subagent is done, **only a summary comes back** to your main conversation. The entire subagent conversation is then discarded
- The **tradeoff** is that you lose visibility into how the subagent reached its conclusions
- Claude Code ships with three built-in subagents: **General purpose** (multi-step tasks), **Explore** (codebase search), and **Plan** (plan-mode research)
- Beyond built-ins, you can **create your own subagents** with custom system prompts and tool access
- Subagents give you three main benefits: **focused work, clean context, concise summaries**

### Detailed Notes

Every time you chat with Claude Code, you're adding to the main context window. Every tool call, every file read, every search result gets stored there. That space is finite, and once it fills up, Claude starts losing track of earlier parts of the conversation. Subagents exist to solve this problem.

When a subagent is launched, it spins up a **separate context window** — a fresh conversation that has no memory of the main thread and won't pollute it. The subagent receives two inputs: a custom system prompt (defined in a config file) that sets up its role and behavior, and a task description (written by the parent agent) that tells it what to do. The subagent then does its work in isolation — reading files, running searches, editing code, whatever it needs. When it finishes, it sends a summary back to the main thread, and the entire subagent conversation is thrown away.

The net effect is a **much cleaner main context**. You asked a question, you got an answer, and your conversation history only contains the question and the answer — not the 15 files that were read or the dozen searches that were run to find the answer. The tradeoff is real but well-bounded: you lose visibility into *how* the subagent reached its conclusions, so for tasks where you need to see every step, subagents are the wrong tool.

#### A Practical Example

Imagine you're exploring an unfamiliar codebase and you want to know which service handles refunds. Without a subagent, Claude might read 15 files, run several searches, and trace through multiple function calls. All of that fills your context window, even though you only needed one fact. With a subagent, the experience is much cleaner: you ask the question, the `Explore` subagent spins up, does all that digging in its own context, and hands back a focused answer. Your main context window only records the question and the summary — not the 15 files that were read along the way.

#### Built-in Subagents

Claude Code ships with three built-in subagents you can use right away:

- **General purpose subagent** — for multi-step tasks that require both exploration and action
- **Explore** — for fast searching and navigation of codebases
- **Plan** — used during plan mode for research and analysis of your codebase before presenting a plan

These cover the most common delegation patterns out of the box. For anything more specialized, you build your own.

#### Custom Subagents

Beyond the built-in options, you can create your own subagents with custom system prompts and tool access. This lets you define specialized agents tailored to your workflow — a code reviewer, a test writer, a documentation generator, or anything else you need. Lesson 2 walks through the creation flow.

### Practical Tips

- **Don't reach for subagents by default.** For short, simple tasks, the overhead of spinning up a subagent isn't worth it. Use them when the task has a meaningful exploration phase
- **Trust the summary, but verify when it matters.** Subagents discard their intermediate work, so if the answer seems off, re-ask with more specifics in the input prompt
- **Combine built-ins first.** The three built-in subagents cover the majority of delegation cases — explore the codebase, plan changes, or run multi-step work. Only build a custom subagent when none of these fit
- **Think of subagents as outsourced work.** The main thread writes a brief, the subagent does the work, and the main thread gets a report. Briefs that are vague produce vague reports

### Put It Into Practice

Open a recent Claude Code conversation where you did a lot of exploration — reading multiple files, running searches, tracing through code paths. Re-run the same task but ask Claude to use the `Explore` subagent explicitly. Compare the two: how much of your main context window did the second run fill up versus the first?

---

## Lesson 2: Creating a subagent

**Estimated time:** 20 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Use the `/agents` slash command to open the subagent management interface
- Choose between project-level and user-level scope for a subagent
- Configure which tools a subagent can access
- Choose the right Claude model for a subagent
- Read and write a subagent config file in `.claude/agents/`
- Make Claude use your subagent automatically with the `proactively` keyword

### Key Takeaways

- **Custom subagents** are markdown files with YAML frontmatter that tell Claude when to use the subagent and how it should behave
- The **`/agents` slash command** is the easiest way to create a subagent — it opens the main management interface
- **Project-level scope** makes the subagent available only in the current project. **User-level scope** shares it across all projects on your machine
- The **recommended creation flow** is to describe what you want in plain language and let Claude generate the name, description, and system prompt
- **Tool categories** include read-only, edit, execution, MCP, and other tools — pick the smallest set that does the job
- **Model options** are Haiku (fast/lightweight), Sonnet (balanced), Opus (complex analysis), and Inherit (matches main conversation)
- A **color** is a UI aid so you can tell at a glance which subagent is active
- The subagent config file is saved as `.claude/agents/your-agent-name.md`
- The **system prompt** (the body of the markdown file) is where you give the subagent its actual instructions
- Include the word **"proactively"** in the description field to make Claude delegate to the subagent automatically
- If the subagent **isn't being used** when you expect, the description needs more specific examples and trigger scenarios

### Detailed Notes

Claude Code comes with built-in subagents, but the real power comes from creating your own. Custom subagents specialize in specific tasks — reviewing code, writing tests, checking documentation — and they are defined as plain markdown files with YAML frontmatter. The frontmatter tells Claude *when* to use the subagent; the body tells it *how* to behave.

#### The Creation Flow

The easiest way to create a subagent is with the `/agents` slash command. This opens the main interface for managing your subagents. From there, select **Create new agent**.

You will first be asked to choose the **scope** of your subagent:

- **Project-level** — available only in the current project. Config is saved to `.claude/agents/` inside the repo, so it gets shared with your team via version control
- **User-level** — shared across all projects on your machine. Config is saved to your home directory's `.claude/agents/` folder

Next, you can choose how to create it. You can write the configuration manually, but the **recommended approach is to let Claude generate it for you**. Just describe what you want the subagent to do, and Claude will produce a name, description, and system prompt based on your input. This is faster and tends to produce better first drafts because Claude knows the patterns of a good subagent config.

#### Customizing Tools

During creation, you get the chance to customize which tools the subagent can access. The tool categories include:

- **Read-only tools** — `Read`, `Glob`, `Grep`, `WebFetch`, `WebSearch`
- **Edit tools** — `Edit`, `Write`, `NotebookEdit`
- **Execution tools** — `Bash`
- **MCP tools** — any tools exposed by your connected MCP servers
- **Other tools** — anything else available in the current environment

Think about what your subagent actually needs. A code reviewer probably does not need edit tools — it should read and analyze code, not change it. However, you might want to keep execution tools enabled so it can more easily identify pending changes (e.g., running `git diff`).

#### Choosing a Model and Color

After configuring tools, you select which Claude model powers the subagent. The model picker offers four options:

- **Haiku** — best for fast, lightweight tasks (e.g., a commit-message generator)
- **Sonnet** — a good middle ground between speed and depth (the most common choice)
- **Opus** — best for complex analysis (e.g., a deep code reviewer)
- **Inherit** — uses whatever model your main conversation is running on

Finally, you pick a **color**. This shows up in the UI so you can quickly tell which subagent is active. It is a small touch, but it helps when you have multiple subagents running at once.

#### The Config File

Once creation is complete, the subagent config file is saved into your project (typically at `.claude/agents/your-agent-name.md`). Here is what a typical subagent config looks like:

```markdown
---
name: code-quality-reviewer
description: Use this agent when you need to review recently written or modified code for quality, security, and best practice compliance.
tools: Bash, Glob, Grep, Read, WebFetch, WebSearch
model: sonnet
color: purple
---

You are an expert code reviewer specializing in quality assurance, security best practices, and
adherence to project standards. Your role is to thoroughly examine recently written or modified code
and identify issues that could impact reliability, security, maintainability, or performance.
```

Let's break down each field:

- **name** — A unique identifier for the subagent. This is how you reference it, either by asking Claude directly or by typing `@agent code-quality-reviewer` in your message
- **description** — Controls when Claude decides to use the subagent. This must be a single line (use escaped newline characters `\n` if you need breaks). You can include example conversations here to help Claude understand when delegation is appropriate
- **tools** — Lists which tools the subagent can access. This matches whatever you selected during generation, but you can edit the list here at any time
- **model** — Specifies which Claude model to use: `sonnet`, `opus`, `haiku`, or `inherit`
- **color** — The UI color for identifying the subagent

#### System Prompts

The body of the markdown file (everything below the YAML frontmatter) is the system prompt. This is where you give the subagent its instructions: what it should focus on, how it should analyze things, and how it should report findings back to the main agent. A well-written system prompt is the difference between a useful subagent and one that misses the point. Be specific about what the subagent should look for and how it should structure its output.

#### Making Claude Use Your Subagent Automatically

If you want Claude to delegate tasks to the subagent without you explicitly asking, include the word **"proactively"** in the description field. For example:

```
description: Proactively suggest running this agent after major code changes...
```

You can also add example conversations to the description to help Claude understand specific scenarios where the subagent should be used. The more concrete your examples, the better Claude gets at knowing when to delegate.

#### Testing Your Subagent

After creating your subagent, test it by making some code changes and asking Claude to review them. If the subagent is not being used when you expect it to be, go back and check the description. Adding more specific examples and trigger scenarios helps Claude understand when to delegate work to your subagent.

### Practical Tips

- **Start with `Inherit` for the model.** Until you've measured a real reason to override it, match the main conversation — fewer surprises
- **Make the description concrete.** A vague description gets a vague trigger pattern. Include example scenarios and the specific phrase "proactively" if you want automatic delegation
- **Commit project-level subagents to git.** They're team assets. User-level subagents are personal preferences
- **Keep the system prompt specific.** "Be a good reviewer" is useless. "Look for SQL injection, missing input validation, and unhandled promise rejections" is actionable
- **Test with explicit invocation first.** Use `@agent your-agent-name` in your message to force the subagent. Once it works, the description-based auto-trigger becomes trustworthy

### Put It Into Practice

Build a `commit-message-writer` subagent in your home directory (`~/.claude/agents/`). Give it access to `Bash` and `Read` only, set the model to `haiku`, and write a system prompt that produces Conventional Commits format. Then make a few changes in a project and ask Claude to commit them — the subagent should run `git diff`, summarize the changes, and propose a commit message. Refine the description and system prompt until the messages match your team's style.

---

## Lesson 3: Designing effective subagents

**Estimated time:** 20 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Explain how the subagent's name and description shape both the trigger and the input prompt
- Write a description that produces specific, useful input prompts
- Define a structured output format that gives the subagent clear stopping points
- Add obstacle reporting to your output formats so the main thread doesn't rediscover workarounds
- Match tool access to a subagent's actual job (read-only / bash / edit-write)

### Key Takeaways

- The **name and description** of every available subagent are included in the main agent's system prompt — this is how the main agent decides which subagent to launch and when
- The **description does double duty**: it controls *when* the subagent runs *and* shapes *what* the subagent is told to do
- The single most important improvement you can make is **defining a structured output format** in the system prompt — it gives the subagent natural stopping points and prevents it from running too long
- **Obstacles, workarounds, and environment quirks** must be explicitly requested in the output format, or the main thread has to rediscover them
- **Tool access should match the job** — read-only for research, `Bash` for reviewers, `Edit`/`Write` only for code-modification agents
- Effective subagents share **four characteristics**: specific descriptions, structured output, obstacle reporting, and limited tool access

### Detailed Notes

Now that you know how to create subagents, let's look at the patterns that make them actually effective. A subagent that's poorly configured will wander, run too long, or produce output the main agent can't use. The fixes come down to four things: writing good descriptions, defining an output format, reporting obstacles, and limiting tool access.

#### How Subagent Config Data Gets Used

When you send a message to the main context window agent, the **name and description of every available subagent are included in the system prompt**. This is how the main agent decides which subagent to launch and when. If you want better control over when a subagent gets triggered automatically, the name and description are what you should tweak.

The description also plays a **second role**. When the main agent launches a subagent, it writes an input prompt to kick off the task. It uses the description as guidance for writing that prompt. So the description doesn't just control when a subagent runs — it shapes what the subagent is told to do. This is the most under-appreciated lever in subagent design.

#### Writing Descriptions That Shape Input Prompts

Consider a code review subagent. With a generic description, the main agent might write an input prompt like "use get diff to find the current changes." That's vague. The subagent has to figure out which files matter on its own.

If you update the description to include something like **"You must tell the agent precisely which files you want it to review,"** the main agent will now write a much more specific input prompt that lists the actual files to review.

This same technique works across different types of subagents. For example, adding **"return sources that can be cited"** to a web search subagent's description causes the main agent to include that instruction when delegating the task. The description is essentially a contract for how the parent agent talks to the subagent.

#### Defining an Output Format

The **single most important improvement** you can make to a subagent is defining an output format in its system prompt. This does two things:

1. **It creates natural stopping points** — the subagent knows it's done when it has filled in each section of the format
2. **It prevents the subagent from running too long** — without a defined output, subagents struggle to decide when enough research has been done and tend to run much longer than necessary

Here's an example of a structured output format for a code review subagent:

```markdown
Provide your review in a structured format:

1. Summary: Brief overview of what you reviewed and overall assessment
2. Critical Issues: Any security vulnerabilities, data integrity risks,
   or logic errors that must be fixed immediately
3. Major Issues: Quality problems, architecture misalignment, or
   significant performance concerns
4. Minor Issues: Style inconsistencies, documentation gaps, or
   minor optimizations
5. Recommendations: Suggestions for improvement, refactoring
   opportunities, or best practices to apply
6. Approval Status: Clear statement of whether the code is ready
   to merge/deploy or requires changes
```

This format gives the subagent a clear checklist to work through. Once every section is filled in, the subagent knows it can stop.

#### Reporting Obstacles

When a subagent discovers a workaround during its work — like solving a dependency issue or finding that a certain command needs particular flags — those details need to appear in the summary it returns. If they don't, the main thread has to rediscover the same solutions on its own, which wastes time and tokens.

The kinds of things you want surfaced include:

- **Setup issues or environment quirks**
- **Workarounds discovered during the task**
- **Commands that needed special flags or configuration**
- **Dependencies or imports that caused problems**

The way to get this information is to **explicitly ask for it in the output format**. Adding an "Obstacles Encountered" section to your output template surfaces this information reliably:

```markdown
7. Obstacles Encountered: Report any obstacles encountered during the
   review process. This can be: setup issues, workarounds discovered or
   environment quirks. Report commands that needed a special flag or
   configuration. Report dependencies or imports that caused problems.
```

#### Limiting Tool Access

Not every subagent needs access to every tool. Think about what a subagent actually needs to do, and only give it the tools required for that job. This does two things: it **prevents unintended side effects**, and it makes each subagent's role clearer when you have several of them.

Here's how to think about tool access for common subagent types:

- **Research / read-only subagent** — Only needs `Glob`, `Grep`, and `Read`. Cannot accidentally modify files
- **Code reviewer** — Needs `Bash` access to run `git diff` and see what changed, but still doesn't need `Edit` or `Write`
- **Styling / code modification agent** — This is where you give `Edit` and `Write` access, because the subagent's job is to actually change your code

#### Putting It All Together

Effective subagents share four characteristics:

1. **Specific descriptions** — The description controls when the subagent is launched and what instructions it receives. Write it to steer both
2. **Structured output** — Define an output format in the system prompt so the subagent knows when it's done and returns information the main thread can use
3. **Obstacle reporting** — Include a section in the output format for workarounds, quirks, and problems so the main thread doesn't have to rediscover them
4. **Limited tool access** — Only give a subagent the tools it actually needs. Read-only for research, bash for reviewers, edit/write only for agents that should change code

Each of these patterns is simple on its own, but together they turn a subagent from something that vaguely tries to help into a focused, predictable worker that finishes on time and reports back clearly.

### Practical Tips

- **The output format is the highest-leverage change.** If you only do one thing to a subagent, give it a numbered, fill-in-the-blank output structure
- **Tighten descriptions before tightening system prompts.** The description is read by the main agent; the system prompt is read by the subagent. If the main agent is launching the wrong subagent, fix the description; if the subagent is doing the wrong thing once launched, fix the system prompt
- **Reserve `Edit`/`Write` for agents that should change code.** Most "review" and "research" agents should be read-only or bash-only. The accidental-write failure mode is real
- **Number your output sections.** Numbered sections make it easy to refer to a specific part of a subagent's report ("see #4 — Minor Issues") and easier for the subagent to recognize when all sections are complete
- **Iterate the output format with real runs.** The first version is a guess. After three or four real uses, you'll know which sections you actually read and which are noise — cut the noise

### Put It Into Practice

Take a subagent you've already created (or the `code-quality-reviewer` from Lesson 2) and rewrite its system prompt with: (1) a 6–7 section structured output, (2) a dedicated "Obstacles Encountered" section, and (3) trimmed tool access to the minimum. Then run it on a real change. Compare the output to your previous run — does the report now have a predictable shape you can scan quickly? Did it surface anything you would have had to dig for?

---

## Lesson 4: Using subagents effectively

**Estimated time:** 15 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Apply the "does the intermediate work matter?" decision rule
- Recognize the three classic subagent anti-patterns (expert claims, sequential pipelines, test runners)
- Identify scenarios where a custom system prompt makes a subagent genuinely better than the main thread
- Build a subagent whose design is justified by a specific use case

### Key Takeaways

- The **decision rule** is one question: does the intermediate work matter?
- If the answer is **no** — you just need the final result — delegate it to a subagent
- If the answer is **yes** — you need to see and react to what's happening along the way — keep it in your main thread
- Subagents **excel** at research, code reviews, and tasks that need a custom system prompt
- Subagents **hurt** when used as expert personas, in sequential pipelines, or as test runners
- **Custom system prompts** make a real difference for copywriting and styling, where Claude Code's default tone and context aren't appropriate
- For **code review**, a subagent sees the code as if authored by someone else — which produces sharper feedback than asking the same thread that wrote the code
- **Bug fixing should stay in the main thread** because each step depends on what the previous step discovered

### Detailed Notes

You know how to create subagents and design them well. Now the question is: when do they actually help, and when do they get in the way? The difference comes down to one thing — whether the intermediate work matters to your main thread.

#### When Subagents Shine

Subagents work best when the **exploration is separate from the execution**. If each step in a task depends on what the previous step discovered, you want that work in your main thread. But if you just need an answer and don't care about the journey, delegate it.

Subagents excel at tasks where:

- You need a **result**, not a play-by-play of how it was found
- The exploratory work would **clutter your main thread's context**
- The task benefits from a **fresh perspective** or a custom system prompt

#### Research Tasks

Research is the classic subagent use case. Consider investigating how authentication works in an unfamiliar codebase. Your main thread needs to know where the JWT is validated, but it doesn't need to see every file that was searched along the way.

A research subagent can read dozens of files, trace through function calls, and explore different code paths. All that exploration stays in the subagent's context. Your main thread receives a clean summary like:

> JWT validation happens in middleware/auth.js line 42,
> called from the Express router in route/api.js

The subagent did the heavy lifting. Your main thread gets exactly what it needs to move forward.

#### Code Reviews

Claude reviews code more effectively when the code is presented as being authored by **someone else**. If you built a feature over many turns with your main thread, asking that same thread to review it often produces weak feedback. Claude was involved in creating it, so it has trouble seeing it with fresh eyes.

A reviewer subagent sees the changes in a separate context. It runs `git diff`, reads the modified files, and applies its specialized review criteria without the history of how the code was written. This separation also lets you encode project-specific review standards in the subagent's system prompt, ensuring consistent review criteria across the team.

#### Custom System Prompts

Claude Code's default system prompt emphasizes concise, code-focused responses. That works great for coding, but not for everything. Here are two cases where a custom system prompt makes the subagent genuinely better than the main thread:

- **Copywriting subagent** — Give it instructions about tone, audience, and style. Claude Code's default prompt tends toward concise technical writing, which really isn't what you want for a landing page or email campaign. A copywriting subagent can have completely different instructions about voice and structure
- **Styling subagent** — Point it at your design system files. When the subagent runs, those files load into its context automatically, so it knows your color variables, spacing conventions, and component patterns before it even starts writing any CSS

#### When Subagents Hurt

The overhead of launching a subagent — losing visibility into its work and compressing its findings into a summary — only makes sense when the subagent does something the main thread can't. There are three common anti-patterns to watch out for.

**Expert Claims**

Subagents that claim expertise rarely help. Prompts like "you are a Python expert" or "you are a Kubernetes specialist" add no value because Claude already has that knowledge. There's nothing a so-called expert subagent can do that your main thread can't do directly. If the goal is just to label Claude with a persona, do that in your main thread — a subagent adds overhead without value.

**Sequential Pipelines**

Sequential subagent pipelines create problems. Consider a three-agent flow: one to reproduce a bug, one to debug it, and one to fix it. Pipelines work when tasks are truly independent. They fail when each step depends on discoveries from the previous step — and bug fixing almost always does. Information gets lost in the handoff between agents. The reproducer's findings get compressed into a summary; the debugger doesn't see the raw output; the fixer only sees what made it through two compression steps.

**Test Runners**

Test runner subagents tend to hide information you need. When tests fail, you want the full output to diagnose issues. A subagent that returns "tests failed" forces you to create additional debug scripts to get details that would have been visible in direct output. Testing has shown that the test runner pattern performed worse among all configurations — the abstraction actively loses useful signal.

#### The Decision Rule

When you're deciding whether to use a subagent, ask yourself one question: **does the intermediate work matter?**

- If the answer is **no** — you just need the final result — delegate it to a subagent
- If the answer is **yes** — you need to see and react to what's happening along the way — keep it in your main thread

**Use subagents for:**

- Research and exploration
- Code reviews
- Tasks that need a custom system prompt

**Avoid subagents for:**

- "Expert" personas that don't add real capability
- Multi-step pipelines where each step depends on the last
- Running tests where you need full output for debugging

### Practical Tips

- **Default to "no" on the decision rule.** Most tasks have a meaningful exploration phase. Only delegate when the final answer is all you need
- **If a subagent's summary is too short, that's the design working.** Don't try to force subagents to expose their full work — that defeats the purpose. If you need the full output, do the work in your main thread
- **For copywriting and styling, custom system prompts are not optional.** Claude Code's defaults are tuned for code; leaning on the default for non-code output produces consistently mediocre non-code output
- **If you find yourself building a pipeline, stop and reconsider.** Almost every multi-agent pipeline I've seen would have been better as a single, focused conversation
- **Audit your existing subagents.** Any subagent whose system prompt is just "you are an X expert" is anti-pattern #1. Delete it

### Put It Into Practice

Look at your last five Claude Code conversations. For each, apply the decision rule: did the intermediate work matter? Mark each one as "should have been a subagent" or "correctly stayed in the main thread." If you find more than two "should have been a subagent" cases, build a project-level subagent for that pattern and run it for real.

---

## 📋 Course Summary

This course taught you how to use subagents in Claude Code as a tool for **context hygiene and focused delegation**. You learned that subagents are isolated context windows that do work and return summaries, keeping your main thread clean. You learned the three built-in subagents (`Explore`, `Plan`, `General purpose`) and how to build custom subagents with the `/agents` command, scoped to a project or a user, with explicit tool access and model selection.

The design patterns from Lesson 3 are the heart of the course: **specific descriptions, structured output, obstacle reporting, and limited tool access**. Together they turn a subagent from "something that vaguely tries to help" into "a focused, predictable worker that finishes on time and reports back clearly." The output format is the highest-leverage change — give the subagent a numbered, fill-in-the-blank structure and it will produce useful, scannable reports every time.

Lesson 4 gave you the **decision rule**: does the intermediate work matter? If no, delegate. If yes, keep it in the main thread. And you saw the three anti-patterns to avoid — expert personas, sequential pipelines, and test runners — all of which add overhead without adding value.

If you take away three things from this course, let them be these:

1. **Subagents exist to keep your main context clean.** They are not magic, they are not faster, and they are not more capable — they are *isolated*. Use them when isolation is what you need
2. **The output format is the design.** A subagent with a clear output structure will be useful; a subagent with a vague "review this" system prompt will wander. Spend the time on the output template
3. **Default to the main thread.** Most tasks have a meaningful exploration phase where you need to see what's happening. Delegate only when you genuinely don't care about the journey

---

## 🔧 Quick Reference: Subagent Patterns

### Subagent File Locations

| Scope | Location | Shared with |
|---|---|---|
| Project | `.claude/agents/<name>.md` in repo | Team via git |
| User | `~/.claude/agents/<name>.md` | All projects on machine |

### Config File Skeleton

```markdown
---
name: <agent-name>
description: <one-line description; include "proactively" for auto-trigger>
tools: <comma-separated tool list>
model: haiku | sonnet | opus | inherit
color: <ui color name>
---

<system prompt — what the subagent should focus on, how to analyze, and how to report>
```

### Frontmatter Field Reference

| Field | Required | Notes |
|---|---|---|
| `name` | Yes | Unique identifier; reference with `@agent name` |
| `description` | Yes | Single line; controls when subagent runs and shapes input prompt |
| `tools` | No | Comma-separated list; omit for default tool set |
| `model` | No | `haiku` / `sonnet` / `opus` / `inherit` (default) |
| `color` | No | UI hint when subagent is active |

### Tool Categories

| Category | Tools | Use for |
|---|---|---|
| Read-only | `Read`, `Glob`, `Grep`, `WebFetch`, `WebSearch` | Research, exploration, code review |
| Execution | `Bash` | Running `git diff`, build commands, scripts |
| Edit | `Edit`, `Write`, `NotebookEdit` | Code modification, file generation |
| MCP | Tools from connected MCP servers | Domain-specific actions |

### Built-in Subagents

| Subagent | Use when |
|---|---|
| `Explore` | You want to navigate or search a codebase without filling your main context |
| `Plan` | You're in plan mode and need research before presenting a plan |
| `General purpose` | You have a multi-step task that requires both exploration and action |

### Four Design Patterns

| Pattern | What it does | How to apply |
|---|---|---|
| Specific descriptions | Shapes both the trigger and the input prompt | Include "proactively" for auto-trigger; include example conversations and required phrasing for the input prompt |
| Structured output | Gives the subagent clear stopping points | Numbered, fill-in-the-blank sections in the system prompt |
| Obstacle reporting | Surfaces workarounds and environment quirks | Explicit "Obstacles Encountered" section in the output template |
| Limited tool access | Prevents unintended side effects | Read-only for research; `Bash` for reviewers; `Edit`/`Write` only for code-modification agents |

### Decision Rule

> **Does the intermediate work matter?**
>
> - **No** → delegate to a subagent
> - **Yes** → keep it in the main thread

### Use / Avoid Cheat Sheet

| ✅ Use subagents for | ❌ Avoid subagents for |
|---|---|
| Research and exploration | "Expert" personas |
| Code reviews | Multi-step pipelines where each step depends on the last |
| Tasks needing a custom system prompt (copywriting, styling) | Running tests where you need full output for debugging |

### Output Format Template (Code Review Example)

```markdown
Provide your review in a structured format:

1. Summary: Brief overview of what you reviewed and overall assessment
2. Critical Issues: Security vulnerabilities, data integrity risks, or logic errors that must be fixed immediately
3. Major Issues: Quality problems, architecture misalignment, or significant performance concerns
4. Minor Issues: Style inconsistencies, documentation gaps, or minor optimizations
5. Recommendations: Suggestions for improvement, refactoring opportunities, or best practices to apply
6. Approval Status: Clear statement of whether the code is ready to merge/deploy or requires changes
7. Obstacles Encountered: Setup issues, workarounds discovered, environment quirks. Report commands that needed a special flag or configuration. Report dependencies or imports that caused problems.
```

### Triggers and Invocation

- **Automatic:** Claude matches your request against the subagent's description; "proactively" in the description enables delegation without explicit asking
- **Explicit:** Use `@agent <name>` in your message to force a specific subagent, e.g. `@agent code-quality-reviewer please look at my staged changes`
- **Slash command:** `/agents` opens the management interface for creating, editing, and removing subagents

---

## 🔗 Related Resources

- **Course URL:** https://anthropic.skilljar.com/introduction-to-subagents
- **Companion course:** "Introduction to Agent Skills" — covers the related Skills feature, including how subagents and skills work together
- **Claude Code docs:** Subagent reference at the official Claude Code documentation
- **Built-in subagents:** `Explore`, `Plan`, `General purpose` — try them before building custom agents
