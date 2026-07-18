# Claude Platform 101 — Course Summary

**Course URL:** https://anthropic.skilljar.com/claude-platform-101

---

## 🎯 Course Overview

Anthropic Academy course on the Claude Developer Platform — APIs, models, agent loops, tool use, thinking, built-in tools, Skills, MCP, context management, and managed agents.

---

## 📚 Table of Contents

1. What is the Claude Developer Platform?
2. Your first API call
3. Choosing the right model
4. The agent loop explained
5. What is tool use?
6. What is thinking?
7. Built-in tools
8. Skills
9. MCP
10. Context management
11. What are managed agents?
12. Building your first managed agent
13. Building with Claude Code

---

## 📖 Lesson Content

### Lesson 1: What is the Claude Developer Platform?

The Claude Developer Platform is Anthropic's infrastructure for building with Claude programmatically. Instead of chatting with Claude in a browser, you send structured requests from your code and get structured responses back — with control over every detail: which model to use, how many tokens to spend, what tools Claude can use, and what system instructions it follows. Concretely, the platform is made up of a few pieces:

**Command line interfaces**

A console where you manage API keys, monitor usage, deploy managed agents, and test prompts

**The three layers of the platform**

A useful way to picture the platform is as three layers stacked on top of each other. Primitives — the API building blocks tuned to Claude. This is the Messages API, tool use, files, web search, code execution, MCP servers, and skills. These are the pieces you actually call from your code. Infrastructure — what you need to build and scale agentic systems past a prototype. Managed agents, retries, queues, observability — the plumbing that keeps things running when one Claude call becomes a thousand. Controls — the tools for running those systems in production, like dashboards and evals.

These are the dials your team uses once it's live. The shorthand: build with primitives, scale on infrastructure, run with control. You can see this structure reflected in the Claude Console itself — it's where the infrastructure and control layers live, with sections for building, managing agents, and analytics.

**A real example: drafting help desk replies**

Say you manage a basic help desk app, and you've been asked to add a feature: draft a reply based on the contents of a ticket, following your team's tone and guidelines. You want to wire this up to a button in the UI. This is a perfect use case for the Messages API. The flow looks like this:

**Return the response to the button to render**

client = anthropic.Anthropic() response = client.messages.create( model="claude-haiku-4-5",   # Haiku: a good fit for a simple drafting task max_tokens=1024, system=TONE_AND_GUIDELINES, messages=[ {"role": "user", "content": ticket_content} ], ) draft = response.content Each paramet…

**Key Takeaways:**

- The Claude Developer Platform is Anthropic's infrastructure for building with Claude programmatically. Instead of chatting with Claude in a browser, you send structured requests from your code and get structured responses back — with control over every detail: which model to use, how many tokens to spend, what tools Claude can use, and what system instructions it follows.
- **Command line interfaces** — A console where you manage API keys, monitor usage, deploy managed agents, and test prompts
- **The three layers of the platform** — A useful way to picture the platform is as three layers stacked on top of each other. Primitives — the API building blocks tuned to Claude.
- **A real example: drafting help desk replies** — Say you manage a basic help desk app, and you've been asked to add a feature: draft a reply based on the contents of a ticket, following your team's tone and guidelines. You want to wire this up to a button in the UI.

---

### Lesson 2: Your first API call

Take the API key and store it in a .env.local file so it stays out of your version control. Hardcoding keys in source files is how they end up leaked on GitHub — keep them in environment files instead. Next, install the SDK: npm install @anthropic-ai/sdk

**The anatomy of a request**

Every API call goes through the messages.create function. You specify three things:

**A max tokens limit — a cap on how long the response can be**

A list of messages — objects with either user or assistant roles, structured similarly to how you'd have a conversation with Claude elsewhere Here's what that looks like in its most basic form: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const msg = await client.messages.create({ model: "claude-opus-4-7", max_tokens: 1024, messages: [{ role: "user", content: "Hello, Claude", }], });

**A real example: reviewing buggy code**

Let's give Claude something a little more interesting than "hello." We'll point it at some buggy code and ask for a review. Here's the whole thing — one file, about 20 lines of code: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const buggyCode = ` function add(a, b) { return a - b; } `; const response = await client.messages.create({ model: "claude-opus-4-8", max_tokens: 1024, system: "You are a terse senior code reviewer.

Give feedback in one paragraph.", messages: [ { role: "user", content: `Review this code:\n${buggyCode}` }, ], }); for (const block of response.content) { if (block.type === "text") { console.log(block.text); } } Two things to notice here: The system prompt is where you shape the persona. I want a terse senior reviewer, not a chatty one — so I just say that. The message.content in the response is an array of blocks, not a string.

For a basic text reply there's usually just one block of type text, but Claude can return multiple blocks — text, tool calls, thinking — so we always loop and check the type. Run it, and Claude spots that add is subtracting and tells you in one paragraph. That's it. That's the whole API call.

**From script to product**

In a real product, this same messages.create shape is the engin…

**Key Takeaways:**

- Take the API key and store it in a .env.local file so it stays out of your version control. Hardcoding keys in source files is how they end up leaked on GitHub — keep them in environment files instead.
- **The anatomy of a request** — Every API call goes through the messages.
- **A max tokens limit — a cap on how long the response can be** — A list of messages — objects with either user or assistant roles, structured similarly to how you'd have a conversation with Claude elsewhere Here's what that looks like in its most basic form: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const msg = await client.
- **A real example: reviewing buggy code** — Let's give Claude something a little more interesting than "hello." We'll point it at some buggy code and ask for a review. Here's the whole thing — one file, about 20 lines of code: import Anthropic from "@anthropic-ai/sdk"; const client = new Anthropic(); const buggyCode = ` function add(a, b) { return a - b; } `; const response = await client.

---

### Lesson 3: Choosing the right model

You're shipping an app with Claude. Which model do you pick? If you default to the smartest one, your API bill will surprise you. Pick the cheapest one, and the output might not hold up. Each model has different trade-offs, and picking the right one affects both quality and cost.

**The model tiers**

Anthropic currently offers four model tiers, and you choose between them with the model parameter in your API call. Note that at the time of this course, Claude Fable was not generally available, and are not reflected in the video above. Learn more about Claude Fable and Claude Mythos here. Claude Fable is our most capable model yet — a new tier that sits above Opus, built for your toughest challenges. It comes at a significantly higher cost than Opus, so reserve it for work where that extra capability is worth paying for.

Claude Opus is the most capable of the three core model families, but also the slowest and highest cost of the three. Use it for deep reasoning, complex analysis, multi-step coding, and nuanced writing. Claude Haiku is the fastest and lowest cost, optimized for speed and cost efficiency rather than maximum intelligence. Use it for high-volume, low-complexity work like classification, extraction, and routing. Claude Sonnet sits in the sweet spot: a balanced combination of intelligence, speed, and cost that works well for most production work.

**Start with a simple evaluation**

Before you write production code, set up a simple evaluation: a set of example inputs that you run through each model and score against what good output means for your use case. You don't need anything fancy — 20 or 30 representative examples from your actual workload is enough to start. Then work your way up the tiers: Run your examples through Haiku first. If the quality holds, you're done — and you just saved a lot of money. If it doesn't, step up to Sonnet. Only reach for Opus when the task needs it.

**Comparing the tiers side by side**

Let's see the difference between the tiers, not just talk about it. We'll send the same prompt through all three models and watch the l…

**Key Takeaways:**

- You're shipping an app with Claude. Which model do you pick?
- **The model tiers** — Anthropic currently offers four model tiers, and you choose between them with the model parameter in your API call. Note that at the time of this course, Claude Fable was not generally available, and are not reflected in the video above.
- **Start with a simple evaluation** — Before you write production code, set up a simple evaluation: a set of example inputs that you run through each model and score against what good output means for your use case. You don't need anything fancy — 20 or 30 representative examples from your actual workload is enough to start.
- **Comparing the tiers side by side** — Let's see the difference between the tiers, not just talk about it. We'll send the same prompt through all three models and watch the latency and token counts: models = ["claude-haiku-4-5", "claude-sonnet-4-6", "claude-opus-4-7"] for model in models: response = client.

---

### Lesson 4: The agent loop explained

You've made API calls, but a single call only returns one response. If you want to automate a workflow, Claude needs to act, look at the result, decide what's next, and keep going. That pattern is what people mean when they talk about agentic workflows.

**What an agent actually is**

An agent is an autonomous version of Claude, running both sides of the messaging loop without a human in the middle. An agent receives a task, picks a tool, and executes code in a loop until Claude decides the task is done. The easiest way to implement an agent loop looks like this: Send a message to Claude with tools available. Claude responds with either a final answer or a request to use a tool you defined. Your code executes that tool. You send the result back to Claude. Repeat until the stop reason is end_turn.

Think of it as a conversation where the turns alternate: the user kicks things off, the agent calls a tool, the tool returns a result, and the agent keeps going until it has an answer.

**A minimal working example**

To see this loop run end to end without dragging in a database or a UI, we'll wire up a fake tool called get_weather and ask Claude what to wear in Austin today. Claude has no way to know the weather on its own, so it has to call the tool, read the result, and then give you an answer.

Here's the whole script: import anthropic client = anthropic.Anthropic() # The tools array tells Claude what's available: # a name, a description, and a JSON schema for the inputs. tools = [ { "name": "get_weather", "description": "Get the current weather for a city.", "input_schema": { "type": "object", "properties": { "city": { "type": "string", "description": "The city to get weather for", } }, "required": ["city"], }, } ] # run_tool is just a hardcoded lookup. # In a real app, this would hit your database, an API, whatever. def run_tool(name, tool_input): if name == "get_weather": return f"Weather in {tool_input['city']}: 95F, sunny" raise ValueError(f"Unknown tool: {name}") messages = [ {"role": "user", "content": "What should I wear in Aus…

**Key Takeaways:**

- You've made API calls, but a single call only returns one response. If you want to automate a workflow, Claude needs to act, look at the result, decide what's next, and keep going.
- **What an agent actually is** — An agent is an autonomous version of Claude, running both sides of the messaging loop without a human in the middle. An agent receives a task, picks a tool, and executes code in a loop until Claude decides the task is done.
- **A minimal working example** — To see this loop run end to end without dragging in a database or a UI, we'll wire up a fake tool called get_weather and ask Claude what to wear in Austin today. Claude has no way to know the weather on its own, so it has to call the tool, read the result, and then give you an answer.
- **Running it** — When you run the script, you'll see two turns: Turn one: the stop reason is tool_use. Claude requests get_weather for Austin, and your code returns the temperature and conditions.

---

### Lesson 5: What is tool use?

Your existing workflows rely on a lot of different technologies — project management software, databases, files. Claude can't just check these things itself. Instead, it relies on tools, which give Claude access to external data and actions.

**What a tool is**

Simply put, a tool is a function you define and expose to Claude. You describe what it does and what inputs it takes, and Claude decides when to call it. Here's the key thing to internalize: Claude doesn't execute the tool — your code does. The flow looks like this: Claude requests a tool call. Your code executes the function. The result goes back to Claude, and it keeps going.

**How tools are defined**

Tools are JSON schemas with three parts: a name, a description, and an input schema. You pass them to Claude in the request body as a tools array. The description is what Claude reads to decide whether to call the tool. If you write a vague description, you get bad tool use. This is the number one reason agents misfire or don't grab the tools that are available to them. Be specific. Here's what a tool definition looks like: { "name": "lookup_building_code", "description": "Look up a specific building code section by its identifier.

Returns the full text of that code section.", "input_schema": { "type": "object", "properties": { "section": { "type": "string", "description": "The building code section to look up" } }, "required": ["section"] } } So what happens when we use this? Say we send an agent a compliance report. On the first turn, Claude comes back with stop_reason: "tool_use" — that's our signal.

Here's what that response looks like: Our loop calls lookup_building_code with the parameter Claude requested, then feeds the result back as a tool result — a user message containing a tool_result block tied to the tool call's id: And Claude keeps going. At that point, we can keep calling tools and returning results to Claude until it has what it needs.

**Multiple tools: letting Claude pick**

One tool is useful, but the interesting part is giving Claude multiple tools and watching it pick which one…

**Key Takeaways:**

- Your existing workflows rely on a lot of different technologies — project management software, databases, files. Claude can't just check these things itself.
- **What a tool is** — Simply put, a tool is a function you define and expose to Claude. You describe what it does and what inputs it takes, and Claude decides when to call it.
- **How tools are defined** — Tools are JSON schemas with three parts: a name, a description, and an input schema. You pass them to Claude in the request body as a tools array.
- **Multiple tools: letting Claude pick** — One tool is useful, but the interesting part is giving Claude multiple tools and watching it pick which one to use, in what order. Picture this scenario: you're packing for a three-day trip to Denver, and you want both today's weather and the forecast for the next few days.

---

### Lesson 6: What is thinking?

Some tasks need more than a quick answer. Claude can work through a problem before responding — a feature called extended thinking. In this lesson, we'll look at what thinking is, how to turn it on, and when it actually helps. Here's the failure mode we're trying to avoid. Ask a model a multi-step question and have it answer immediately, and it can confidently get it wrong: What is extended thinking? Extended thinking lets Claude reason step by step before producing a final response.

When it's enabled, Claude generates internal reasoning tokens — often called a chain of thought — and then delivers the answer. The reasoning isn't hidden: you can see it in the response alongside the final text.

**Adaptive thinking on Opus 4.7**

With Opus 4.7, thinking is adaptive. You don't pick a token budget. You just turn it on, and Claude decides dynamically when to think and how much. To control how much Claude thinks, use the effort parameter. One gotcha: it goes inside output_config, not next to the thinking block. The levels are: low medium high (the default) xhigh (extra high) max

**Anything that involves trade-offs or comparing options**

Skip it for simple classification, extraction, or boilerplate. For those tasks it just adds latency and cost without actually improving the results.

**Thinking in action**

Let's see it work. Here's an agent loop with one weather tool, and we'll ask Claude to plan a road trip out of San Francisco — two stops, weighing weather and drive time.

That's a real trade-off, the kind of question where thinking earns its keep. import anthropic client = anthropic.Anthropic() weather_tool = { "name": "get_weather", "description": "Get the current weather for a city.", "input_schema": { "type": "object", "properties": { "city": {"type": "string", "description": "City name"} }, "required": ["city"], }, } response = client.messages.create( model="claude-opus-4-7", max_tokens=16000, thinking={"type": "adaptive"}, output_config={"effort": "high"},  # low | medium | high | xhigh | max tools=[weather_tool], messages=[ { "role": "user", "content": "Plan a road trip out of San Fr…

**Key Takeaways:**

- Some tasks need more than a quick answer. Claude can work through a problem before responding — a feature called extended thinking.
- **Adaptive thinking on Opus 4.7** — With Opus 4.7, thinking is adaptive. You don't pick a token budget.
- **Anything that involves trade-offs or comparing options** — Skip it for simple classification, extraction, or boilerplate. For those tasks it just adds latency and cost without actually improving the results.
- **Thinking in action** — Let's see it work. Here's an agent loop with one weather tool, and we'll ask Claude to plan a road trip out of San Francisco — two stops, weighing weather and drive time.

---

### Lesson 7: Built-in tools

You can build your own custom tools, but some capabilities are common enough that Anthropic ships them pre-built. You don't write the code. You don't host the sandbox. You just declare the tool, and Anthropic runs it.

**Server tools: declared by you, run by Anthropic**

Anthropic provides server tools that run on their infrastructure. You don't execute these — Anthropic does. That means you don't need an agent loop for these calls. Claude calls the tools on its own, and the result comes back inside the same response. The main ones are:

**Two server tools in one file**

Let's check out some of the big ones in one file: two messages.create calls, one with web search and one with code execution. import anthropic client = anthropic.Anthropic() # Call 1: web search — Anthropic runs the search server-side search_response = client.messages.create( model="claude-opus-4-8", max_tokens=1024, tools=[{"type": "web_search_20260209", "name": "web_search"}], messages=[ {"role": "user", "content": "What is Anthropic's latest model release?

Answer in one sentence."} ], ) for block in search_response.content: if block.type == "server_tool_use": print(f"Tool call: {block.name} — {block.input}") elif block.type == "text": print(block.text) # Call 2: code execution — Claude writes and runs Python in a sandbox code_response = client.messages.create( model="claude-opus-4-8", max_tokens=1024, tools=[{"type": "code_execution_20260120", "name": "code_execution"}], messages=[ {"role": "user", "content": "Calculate the mean and standard deviation of [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]"} ], ) for block in code_response.content: if block.type == "server_tool_use": print(f"Tool call: {block.name} — {block.input}") elif block.type == "bash_code_execution_tool_result": print(f"stdout: {block.content.stdout}") elif block.type == "text": print(block.text) Two things to notice: There's no agent loop here.

We don't switch on stop_reason. We don't push tool results back. Anthropic runs the tool server-side, and the response already contains the result. The response has new block types. A server…

**Key Takeaways:**

- You can build your own custom tools, but some capabilities are common enough that Anthropic ships them pre-built. You don't write the code.
- **Server tools: declared by you, run by Anthropic** — Anthropic provides server tools that run on their infrastructure. You don't execute these — Anthropic does.
- **Two server tools in one file** — Let's check out some of the big ones in one file: two messages.create calls, one with web search and one with code execution. import anthropic client = anthropic.Anthropic() # Call 1: web search — Anthropic runs the search server-side search_response = client.messages.create( model="claude-opus-4-8", max_tokens=1024, tools=[{"type": "web_search_20260209", "name": "web_search"}], messages=[ {"role": "user", "content": "What is Anthropic's latest model release? Answer in one sentence.
- **Running it** — For web search, you'll see Claude's tool call printed, then a one-sentence answer about the latest model release with the search citations folded in. For code execution, you'll see the actual Python Claude wrote, the stdout from the sandbox running it, and a final text answer.

---

### Lesson 8: Skills

Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. At the core of every Skill is a SKILL.md file — a packaged set of instructions you upload once and then attach to any messages.create call. You're teaching Claude how you do something: your status report format, your review checklist, your release notes. Claude reads the Skill, follows the procedure, and produces output in your shape.

**Skills vs. tools**

It's worth being clear on the difference, because the two solve different problems: Tools connect Claude to data and actions. "Look up this code section," "send this email" — Claude calls the tool, and something else runs. Skills teach Claude a procedure. "Generate the daily status report following this template" — it's a playbook Claude reads and follows, which sometimes means running bundled scripts itself. A simple way to remember it: tools are about what Claude can do, while Skills are about how you want it done. One more thing worth knowing: Skills don't load fully into context on startup.

Only the name and description load at first. When your agent decides a Skill is relevant, it then loads the full Skill into context. That keeps your context lean even when many Skills are available.

**Uploading a Skill**

Skills are uploaded once to your workspace, then referenced by ID. You can upload directly on the Claude Platform, or do it programmatically: skill = client.beta.skills.create( display_title="Status Report Generator", files=files_from_dir("status-report-skill"),  # folder containing SKILL.md ) print(skill.id)  # reference this ID in future requests For this example, I want a status report generator. All the rules for what makes a good status report — sections, tone, how to summarize, how to handle blockers — live in a Skill packaged ahead of time.

The activity log itself is just a string passed in at request time.

**Attaching a Skill to a request**

Skills attach to a request through the container configuration — a skills array inside the container, where e…

**Key Takeaways:**

- Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks. At the core of every Skill is a SKILL.
- **Skills vs. tools** — It's worth being clear on the difference, because the two solve different problems: Tools connect Claude to data and actions. "Look up this code section," "send this email" — Claude calls the tool, and something else runs. Skills teach Claude a procedure.
- **Uploading a Skill** — Skills are uploaded once to your workspace, then referenced by ID. You can upload directly on the Claude Platform, or do it programmatically: skill = client.
- **Attaching a Skill to a request** — Skills attach to a request through the container configuration — a skills array inside the container, where each entry names a skill_id and version. Here's the full call for the status report generator: response = client.

---

### Lesson 9: MCP

We have tools, skills, and connectors. So why does MCP exist? At first glance it looks like a second API stacked on top of the API. Fair question — and the answer comes down to who maintains the integration code.

**The maintenance problem**

Say your agent needs to pull tasks from Asana, check a Google Calendar, and search Slack — all in one go. With custom tools, you have to write three integrations. That part is doable. The painful part comes after: you also have to maintain those integrations every time one of those services changes its API, which happens often. Congratulations, you're now maintaining a pile of third-party API wrappers. MCP shifts that maintenance to the service provider. Asana publishes an MCP server. Slack publishes one. Google publishes one.

Each server exposes its own tools — with descriptions, schemas, and authentication — through a standard protocol. When their API changes, they update their server. You change nothing.

**Tools vs. skills vs. MCP**

These three features do different jobs: Tools connect Claude to your internal systems — your database, your project tracker, your proprietary APIs. You own the code, so you also own the maintenance. Skills teach Claude a procedure — your report template, your review checklist. Skills are instructions, not necessarily integrations. MCP connects Claude to third-party services, where the service provider maintains the integration. You don't write the Asana wrapper — Asana did. The short version: tools are for your stuff, skills are for your processes, and MCP is for everyone else's stuff.

**Connecting to an MCP server**

The cleanest way to get a feel for MCP is to point Claude at any MCP server and let it discover what's there. For this example, we'll use the Linear MCP server, with the connection details and auth token stored in a .env file. Two pieces work together in the request. The mcp_servers key declares the connection — a type, a URL, a name to refer to it by, and optionally an auth token. Then a tool with the type mcp_toolset configures which tools Claude can use from that server. T…

**Key Takeaways:**

- We have tools, skills, and connectors. So why does MCP exist?
- **The maintenance problem** — Say your agent needs to pull tasks from Asana, check a Google Calendar, and search Slack — all in one go. With custom tools, you have to write three integrations.
- **Tools vs. skills vs. MCP** — These three features do different jobs: Tools connect Claude to your internal systems — your database, your project tracker, your proprietary APIs. You own the code, so you also own the maintenance.
- **Connecting to an MCP server** — The cleanest way to get a feel for MCP is to point Claude at any MCP server and let it discover what's there. For this example, we'll use the Linear MCP server, with the connection details and auth token stored in a .

---

### Lesson 10: Context management

Every request you send Claude has a context window. A million tokens sounds like a lot, but it runs out faster than you think once you're shipping a real agent. That's where context management comes in: it's how you stay inside the window without losing what matters.

**Thinking blocks**

It's the input to every single API call. You pay for it on the way in, and you pay for it on the way out. And once the window is full, the request fails. So the goal isn't to fit everything in. The goal is to fit the right things in. Anthropic publishes four patterns for managing context in long-running agents. Three are first-class API features, and one is a design pattern.

**Pattern 1: Just-in-time context**

Don't load everything upfront. Load what the agent needs now, and let it pull more in via tools when it asks. Think of a compliance review agent. It doesn't get the entire building code book stuffed into its system prompt — it calls a lookup_building_code tool when it needs a specific section. This is the design pattern of the four: nothing special in the API, just a deliberate choice about what you load and when.

**Pattern 2: Server-side compaction**

When a conversation runs long, Anthropic's server-side compaction summarizes old turns into a single block. You opt in by adding a context_management key to your request, holding an edit with a type: response = client.messages.create( model="claude-sonnet-4-5", max_tokens=1024, context_management={ "edits": [ {"type": "compact"} ] }, messages=messages, ) The API auto-summarizes when the input crosses the trigger threshold. You don't have to track conversation length yourself.

**Pattern 3: Prompt caching**

Prompt caching lets you mark the stable parts of a request — the system prompt, the tool definitions, a long document — and reuse them across calls at a fraction of the cost. The math matters more than it looks. If your system prompt is 4,000 tokens and you call it 100 times an hour, caching is the difference between a usable bill and a phone call from finance.

**Key Takeaways:**

- Every request you send Claude has a context window. A million tokens sounds like a lot, but it runs out faster than you think once you're shipping a real agent.
- **Thinking blocks** — It's the input to every single API call. You pay for it on the way in, and you pay for it on the way out.
- **Pattern 1: Just-in-time context** — Don't load everything upfront. Load what the agent needs now, and let it pull more in via tools when it asks.
- **Pattern 2: Server-side compaction** — When a conversation runs long, Anthropic's server-side compaction summarizes old turns into a single block. You opt in by adding a context_management key to your request, holding an edit with a type: response = client.

---

### Lesson 11: What are managed agents?

Under the hood, this is an agent loop: Claude reasons, calls a tool, reads the result, and repeats until the job is done. If you've built agents before, you've probably written this kind of loop yourself. Managed agents takes that same loop and hosts it on Anthropic's infrastructure, so you don't have to run it. You'll find Managed Agents in its own section of the Claude Console. The best way to understand what this unlocks is to walk through a few examples.

**Example 1: A Kanban board that does the work**

Picture a Kanban board sitting on top of managed agents. You drag a ticket into the "in progress" column, and that fires off a session automatically. Say the ticket reads "optimize website performance." Here's what happens: Your back end creates a session. The session points to an environment you configured with Lighthouse and Puppeteer pre-installed. Your GitHub repo gets mounted into the container. Now Claude has the codebase, the tools, and a rubric that defines what done looks like:

**All images lazy loaded**

Claude runs the audit, then starts compressing images, inlining CSS, and deferring scripts. Every tool call streams back to the board in real time through the event stream, so you can watch the work as it happens. Then the rubric kicks in. A separate grader, running in its own context window, evaluates the output against your criteria. Claude reads that feedback, goes back in, fixes what it missed, and resubmits. In the demo, that loop takes the Lighthouse score up to 96. One more thing: you can drag a second ticket over while the first is still running.

Two sessions, two containers, two separate tasks running in parallel.

**Example 2: A recurring research agent with memory**

Here's a different shape of agent: one whose job is to track prices and plan changes across every SaaS tool your company pays for, with a report ready before stand-up. On each run, the agent: Searches the web for current pricing pages, checks for plan tier changes, and flags new features that might affect your contracts

**Uses an Excel spreadsheet skill and writes an executive summary**

Posts a link to Slack and creates a review task in Asana, both through MCP servers The agent als…

**Key Takeaways:**

- Under the hood, this is an agent loop: Claude reasons, calls a tool, reads the result, and repeats until the job is done. If you've built agents before, you've probably written this kind of loop yourself.
- **Example 1: A Kanban board that does the work** — Picture a Kanban board sitting on top of managed agents. You drag a ticket into the "in progress" column, and that fires off a session automatically.
- **All images lazy loaded** — Claude runs the audit, then starts compressing images, inlining CSS, and deferring scripts. Every tool call streams back to the board in real time through the event stream, so you can watch the work as it happens.
- **Example 2: A recurring research agent with memory** — Here's a different shape of agent: one whose job is to track prices and plan changes across every SaaS tool your company pays for, with a report ready before stand-up.

---

### Lesson 12: Building your first managed agent

If you've built an agent loop by hand, you know the drill: while loops, stop reason switches, tool executions. That works, and for a lot of features it's actually the right shape. But sometimes that loop is going to run for a very long time — minutes, maybe even hours — across many tools, with state to keep, files to write, and work to resume after a network hiccup. At that point, you don't want to run the loop on your server. You want to delegate it. That's what managed agents are. What is a managed agent?

A managed agent is an agent loop that runs on Anthropic's infrastructure instead of yours. You describe the agent once, you give it an environment to work in, and you start a session. Anthropic runs the loop, and you just stream the events back out as it works. Managed agents are enabled by default for every API account — no special access needed.

**The four primitives**

There are four primitives, and they come in order: Agent — the persona: model, system prompt, and toolset. This is reusable across many runs. Environment — where the agent runs: cloud or local, networking config, and so on. Session — a single run of an agent inside a certain environment. The session is the unit of work. Events — the messages flowing in and out: the agent's actions, the tool calls, the results, the replies.

Here's how the pieces fit together: your app talks to a session, the session drives work inside the environment, and everything that happens flows back out through the event stream: Notice the shift here: you're not running a while loop. You're sending events and reading events.

**The smallest possible managed agent**

Let's build the smallest managed agent that does something useful: create a file in the temp drive, count its lines, and report back. For tools, we'll use the agent toolset — Anthropic's bundled file, bash, and web tools. They work fine for this task, so we don't have to define any tools ourselves.

**Step 1: Create the agent**

First, we create the agent. Note the agent toolset defined right in the tools array — that's the bundled toolset: import anthropic cl…

**Key Takeaways:**

- If you've built an agent loop by hand, you know the drill: while loops, stop reason switches, tool executions. That works, and for a lot of features it's actually the right shape.
- **The four primitives** — There are four primitives, and they come in order: Agent — the persona: model, system prompt, and toolset. This is reusable across many runs.
- **The smallest possible managed agent** — Let's build the smallest managed agent that does something useful: create a file in the temp drive, count its lines, and report back. For tools, we'll use the agent toolset — Anthropic's bundled file, bash, and web tools.
- **Step 1: Create the agent** — First, we create the agent. Note the agent toolset defined right in the tools array — that's the bundled toolset: import anthropic client = anthropic.

---

### Lesson 13: Building with Claude Code

Writing code that calls the Claude API by hand works fine, but there's an even faster path: have Claude write it for you. In this lesson, we'll use Claude Code to fill in an API integration from a stubbed-out file — using the same primitives you've learned throughout this course.

**Starting from a stub**

The project is simple: a TypeScript file that gets weather. It contains two stubs: getWeather — accepts a city and returns the temperature and conditions. run — a function that should use the tool runner and the Claude TypeScript SDK. The tool runner is the piece that handles tool calling and the agent loop for you, so you don't have to wire that up manually.

**The Claude API skill**

Claude Code comes with a built-in skill called Claude API. You can invoke it directly with /claude-api, or Claude Code will invoke it automatically when it detects that you're using the TypeScript SDK. If you don't see the skill, you can add it from the marketplace: /plugin marketplace add AnthropicsSkills Note the s at the end of Anthropics — it's easy to miss.

**One prompt, working code**

Open the project folder in your terminal and launch Claude Code. From there, it takes a single prompt. A good prompt does three things: It names the file you want changed. It names the pattern you want used. It names the end state you expect. Claude Code then fills in getWeather and run against the types, appends a call at the bottom of the file, executes the script, and reports the output. If something errors out, it reads the error message and patches the code in place.

**What Claude Code produced**

In this run, Claude Code created a Zod tool that parsed the input and returned the output based on the city type. It also created the tool runner and the run function we asked for, and printed the final results of the agent loop.

**Key Takeaways:**

- Writing code that calls the Claude API by hand works fine, but there's an even faster path: have Claude write it for you. In this lesson, we'll use Claude Code to fill in an API integration from a stubbed-out file — using the same primitives you've learned throughout this course.
- **Starting from a stub** — The project is simple: a TypeScript file that gets weather. It contains two stubs: getWeather — accepts a city and returns the temperature and conditions.
- **The Claude API skill** — Claude Code comes with a built-in skill called Claude API. You can invoke it directly with /claude-api, or Claude Code will invoke it automatically when it detects that you're using the TypeScript SDK.
- **One prompt, working code** — Open the project folder in your terminal and launch Claude Code. From there, it takes a single prompt.

---
