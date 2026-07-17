# Introduction to Subagents - Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/introduction-to-subagents

---

## 📋 Course at a Glance

Learn how Claude's subagent capabilities work, how to design multi-agent systems, orchestration patterns, parallelization, and building effective agent pipelines for complex tasks.

---

## 🔑 Key Takeaways by Lesson

**From 'What are subagents?':**

Subagents give you three main benefits:

---

## 📌 Lesson-by-Lesson Insights

### What are subagents?

Subagents are specialized assistants that Claude Code can delegate tasks to. Think of them as focused helpers: each one runs in its own conversation context window, does its work, and returns a summary to the main thread. The intermediate steps -- all the file reads, searches, and tool calls -- stay isolated and never clutter your main conversation.

---

### Creating a subagent

Claude Code comes with built-in subagents, but you can also create your own. Custom subagents specialize in specific tasks -- like reviewing code, writing tests, or checking documentation. They are defined as markdown files with YAML frontmatter that tell Claude when to use the subagent and how the subagent should behave.

---

### Designing effective subagents

Now that you know how to create subagents, let's look at the patterns that make them actually effective. A subagent that's poorly configured will wander, run too long, or produce output the main agent can't use. The fixes come down to four things: writing good descriptions, defining an output format, reporting obstacles, and limiting tool access.

---

### Using subagents effectively

You know how to create subagents and design them well. Now the question is: when do they actually help, and when do they get in the way? The difference comes down to one thing -- whether the intermediate work matters to your main thread.

---

## 🚀 How to Apply This in Real Projects

1. **Start small** — Pick one concept from each lesson and apply it immediately
2. **Iterate** — First attempts rarely perfect; refine based on results
3. **Document patterns** — Keep notes on what prompts/approaches work best for your use case
4. **Build on examples** — Use course examples as templates for your own work
5. **Connect concepts** — Look for how lessons build on each other to form complete workflows

---

*Tips extracted from course content at https://anthropic.skilljar.com/introduction-to-subagents*
