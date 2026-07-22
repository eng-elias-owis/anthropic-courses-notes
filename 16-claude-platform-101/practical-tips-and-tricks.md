# Claude Platform 101 — Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/claude-platform-101

---

## 📋 Course at a Glance

A developer-focused course covering the full Claude Platform: the Messages API, model selection, agent loops, tool use, extended thinking, built-in server tools, Skills, MCP, context management, and managed agents.

**Lessons:** 13 instructional + quiz

---

## 📌 Lesson-by-Lesson Insights

### 1. What is the Claude Developer Platform?

The platform is three stacked layers: **Primitives** (Messages API, tools, files, web search, MCP), **Infrastructure** (managed agents, retries, queues, observability), and **Controls** (dashboards, evals, analytics). The shorthand: *build with primitives, scale on infrastructure, run with controls.*

💡 Even a simple feature like "draft a help-desk reply from a ticket" is a perfect Messages API use case — you don't need the full agent stack until your scale demands it.

---

### 2. Your first API call

Every call goes through `messages.create` with three things: a model name, a `max_tokens` cap, and a messages list. The response `content` is an array of typed blocks — never a plain string. Always loop through and check the block type before reading text.

💡 Never hardcode your API key. Store it in `.env` / `.env.local` and keep it out of version control. A leaked key on GitHub is a real, common incident.

💡 The system prompt is where you define the persona. "You are a terse senior code reviewer" beats "help me review code" every time.

---

### 3. Choosing the right model

| Tier | Best for | Trade-off |
|---|---|---|
| **Haiku** | Classification, extraction, routing | Fastest, cheapest, lower quality |
| **Sonnet** | Most production work | Balanced |
| **Opus** | Deep reasoning, complex coding, nuance | Slowest, most expensive |
| **Fable** | Toughest challenges | Above Opus cost |

💡 **Evaluation-first workflow:** before writing production code, run 20–30 representative examples through each tier starting from Haiku. Stop at the first tier where quality holds. This saves significant money.

💡 In a real app, route *different work* to different models in the same endpoint — e.g., classify with Haiku, then analyze with Sonnet only if the classifier flags it.

---

### 4. The agent loop explained

An agent loop is just a `while` loop: send message → check `stop_reason` → if `tool_use`, execute tool and append result → repeat until `end_turn`. Claude never runs tools itself — your code does.

💡 The shape is always the same: user kicks off → Claude responds or calls a tool → tool returns → Claude continues. Keep this mental model and you can debug any agentic failure.

---

### 5. What is tool use?

Tools are JSON schemas with three parts: `name`, `description`, and `input_schema`. The **description** is what Claude reads to decide whether to call the tool. Vague descriptions = wrong tool calls. Be specific.

💡 Use a **tool runner** to skip hand-writing JSON schemas for every function. The tool runner handles the agent loop boilerplate automatically.

💡 When Claude returns `stop_reason: "tool_use"`, grab `tool_use` blocks from `content`, run each, then post back a `user` message containing `tool_result` blocks tied to each tool call's `id`.

---

### 6. What is thinking?

Extended thinking lets Claude reason step-by-step before responding. With Opus 4.x, thinking is **adaptive** — you don't set a token budget; you set an `effort` level: `low`, `medium`, `high`, `xhigh`, or `max`. The `effort` param goes inside `output_config`, not next to `thinking`.

💡 **Use thinking when:** the task involves trade-offs, options comparison, or multi-step reasoning. **Skip thinking for:** classification, extraction, boilerplate — it just adds latency and cost.

💡 In a compliance app, adaptive thinking lets Claude cross-reference specs across sections — catching conflicts a single-pass response would miss.

---

### 7. Built-in tools

Server tools (web search, code execution) run on Anthropic's infrastructure — you don't need an agent loop for them. Claude calls them server-side and the result comes back inside the same response. Client tools run where your code runs and require an agent loop.

💡 For web search, declare `{"type": "web_search_20260209", "name": "web_search"}` in your `tools` array — no loop needed. The response already contains the search result and citations.

💡 For code execution, Claude writes and runs Python in a sandboxed environment. Useful for math, data processing, or anything requiring reliable computation.

---

### 8. Skills

A Skill is a folder of instructions, scripts, and resources with a `SKILL.md` at its core. Upload once with `client.beta.skills.create()`, then reference the returned `skill.id` in your requests. Skills attach inside the `container` config as a `skills` array.

💡 Skills ≠ Tools: Tools are *what Claude can do* (run code, query a DB). Skills are *how you want it done* (your report format, your review checklist, your release notes template).

💡 Skills load lazily — only name + description appear at first; the full skill content loads only when Claude decides it's relevant. This keeps context lean even with many skills registered.

---

### 9. MCP

MCP (Model Context Protocol) shifts integration maintenance to the service provider. Instead of writing and maintaining wrappers for Asana, Slack, and Google Calendar yourself, each service publishes its own MCP server. When their API changes, they update the server — you change nothing.

💡 Tools = your internal stuff. Skills = your processes. MCP = everyone else's stuff.

💡 Filter which tools Claude can access from an MCP server using the `allowed_tools` list inside the `mcp_toolset` config — critical for scoping permissions and keeping context lean.

---

### 10. Context management

Four patterns for staying inside the context window:

1. **Just-in-time context** — Don't front-load everything. Let Claude pull specifics via tools when needed.
2. **Server-side compaction** — Add `context_management: {edits: [{type: "compact"}]}` to auto-summarize long conversations on the server side.
3. **Prompt caching** — Mark stable parts of a request (system prompt, tool defs, large docs) as cacheable. At 100 calls/hour on a 4,000-token system prompt, caching is the difference between a manageable bill and a call from finance.
4. **Explicit summarization** — Manually summarize and inject a condensed history at session checkpoints.

💡 You pay for tokens both in and out. Design your context deliberately — fit the *right* things in, not everything.

---

### 11. What are managed agents?

Managed agents are agent loops that run on Anthropic's infrastructure instead of your server. You define the agent, create a session, and stream events back. Sessions run in isolated containers with file systems, tools, and mounts — two sessions run in parallel without affecting each other.

💡 Key shift: you're not running a `while` loop. You're sending events and reading events.

💡 Managed agents support memory stores — agents can read what they found last week and build on it, enabling recurring research and tracking workflows.

---

### 12. Building your first managed agent

Four primitives in order: **Agent** (model + system prompt + toolset, reusable across runs) → **Environment** (container, networking) → **Session** (one unit of work) → **Events** (stream of actions and results).

💡 Use the agent toolset (`type: "agent_toolset"`) for bundled file, bash, and web tools — no custom definitions needed for basic tasks.

💡 Events stream back in real time — you can pipe them to a UI (Kanban board, dashboard, chat) while the agent is still working.

---

### 13. Building with Claude Code

Claude Code can write API integrations from a stub file with a single, well-structured prompt. A good prompt: names the file, names the pattern (tool runner + SDK), and names the expected end state. Claude Code reads errors and patches code in place.

💡 Use the built-in **Claude API skill** (`/claude-api`) to get Claude Code to follow the right SDK patterns automatically. Add it from the marketplace if missing: `/plugin marketplace add AnthropicsSkills` (note the trailing *s*).

---

## 💡 Key Principles Across the Course

- **Start simple, scale up** — Haiku → Sonnet → Opus. Only reach for the bigger model when the smaller one fails your evaluation.
- **Your code runs tools, not Claude** — Claude requests; you execute. This is the mental model for all agentic work.
- **The description is the interface** — For tools, MCP, and Skills alike, what Claude reads to decide what to do is always a description you wrote. Write it precisely.
- **Context is money** — Every token you send costs. Cache stable content, compact long conversations, and load context just-in-time.
- **Managed agents ≠ writing a loop** — They're an event-driven API. Learn to think in events and sessions, not while loops.

---

*Tips extracted from Claude Platform 101 course content.*
