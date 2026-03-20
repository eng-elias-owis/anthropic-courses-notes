# Claude 101 — Course Notes

> Source: https://anthropic.skilljar.com/claude-101
> Extracted: March 2026 (real content from actual lessons)

## Course Overview

Claude 101 is Anthropic's foundational course covering what Claude is, how to interact with it effectively, how to organize work with Projects/Artifacts/Skills, how to extend Claude via connectors and integrations, and how to use advanced features like Research mode and Enterprise Search. The course has 12 content lessons plus a final quiz, estimated at ~2.5–3 hours total.

---

## Lesson Notes

### Lesson 1: What is Claude?

**Key Concepts:**
- Claude is guided by **Constitutional AI** — trained to be helpful, harmless, and honest; avoids toxic, discriminatory, or illegal outputs
- Claude is a **thinking partner**, not just a chatbot — handles summarization, research, creative writing, Q&A, coding, and more
- Claude is **steerable**: can take direction on personality, tone, and behavior with less effort than other AI tools
- **Context window**: 200K+ tokens (~500 pages of text) — can ingest large documents in a single conversation
- **Extended thinking**: Opus 4.5 and Sonnet 4.5 offer two modes — near-instant responses and step-by-step deep reasoning
- **Learning mode**: Guides your reasoning process rather than just giving answers; develops critical thinking
- Claude Opus 4.5 is described as the best coding model

**Access methods:**
- Claude.ai (web, desktop, mobile) — primary interface
- Claude Code — agentic coding tool for developers
- Claude in Slack — search workspace channels/DMs via @Claude
- Claude for Excel — sidebar integration for model analysis, formula help, workbook editing

**Practical Tips:**
- Use the Use Case Gallery at claude.com to find inspiration specific to your role
- Conversations, projects, memory, and preferences sync across devices when signed in

---

### Lesson 2: Your First Conversation with Claude

**Key Concepts:**
- Talk to Claude **like a coworker** — naturally, concisely, conversationally
- **The 3-part prompt framework**:
  1. **Setting the stage** — your role, objectives, context
  2. **Defining the task** — what action you want (write, analyze, build, etc.)
  3. **Specifying rules** — style, tone, format, examples
- Uploading files gives Claude context as a shortcut — supported types: PDF, DOCX, CSV, TXT, PNG, JPEG
- The real power comes from **continued, frequent interaction**, not one-off prompts
- Conversations are **iterative** — first response is a starting point, not a final answer

**The 4D Framework for AI Fluency** (referenced throughout the course):
- **Delegation** — deciding what humans vs. AI should do
- **Description** — communicating clearly with AI
- **Discernment** — critically evaluating AI outputs
- **Diligence** — using AI responsibly and ethically

**Practical Tips:**
- Include who your audience is, your role, constraints, and context in prompts
- If Claude goes off track: ask follow-ups, give feedback, or restart with a cleaner prompt
- Add global preferences in Settings to apply to every response automatically
- Name uploaded files descriptively — Claude uses filenames to retrieve information

---

### Lesson 3: Getting Better Results

**Key Concepts:**
- First prompts rarely produce perfect results — adopt an **iteration mindset**
- AI Fluency = ability to collaborate effectively with AI tools, not just click buttons

**Common challenges & fixes:**

| Challenge | Fix |
|---|---|
| Response too generic | Add specific context: audience, role, constraints |
| Too long / too short | Be explicit: "Keep under 100 words" or "Length isn't a concern" |
| Wrong format | Show, don't just tell — provide an example or describe structure explicitly |
| Confident-sounding but wrong | Verify facts independently; ask Claude to cite sources; enable web search |
| Wrong tone | Describe tone in plain language; provide a writing sample example |

**Evals (Evaluations):**
- Collect 5–10 real examples of a recurring task
- Write prompts that would generate similar outputs
- Compare Claude's results to your originals
- Refine prompts based on what's missing or off

**Practical Tips:**
- "Cut the first two paragraphs and make the conclusion more action-oriented" beats "make it shorter"
- Know when to start fresh — sometimes a new chat is faster than redirecting
- Run lightweight evals before relying on Claude for recurring high-stakes tasks

---

### Lesson 4: Claude Desktop App — Chat, Cowork, Code

**Key Concepts:**
- The desktop app has **three modes**: Chat, Cowork, and Code
- Cowork and Code both run on Claude Code under the hood — capable of spawning sub-agents and sustaining long tasks

**Chat mode:**
- Same as claude.ai, plus desktop-native features
- **Quick entry**: Double-tap Option key (Mac) to overlay Claude over any app
- **Screenshots**: Drag to capture any window without leaving your current task
- **Dictation**: Talk through problems instead of typing
- **Desktop connectors**: Connect local tools via Settings

**Cowork mode** (research preview, Pro/Max/Team/Enterprise):
- Built for complex sustained work: research briefs, financial analysis, contract review, slide decks
- Asks clarifying questions upfront; builds a reviewable plan in the sidebar
- **Folder access**: Give Claude a folder; it reads, processes, and saves back to same location
- **Scheduled tasks**: Set recurring work (daily briefings, weekly roundups) — runs automatically when app is open
- **Browser use**: Claude can navigate websites via Chrome connector
- **Plugins**: Add specialized capabilities (live financial data, internal knowledge bases)
- Runs in a **protected/contained environment**

**Code mode** (Pro/Max/Team/Enterprise):
- Full development environment with visual diffs, built-in terminal, git integration
- **Local**: Works directly with your files and local tools
- **Remote**: Connect a GitHub repo; sessions persist even when app is closed
- Three interaction modes: **Ask** (approve every change), **Code** (auto-applies file changes), **Plan** (review full approach before execution)

**Practical Tips:**
- Use Chat for quick questions and iterative drafting
- Use Cowork for research, document production, and recurring automated tasks
- Use Code for actual software development

---

### Lesson 5: Introduction to Projects

**Key Concepts:**
- Projects are **self-contained workspaces** with their own memory, chat histories, knowledge bases, and custom instructions
- Ideal for ongoing work — not one-off questions
- **RAG mode**: When knowledge base approaches context limits, Claude auto-enables Retrieval Augmented Generation — expands capacity up to 10x while maintaining quality
- For Team/Enterprise users: share projects with **three permission levels**: Can view, Can edit, Owner

**Creating a project (3 steps):**
1. **Set up**: Name, description, visibility (private vs. org-wide)
2. **Add instructions**: Context, process steps, tone/style preferences, specific requirements
3. **Build knowledge base**: Upload PDFs, DOCX, CSV, TXT, HTML, or link Google Drive

**Example project types:**
- Q4 product launch (specs + competitive analysis + messaging notes)
- Research support (user research data + customer feedback)
- Client account hub (brand guidelines + past deliverables + communication history)
- Job description generator (past JDs + team charters + headcount docs)

**Practical Tips:**
- Name files descriptively — "Q4-2024-Brand-Guidelines.pdf" beats "document1.pdf"
- Start focused with one use case; expand later
- Keep knowledge base current — outdated docs lead to outdated responses
- Reference documents by name in prompts: "Based on our Q3 report, what were the top customer concerns?"

---

### Lesson 6: Creating with Artifacts

**Key Concepts:**
- Artifacts are **standalone, interactive outputs** that appear in a dedicated window alongside chat
- Claude auto-creates an artifact when content is: self-contained (>15 lines), editable/reusable, complex, or worth referencing later

**Types of artifacts:**
- **Documents** (markdown, plain text) — reports, meeting notes, blog posts
- **Code snippets** — working code in any language
- **HTML pages** — complete web pages with HTML/CSS/JS
- **SVG images** — logos, icons, illustrations
- **Mermaid diagrams** — flowcharts, sequence diagrams, Gantt charts, org charts
- **React components** — interactive UIs with real functionality (calculators, dashboards, games)

**Sharing options:**
- Copy or download (any user)
- Share within organization (Team/Enterprise only)
- **Publish publicly**: anyone with link can view/interact; others can "remix" into their own Claude chat

**Practical Tips:**
- Be specific: "Build a monthly budget tracker where I can input expenses by category, see a pie chart breakdown, and get a warning when I'm over budget"
- Describe the end user — affects design choices
- Iterate one feature at a time
- If Claude doesn't auto-create an artifact, say: "Please create that as an artifact"

---

### Lesson 7: Working with Skills

**Key Concepts:**
- Skills are **folders of instructions, scripts, and resources** Claude loads dynamically for specialized tasks
- Powers file creation capabilities for Excel, Word, PowerPoint, and PDF
- **Two types**:
  - **Anthropic Skills**: Built-in, automatically invoked, available to all paid users
  - **Custom Skills**: You or your org creates for specialized workflows

**Enabling Skills:**
- Settings > Capabilities > ensure "Code execution and file creation" is on > toggle Skills
- Enterprise: Owner must enable in Admin settings first
- Team: Enabled by default at org level

**Skills vs. Projects:**
| | Projects | Skills |
|---|---|---|
| Purpose | Store knowledge Claude references | Define processes Claude executes |
| Best for | Long-term context, reference materials, team collaboration | Repeatable workflows, multi-step tasks |
| Example | Customer hub, research buddy | Brand guidelines, blog drafting, PDF creation |

**Creating a custom skill (via conversation):**
1. Tell Claude what you want to create
2. Answer Claude's interview questions about your workflow
3. Upload reference materials (templates, style guides, examples)
4. Download the generated ZIP file
5. Upload to Settings > Capabilities > Skills

**Practical Tips:**
- Skills and Projects complement each other — a skill can reference project knowledge
- Only install custom Skills from trusted sources
- Custom skills are private to your individual account

---

### Lesson 8: Connecting Your Tools

**Key Concepts:**
- Connectors transform Claude from assistant into **informed collaborator** with access to your actual data
- Powered by **Model Context Protocol (MCP)** — "USB-C for AI," a universal standard for connecting apps
- **Two types**:
  - **Web connectors**: Cloud services (Google Drive, Notion, Slack, Asana, Gmail, Stripe, Salesforce, etc.)
  - **Desktop extensions**: Local tools via Claude Desktop app (file system, browser automation, Figma, etc.)
- Claude only sees data **you have permission to access**

**Setting up a web connector:**
1. Navigate to claude.ai/directory or click "Search and tools" > "Add connectors"
2. Click Connect, authenticate, review and grant permissions
3. Test: "Can you access my [tool name]?"

**Example use cases by tool type:**
- **Project management** (Asana/Linear/Jira): "What are my highest priority tasks due this week?"
- **Communication** (Slack/Gmail): "Find the email thread where we discussed the vendor contract"
- **Documentation** (Notion/Drive): "Search our docs for our brand voice guidelines"
- **Business tools** (Stripe/Salesforce): "Show me revenue trends for the past quarter"

**Practical Tips:**
- Permissions are scoped and revocable at any time
- Connectors eliminate the need to copy-paste information between tools
- Combining data from multiple connected sources is where the real time savings emerge

---

### Lesson 9: Enterprise Search

**Key Concepts:**
- Adds a dedicated **"Ask {Your Org Name}"** option in sidebar
- Designed specifically for **information gathering** across your company's tools — not general conversation
- Searches SharePoint, Slack, Gmail, Google Drive simultaneously and synthesizes with citations
- Requires two-step setup: admin configures first, then users authenticate

**Admin setup:**
1. Click "Ask Your Org" in sidebar > "Set up for your org"
2. Connect Documents (Google Drive or SharePoint) and Chat (Slack or Teams) — both required; email optional
3. Customize project name; it appears as "Ask [Name]" for all users

**What it's great for:**
- Getting up to speed after absence ("What happened yesterday?")
- Policy/process questions ("How do I submit an expense report?")
- Onboarding new team members ("Who should I talk to about the billing system?")
- Cross-source research ("Summarize discussions about the Q4 product roadmap")

**Practical Tips:**
- More connectors = more comprehensive results
- Data shown is scoped to what you already have permission to see — safe by design
- Conversations remain private; connected data isn't indexed or stored separately

---

### Lesson 10: Research Mode for Deep Dives

**Key Concepts:**
- Research mode = Claude as a **systematic, multi-step investigator**
- Instead of a single search, Claude conducts **multiple iterative searches** that build on each other
- **Extended thinking auto-enables** with Research — for planning and thorough analysis
- Most reports complete in **5–15 minutes**; complex ones up to 45 minutes
- Every claim includes **citations** for easy verification

**How Research works (4 steps):**
1. Claude plans its approach (extended thinking)
2. Conducts multiple searches, building on findings
3. Synthesizes from web + connected integrations (Gmail, Drive, Calendar)
4. Delivers a comprehensive report with citations

**When to use Research vs. alternatives:**
- **Use Research**: Comprehensive reports, comparative analysis, multi-source investigations
- **Use web search instead**: Quick single-source facts
- **Use extended thinking instead**: Complex reasoning problems not requiring external data

**Enabling Research:**
- Click Research button (bottom left of chat) — turns blue when active
- Web search must be enabled first in Search and tools settings

**Practical Tips:**
- Be specific: "Analyze the electric vehicle battery market—identify key players, technology trends, and supply chain challenges" beats "Tell me about the EV market"
- Specify sections/structure you want — Claude organizes findings around your structure
- Include constraints (budget, geography, timeline) to focus research
- Ask Claude to help refine your Research prompt before enabling the feature
- With Google Workspace connected: "Review my calendar commitments for next week and research each company I'm meeting with"

---

### Lesson 11: Claude in Action — Use Cases by Role

**Key Concepts:**
- Claude applies across all professional roles
- The Use Case Gallery (claude.com) has step-by-step guides for each

**Use cases by role:**

**General professional:**
- Generate project status reports
- Analyze patterns in user feedback
- Package brand guidelines in a custom skill

**Sales:**
- Build a battle card library (competitive intelligence)
- Prepare for sales deals (prospect research + talking points)
- Create sales reports from pipeline data

**Marketing:**
- Analyze campaign performance
- Adapt/repurpose content across platforms

**Finance:**
- Build financial models
- Draft investment memos
- Understand and extend inherited spreadsheets

**HR:**
- Create new hire onboarding guides

**Legal:**
- Track discovery timelines and analyze patterns

**Research:**
- Plan literature reviews
- Verify statistics from raw data

---

### Lesson 12: What's Next?

**Key Concepts:**
- Course recap: Claude is a collaborator, not a replacement — best results when you bring expertise, context, and judgment
- Start simple: pick one recurring task this week and try it with Claude

**Additional resources:**
- AI Fluency courses (free) — deep dive into Delegation, Description, Discernment, Diligence
- Use Case Gallery — step-by-step workflow guides
- Anthropic Help Center — documentation and troubleshooting
- Claude Code in Action — course for development workflows
- Connector Directory — claude.ai/directory

---

## Top 20 Practical Tips

1. **Write prompts like you're talking to a coworker** — natural, concise, conversational; no need for formal commands
2. **Use the 3-part prompt framework**: Set the stage (context + role) → Define the task (action) → Specify rules (format/tone/examples)
3. **Treat first responses as drafts** — build the habit of iterating rather than expecting perfection on the first try
4. **Give specific feedback**: "Cut the first two paragraphs and make the conclusion more action-oriented" vs. "make it shorter"
5. **Be explicit about length**: Say "Keep under 100 words" or "length isn't a concern" rather than letting Claude guess
6. **Show format examples, don't just describe them** — attach a sample or describe structure explicitly: "Use bullet points with bold headers"
7. **For high-stakes facts, verify independently** — ask Claude to cite sources or enable web search to ground responses in current info
8. **Name project files descriptively** — "Q4-2024-Brand-Guidelines.pdf" helps Claude find the right doc; "document1.pdf" doesn't
9. **Create projects for recurring work** — stop re-uploading the same files; keep reference materials in a project knowledge base
10. **Write specific project instructions** — include context, process steps, tone preferences, and hard requirements; vague instructions = inconsistent results
11. **Use artifacts for reusable outputs** — request dashboards, flowcharts, templates, and tools as artifacts you can share and remix
12. **Iterate on artifacts one feature at a time** — this makes it easy to catch issues early
13. **Create custom Skills for repeatable workflows** — use a conversation with Claude to build it; no coding required
14. **Connect your tools via connectors** — eliminate copy-pasting; Claude can read Slack, Gmail, Drive, Notion, and more directly
15. **Use Research mode for multi-source investigations** — saves hours; include specific structure/sections you want in the output
16. **For Research prompts, add constraints** — budget, geography, timeline parameters focus the research on relevant options
17. **Use Cowork scheduled tasks for recurring morning work** — daily briefings, status updates, inbox triage on autopilot
18. **In the desktop app, use Quick Entry** (double-tap Option on Mac) to ask questions without leaving your current app
19. **Run lightweight evals before relying on Claude for recurring tasks** — compare Claude's output to 5–10 of your own examples
20. **Start simple and expand** — pick one recurring task this week, try it, iterate, then build from there

---

### Lesson 13: Other Ways to Work with Claude

**Key Concepts:**
- Claude is an intelligence — Claude.ai is just one interface; it's available across specialized tools
- **Claude Code** — agentic coding tool in your terminal/IDE; understands full codebase, runs tests, makes commits
- **Claude in Slack** — drafts responses, summarizes threads, preps for meetings, can spin up Claude Code sessions from bug reports
- **Claude for Excel** — sidebar in Microsoft Excel for analysis, formula help, pattern recognition, workbook editing
- **Claude for Chrome** — browser extension to summarize pages, draft emails in Gmail, assist in any web app

**When to use each:**

| Tool | Best For |
|------|----------|
| Claude Code | Building features, debugging, navigating unfamiliar codebases |
| Claude in Slack | Team collaboration, thread summaries, meeting prep |
| Claude for Excel | Data analysis, formula help, pattern recognition |
| Claude for Chrome | Web research, Gmail drafting, any browser-based work |

**Practical Tips:**
- Use Claude Code when you want to describe what you need in plain English and have it write + test the code
- Tag @Claude in Slack to hand off coding tasks directly from a bug report or feature discussion
- Claude for Excel can analyze patterns, model scenarios, and generate visualizations from your spreadsheet data
- Claude for Chrome works as an overlay — no need to switch tabs for quick summaries or drafts
