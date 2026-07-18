# Claude Platform 101 — Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/claude-platform-101

---

## 📋 Course at a Glance

Anthropic Academy course on the Claude Developer Platform — APIs, models, agent loops, tool use, thinking, built-in tools, Skills, MCP, context management, and managed agents.

**Lessons:** 13 instructional + quiz

---

## 🔑 Key Takeaways by Lesson

### 1. What is the Claude Developer Platform?

- The Claude Developer Platform is Anthropic's infrastructure for building with Claude programmatically. Instead of chatting with Claude in a browser, you send structured requests from your code and get structured responses back — with control over every detail: which model to use, how many tokens to spend, what tools Claude can use, and what system instructions it follows.
- **Command line interfaces** — A console where you manage API keys, monitor usage, deploy managed agents, and test prompts
- **The three layers of the platform** — A useful way to picture the platform is as three layers stacked on top of each other. Primitives — the API building blocks tuned to Claude.
- **A real example: drafting help desk replies** — Say you manage a basic help desk app, and you've been asked to add a feature: draft a reply based on the contents of a ticket, following your team's tone and guidelines. You want to wire this up to a button in the UI.
- **Return the response to the button to render** — client = anthropic.Anthropic() response = client.messages.create( model="claude-haiku-4-5",   # Haiku: a good fit for a simple drafting task max_tokens=1024, system=TONE_AND_GUIDELINES, messages=[ {"role": "user", "content": ticket_content} ], ) draft = response.content Each parameter does a specific job: model — which model handles the request. Here that's Haiku, since drafting a reply is a simple task.

### 2. Your first API call

- Take the API key and store it in a .env.local file so it stays out of your version control. Hardcoding keys in source files is how they end up leaked on GitHub — keep them in environment files instead.
- **The anatomy of a request** — Every API call goes through the messages.
- **A max tokens limit — a cap on how long the response can be** — A list of messages — objects with either user or assistant roles, structured similarly to how you'd have a conversation with Claude elsewhere Here's what that looks like in its most basic form: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const msg = await client.
- **A real example: reviewing buggy code** — Let's give Claude something a little more interesting than "hello." We'll point it at some buggy code and ask for a review. Here's the whole thing — one file, about 20 lines of code: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const buggyCode = ` function add(a, b) { return a - b; } `; const response = await client.
- **From script to product** — In a real product, this same messages.create shape is the engine behind something like a summarize endpoint. Pull a meeting transcript out of the database, hand it to Claude with a system prompt that says "extract insights and risks," save the result back on the row, and return it to the UI.

### 3. Choosing the right model

- You're shipping an app with Claude. Which model do you pick?
- **The model tiers** — Anthropic currently offers four model tiers, and you choose between them with the model parameter in your API call. Note that at the time of this course, Claude Fable was not generally available, and are not reflected in the video above.
- **Start with a simple evaluation** — Before you write production code, set up a simple evaluation: a set of example inputs that you run through each model and score against what good output means for your use case. You don't need anything fancy — 20 or 30 representative examples from your actual workload is enough to start.
- **Comparing the tiers side by side** — Let's see the difference between the tiers, not just talk about it. We'll send the same prompt through all three models and watch the latency and token counts: models = ["claude-haiku-4-5", "claude-sonnet-4-6", "claude-opus-4-7"] for model in models: response = client.
- **Routing different work to different models** — In a real app, you'd route different kinds of work to different models inside the same endpoint. Take an operations dashboard with a document processing route: Every incoming file gets classified with Haiku.

### 4. The agent loop explained

- You've made API calls, but a single call only returns one response. If you want to automate a workflow, Claude needs to act, look at the result, decide what's next, and keep going.
- **What an agent actually is** — An agent is an autonomous version of Claude, running both sides of the messaging loop without a human in the middle. An agent receives a task, picks a tool, and executes code in a loop until Claude decides the task is done.
- **A minimal working example** — To see this loop run end to end without dragging in a database or a UI, we'll wire up a fake tool called get_weather and ask Claude what to wear in Austin today. Claude has no way to know the weather on its own, so it has to call the tool, read the result, and then give you an answer.
- **Running it** — When you run the script, you'll see two turns: Turn one: the stop reason is tool_use. Claude requests get_weather for Austin, and your code returns the temperature and conditions.
- **The same loop in production** — In a real environment, this same loop powers something like an auto-review endpoint: a compliance agent that reads a structural report, looks up the relevant building codes via a tool, and writes risk findings back to the database one by one as it works. The shape of the loop is identical to what you just ran.

### 5. What is tool use?

- Your existing workflows rely on a lot of different technologies — project management software, databases, files. Claude can't just check these things itself.
- **What a tool is** — Simply put, a tool is a function you define and expose to Claude. You describe what it does and what inputs it takes, and Claude decides when to call it.
- **How tools are defined** — Tools are JSON schemas with three parts: a name, a description, and an input schema. You pass them to Claude in the request body as a tools array.
- **Multiple tools: letting Claude pick** — One tool is useful, but the interesting part is giving Claude multiple tools and watching it pick which one to use, in what order. Picture this scenario: you're packing for a three-day trip to Denver, and you want both today's weather and the forecast for the next few days.
- **The tool runner: skip the boilerplate** — You've probably already spotted two red flags with what we just wrote: That's a lot of code for two simple lookups. In a real codebase, you don't want to handwrite JSON schemas for every function you have.

### 6. What is thinking?

- Some tasks need more than a quick answer. Claude can work through a problem before responding — a feature called extended thinking.
- **Adaptive thinking on Opus 4.7** — With Opus 4.7, thinking is adaptive. You don't pick a token budget.
- **Anything that involves trade-offs or comparing options** — Skip it for simple classification, extraction, or boilerplate. For those tasks it just adds latency and cost without actually improving the results.
- **Thinking in action** — Let's see it work. Here's an agent loop with one weather tool, and we'll ask Claude to plan a road trip out of San Francisco — two stops, weighing weather and drive time.
- **Why this matters in production** — In a production app, this is the difference between an agent that finds problems one at a time and an agent that connects them. Take a compliance review app: toggling adaptive thinking on the auto-review call lets the agent reason across report sections — catching things like a wind load spec in section three that conflicts with the material spec elsewhere in the document.

### 7. Built-in tools

- You can build your own custom tools, but some capabilities are common enough that Anthropic ships them pre-built. You don't write the code.
- **Server tools: declared by you, run by Anthropic** — Anthropic provides server tools that run on their infrastructure. You don't execute these — Anthropic does.
- **Two server tools in one file** — Let's check out some of the big ones in one file: two messages.create calls, one with web search and one with code execution. import anthropic client = anthropic.Anthropic() # Call 1: web search — Anthropic runs the search server-side search_response = client.messages.create( model="claude-opus-4-8", max_tokens=1024, tools=[{"type": "web_search_20260209", "name": "web_search"}], messages=[ {"role": "user", "content": "What is Anthropic's latest model release? Answer in one sentence.
- **Running it** — For web search, you'll see Claude's tool call printed, then a one-sentence answer about the latest model release with the search citations folded in. For code execution, you'll see the actual Python Claude wrote, the stdout from the sandbox running it, and a final text answer.
- **The other category: client tools** — Worth knowing the other category exists. Client tools run where your code runs.

### 8. Skills

- Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. At the core of every Skill is a SKILL.
- **Skills vs. tools** — It's worth being clear on the difference, because the two solve different problems: Tools connect Claude to data and actions. "Look up this code section," "send this email" — Claude calls the tool, and something else runs. Skills teach Claude a procedure.
- **Uploading a Skill** — Skills are uploaded once to your workspace, then referenced by ID. You can upload directly on the Claude Platform, or do it programmatically: skill = client.
- **Attaching a Skill to a request** — Skills attach to a request through the container configuration — a skills array inside the container, where each entry names a skill_id and version. Here's the full call for the status report generator: response = client.
- **Running it** — The output is a status report formatted exactly the way the Skill says to format it. Sections, tone, blocker handling — all of it comes from the SKILL.

### 9. MCP

- We have tools, skills, and connectors. So why does MCP exist?
- **The maintenance problem** — Say your agent needs to pull tasks from Asana, check a Google Calendar, and search Slack — all in one go. With custom tools, you have to write three integrations.
- **Tools vs. skills vs. MCP** — These three features do different jobs: Tools connect Claude to your internal systems — your database, your project tracker, your proprietary APIs. You own the code, so you also own the maintenance.
- **Connecting to an MCP server** — The cleanest way to get a feel for MCP is to point Claude at any MCP server and let it discover what's there. For this example, we'll use the Linear MCP server, with the connection details and auth token stored in a .
- **Filtering which tools Claude can use** — MCP servers often expose many, many tools — and you don't always want Claude using all of them. Maybe you don't want it to have write permissions, or you just don't want all those tool definitions taking up context.

### 10. Context management

- Every request you send Claude has a context window. A million tokens sounds like a lot, but it runs out faster than you think once you're shipping a real agent.
- **Thinking blocks** — It's the input to every single API call. You pay for it on the way in, and you pay for it on the way out.
- **Pattern 1: Just-in-time context** — Don't load everything upfront. Load what the agent needs now, and let it pull more in via tools when it asks.
- **Pattern 2: Server-side compaction** — When a conversation runs long, Anthropic's server-side compaction summarizes old turns into a single block. You opt in by adding a context_management key to your request, holding an edit with a type: response = client.
- **Pattern 3: Prompt caching** — Prompt caching lets you mark the stable parts of a request — the system prompt, the tool definitions, a long document — and reuse them across calls at a fraction of the cost. The math matters more than it looks.

### 11. What are managed agents?

- Under the hood, this is an agent loop: Claude reasons, calls a tool, reads the result, and repeats until the job is done. If you've built agents before, you've probably written this kind of loop yourself.
- **Example 1: A Kanban board that does the work** — Picture a Kanban board sitting on top of managed agents. You drag a ticket into the "in progress" column, and that fires off a session automatically.
- **All images lazy loaded** — Claude runs the audit, then starts compressing images, inlining CSS, and deferring scripts. Every tool call streams back to the board in real time through the event stream, so you can watch the work as it happens.
- **Example 2: A recurring research agent with memory** — Here's a different shape of agent: one whose job is to track prices and plan changes across every SaaS tool your company pays for, with a report ready before stand-up.
- **Uses an Excel spreadsheet skill and writes an executive summary** — Posts a link to Slack and creates a review task in Asana, both through MCP servers The agent also reads from and writes to a memory store. Before it starts, it checks what it found last week.

### 12. Building your first managed agent

- If you've built an agent loop by hand, you know the drill: while loops, stop reason switches, tool executions. That works, and for a lot of features it's actually the right shape.
- **The four primitives** — There are four primitives, and they come in order: Agent — the persona: model, system prompt, and toolset. This is reusable across many runs.
- **The smallest possible managed agent** — Let's build the smallest managed agent that does something useful: create a file in the temp drive, count its lines, and report back. For tools, we'll use the agent toolset — Anthropic's bundled file, bash, and web tools.
- **Step 1: Create the agent** — First, we create the agent. Note the agent toolset defined right in the tools array — that's the bundled toolset: import anthropic client = anthropic.
- **Step 2: Create the environment** — Next, the environment. This spins up the container template — cloud, with unrestricted networking.

### 13. Building with Claude Code

- Writing code that calls the Claude API by hand works fine, but there's an even faster path: have Claude write it for you. In this lesson, we'll use Claude Code to fill in an API integration from a stubbed-out file — using the same primitives you've learned throughout this course.
- **Starting from a stub** — The project is simple: a TypeScript file that gets weather. It contains two stubs: getWeather — accepts a city and returns the temperature and conditions.
- **The Claude API skill** — Claude Code comes with a built-in skill called Claude API. You can invoke it directly with /claude-api, or Claude Code will invoke it automatically when it detects that you're using the TypeScript SDK.
- **One prompt, working code** — Open the project folder in your terminal and launch Claude Code. From there, it takes a single prompt.
- **What Claude Code produced** — In this run, Claude Code created a Zod tool that parsed the input and returned the output based on the city type. It also created the tool runner and the run function we asked for, and printed the final results of the agent loop.

---

## 💡 Universal Tips for Working with AI

- **Start with the goal, not the prompt** — be clear about what outcome you want
- **Iterate** — first outputs are drafts, not final answers
- **Verify facts independently** — AI is confident but not always correct
- **Stay in the loop** — always review AI output before acting on it
- **Be transparent** — disclose AI use to collaborators and stakeholders
- **Match AI use to value** — delegate tasks where AI genuinely helps, not for its own sake

---

*Tips extracted from Claude Platform 101 course content.*
