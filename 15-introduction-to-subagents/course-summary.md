# Introduction to Subagents - Course Summary

**Course URL:** https://anthropic.skilljar.com/introduction-to-subagents

---

## 🎯 Course Overview

Learn how Claude's subagent capabilities work, how to design multi-agent systems, orchestration patterns, parallelization, and building effective agent pipelines for complex tasks.

---

## 📚 Table of Contents

  📌 What are subagents?
  📌 Creating a subagent
  📌 Designing effective subagents
  📌 Using subagents effectively

---

## 📖 Lesson Content

#### 1. What are subagents?

Subagents are specialized assistants that Claude Code can delegate tasks to. Think of them as focused helpers: each one runs in its own conversation context window, does its work, and returns a summary to the main thread. The intermediate steps -- all the file reads, searches, and tool calls -- stay isolated and never clutter your main conversation.

Why Subagents Matter

Every time you chat with Claude Code, you're adding to the main context window. Every tool call, every file read, every search result gets stored there. That space is finite, and once it fills up, Claude starts losing track of earlier parts of the conversation.

Subagents solve this by spinning up a separate context window. The subagent receives two things:

- A custom system prompt from your configuration file that defines the subagent's role and behavior
- A task description written by the parent agent based on what you asked for

The subagent then works on its own. It reads files, runs searches, edits code -- whatever it needs to do. When it's done, only a summary comes back to your main conversation. The entire subagent conversation is then discarded.

This means your main context stays clean. You get the answer without all the noise of the journey it took to find it. The tradeoff is that you lose visibility into how the subagent reached its conclusions.

A Practical Example

Say you're exploring an unfamiliar codebase and you want to know which service handles refunds. Without a subagent, Claude might read 15 files, run several searches, and trace through multiple function calls. All of that fills your context window, even though you only needed one fact.

With a subagent, the experience is much cleaner. You ask the question, the Explore subagent spins up, does all that digging in its own context, and hands back a focused answer.

Your main context window only records the question and the summary -- not the 15 files that were read along the way.

Built-in Subagents

Claude Code ships with several built-in subagents you can use right away:

- General purpose subagent -- for multi-step tasks that require both exploration and action
- Explore -- for fast searching and navigation of codebases
- Plan -- used during plan mode for research and analysis of your codebase before presenting a plan

Custom Subagents


> *(See full lesson at course URL)*

#### 2. Creating a subagent

Claude Code comes with built-in subagents, but you can also create your own. Custom subagents specialize in specific tasks -- like reviewing code, writing tests, or checking documentation. They are defined as markdown files with YAML frontmatter that tell Claude when to use the subagent and how the subagent should behave.

Creating a Subagent

The easiest way to create a subagent is with the /agents slash command. This opens the main interface for managing your subagents. From there, select Create new agent.

You will first be asked to choose the scope of your subagent:

- Project-level -- available only in the current project
- User-level -- shared across all projects on your machine

Next, you can choose how to create it. You can write the configuration manually, but the recommended approach is to let Claude generate it for you. Just describe what you want the subagent to do, and Claude will produce a name, description, and system prompt based on your input.

Customizing Tools

During creation, you get the chance to customize which tools the subagent can access. The tool categories include:

- Read-only tools
- Edit tools
- Execution tools
- MCP tools
- Other tools

Think about what your subagent actually needs. A code reviewer probably does not need edit tools -- it should read and analyze code, not change it. However, you might want to keep execution tools enabled so it can more easily identify pending changes.

Choosing a Model and Color

After configuring tools, you select which Claude model powers the subagent. The model picker offers four options:

- Haiku -- best for fast, lightweight tasks
- Sonnet -- a good middle ground between speed and depth
- Opus -- best for complex analysis
- Inherit -- uses whatever model your main conversation is running

Finally, you pick a color. This shows up in the UI so you can quickly tell which subagent is active. It is a small touch, but it helps when you have multiple subagents running.

The Config File

Once creation is complete, the subagent config file is saved into your project (typically at .claude/agents/your-agent-name.md). Here is what a typical subagent config looks like:

---
name: code-quality-reviewer
description: Use this agent when you need to review recently written or modified code for quality, security, and best practice compliance.
tools: Bash, Glob, Grep, Read, WebFetch, WebSearch
model: sonnet
color: purple
---


> *(See full lesson at course URL)*

#### 3. Designing effective subagents

Now that you know how to create subagents, let's look at the patterns that make them actually effective. A subagent that's poorly configured will wander, run too long, or produce output the main agent can't use. The fixes come down to four things: writing good descriptions, defining an output format, reporting obstacles, and limiting tool access.

How Subagent Config Data Gets Used

When you send a message to the main context window agent, the name and description of every available subagent are included in the system prompt. This is how the main agent decides which subagent to launch and when. If you want better control over when a subagent gets triggered automatically, the name and description are what you should tweak.

The description also plays a second role. When the main agent launches a subagent, it writes an input prompt to kick off the task. It uses the description as guidance for writing that prompt. So the description doesn't just control when a subagent runs -- it shapes what the subagent is told to do.

Writing Descriptions That Shape Input Prompts

Consider a code review subagent. With a generic description, the main agent might write an input prompt like "use get diff to find the current changes." That's vague. The subagent has to figure out which files matter on its own.

If you update the description to include something like "You must tell the agent precisely which files you want it to review," the main agent will now write a much more specific input prompt that lists the actual files to review.

This same technique works across different types of subagents. For example, adding "return sources that can be cited" to a web search subagent's description causes the main agent to include that instruction when delegating the task.

Defining an Output Format

The single most important improvement you can make to a subagent is defining an output format in its system prompt. This does two things:

- It creates natural stopping points -- the subagent knows it's done when it has filled in each section of the format.
- It prevents the subagent from running too long. Without a defined output, subagents struggle to decide when enough research has been done and tend to run much longer than necessary.

Here's an example of a structured output format for a code review subagent:

Provide your review in a structured format:

1. Summary: Brief overview of what you reviewed and overall assessment

> *(See full lesson at course URL)*

#### 4. Using subagents effectively

You know how to create subagents and design them well. Now the question is: when do they actually help, and when do they get in the way? The difference comes down to one thing -- whether the intermediate work matters to your main thread.

When subagents shine

Subagents work best when the exploration is separate from the execution. If each step in a task depends on what the previous step discovered, you want that work in your main thread. But if you just need an answer and don't care about the journey, delegate it.

Subagents excel at tasks where:

- You need a result, not a play-by-play of how it was found
- The exploratory work would clutter your main thread's context
- The task benefits from a fresh perspective or a custom system prompt

Research tasks

Research is the classic subagent use case. Consider investigating how authentication works in an unfamiliar codebase. Your main thread needs to know where the JWT is validated, but it doesn't need to see every file that was searched along the way.

A research subagent can read dozens of files, trace through function calls, and explore different code paths. All that exploration stays in the subagent's context. Your main thread receives a clean summary like:

JWT validation happens in middleware/auth.js line 42,
called from the Express router in route/api.js

The subagent did the heavy lifting. Your main thread gets exactly what it needs to move forward.

Code Reviews

Claude reviews code more effectively when the code is presented as being authored by someone else. If you built a feature over many turns with your main thread, asking that same thread to review it often produces weak feedback. Claude was involved in creating it, so it has trouble seeing it with fresh eyes.

A reviewer subagent sees the changes in a separate context. It runs git diff, reads the modified files, and applies its specialized review criteria without the history of how the code was written. This separation also lets you encode project-specific review standards in the subagent's system prompt, ensuring consistent review criteria across the team.

Custom System Prompts

Claude Code's default system prompt emphasizes concise, code-focused responses. That works great for coding, but not for everything.

Here are two cases where a custom system prompt makes the subagent genuinely better than the main thread:


> *(See full lesson at course URL)*


---

*Summary generated from course content at https://anthropic.skilljar.com/introduction-to-subagents*
