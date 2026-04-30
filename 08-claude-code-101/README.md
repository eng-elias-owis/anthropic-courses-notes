# Claude Code 101 — Course Notes

> Source: https://anthropic.skilljar.com/claude-code-101
> Extracted: April 2026 (real text content from 9 lessons)

---

## Course Overview

Claude Code is an **agentic coding tool** that reads your codebase, edits files, runs terminal commands, and integrates with external tools — all to help developers ship faster. Unlike Claude.ai (a chat interface), Claude Code operates as an AI Agent with direct access to your files, terminal, and project directories.

Available in: Terminal (macOS/Linux/WSL/Windows), VS Code, JetBrains IDEs, Claude Desktop, and the Web (claude.ai/code).

---

## Lesson Notes

### Lesson 1: What is Claude Code?

**Key Concepts:**
- Claude Code is an **AI Agent** — software that interacts with its environment in a loop to complete defined goals
- Has direct access to your files, terminal, and entire codebase (vs. copy-paste with Claude.ai)
- Can use tools, external services, and even other AI agents to reach goals
- Operates using a **context window** (working memory) — strategically loads relevant parts of the codebase rather than the entire thing

**Capabilities:**
- Read and understand your codebase (explain features, trace bugs)
- Edit files across an entire project (e.g., refactor a function and update all references)
- Run terminal commands (build scripts, tests, package installs) and use the output to decide next steps
- Search the web for documentation and API references

**Practical Tips:**
- Claude asks for permission before running commands or making changes by default — you are always in control
- Stay in the loop: Claude can make mistakes (misunderstand intent, introduce bugs, over-engineer). Catching errors early saves time
- Think of the context window as working memory — it can hold a lot, but not everything

---

### Lesson 2: How Claude Code Works

**Key Concepts:**
- **The Agentic Loop**: Prompt → Gather context → Take action → Verify results → Loop if needed → Finish
- You can interrupt, add context, or steer the model at any point in the loop
- **Context window**: Holds conversation, file contents, command outputs. When full, Claude Code auto-compacts (summarizes + removes unnecessary tool results)
- **Tools**: The backbone of agents. Allow Claude to execute code, read files, search the web — not just return text

**Permission Modes:**
- **Default**: Asks explicit permission before editing files or running shell commands
- **Auto-accept**: File edits happen automatically; commands still require approval
- **Plan mode**: Read-only tools only — compiles a plan before any work begins

**Practical Tips:**
- Configure permission modes in your settings file
- Be cautious with auto-accept for commands — mistakes can be harder to catch before they happen
- Plan mode is ideal for reviewing what Claude intends to do before it does it

---

### Lesson 3: Installing Claude Code

**Key Concepts:**
- Terminal installation: `curl` command on macOS/Linux/WSL; `Invoke-RestMethod` (PowerShell) or `curl` (CMD) on Windows
- Homebrew/winget installs don't support auto-updates
- VS Code: Install "Claude Code" extension by Anthropic (blue verification check); access via `Ctrl/Cmd + Shift + P` → "Claude Code Open in New Tab"
- JetBrains: Install from Marketplace; Claude pane appears alongside the editor
- Desktop: Toggle "Code" in Claude Desktop for background operation with folder-specific context
- Web: `claude.ai/code` — works with GitHub repositories only

**Practical Tips:**
- Terminal gets new features first — use it to stay on the cutting edge
- IDE integrations are nearly identical to terminal if you prefer Claude embedded in your editor
- Desktop is ideal for letting Claude run in the background while you handle other tasks
- Web is useful for remote access to GitHub projects
- After install, navigate to your project directory before running `claude` — it only has access to that directory and its subfolders

---

### Lesson 4: Your First Prompt

**Key Concepts:**
- `Shift + Tab` cycles between: Approval mode → Auto-accept mode → Plan mode
- **Plan Mode**: Uses read-only tools to analyze codebase, asks clarifying questions, returns a detailed plan before writing any code
- Plan mode excels at complex, multi-step implementations

**Practical Tips:**
- Be as descriptive as possible with your prompts
- Use Plan Mode for complex changes — it's the safest way to review Claude's intended approach
- After reviewing a plan, you can accept it and still require approval at each step
- Example prompt for Plan Mode: *"My app needs a dark mode implemented across the entire app. Can you create a toggle switch on the header that allows a user to toggle between light mode and dark mode? I need you to find a good contrast color that works based on my existing light theme."*

---

### Lesson 5: The Explore → Plan → Code → Commit Workflow

**Key Concepts:**
- The core workflow: **Explore → Plan → Code → Commit**
- Most people jump straight to "write code" and end up course-correcting more — this workflow prevents that
- **Explore**: Use Plan Mode or the explore subagent to gather context about the codebase
- **Plan**: Claude reads files, runs web searches, creates a plan of action. Review and revise before any code is written
- **Code**: Accept the plan and let Claude execute. Define success criteria upfront
- **Commit**: Run a subagent code reviewer for fresh, unbiased review, then generate a commit message

**Practical Tips:**
- Course-correct during the Plan phase — it's before any code is written
- **Define success criteria** explicitly so Claude knows what "correct" looks like
- Install the Claude in Chrome extension for web UIs so Claude Code can control a browser tab and test UI directly
- Give Claude a test suite to validate against continuously; have Claude write tests if needed
- If Claude keeps hitting the same issues, ask it to save the solution to `CLAUDE.md`
- Use a subagent code reviewer before pushing — it has no bias from the coding session
- Ask Claude to generate commit messages in your style

---

### Lesson 6: Context Management

**Key Concepts:**
- Context window = Claude's working memory. Every file read, command run, and message adds to it
- **Auto-compaction**: When the limit is reached, Claude summarizes and removes unnecessary tool results (can lose details)
- `/compact` — manually compact the session, preserving a summary
- `/clear` — wipe everything for a completely fresh start
- `/context` — shows context size, categories consuming the most space, and a visual breakdown

**When to Use Each:**
- `/compact`: Mid-feature when running out of space but need to continue
- `/clear`: Starting a new feature — avoids carrying bias from the previous session
- Put persistent knowledge in `CLAUDE.md` so Claude doesn't have to rediscover it

**Practical Tips:**
- **Be specific with prompts**: Vague prompts cost more context because Claude has to explore more on its own
- **Manage MCP servers**: They load all tools into context by default — turn off unrelated servers to save space
- **Use subagents**: They run in a separate context window; only return a summary to your main context. Ideal for tasks where you only need the result (e.g., "where are the authentication endpoints?")

---

### Lesson 7: Code Review

**Key Concepts:**
- Subagent code reviewers have a **fresh context window** — no bias from the session that wrote the code
- Restrict code-reviewer subagents to **read-only tools** — they should flag issues, not edit files
- Check subagent reviewer config into your repo so the whole team uses the same reviewer
- `/commit-push-pr` skill: handles commit + push + PR creation in one command
- If a Slack MCP server is configured with channels in `CLAUDE.md`, it auto-posts the PR link to the team channel
- `claude --from-pr <PR_NUMBER>`: resumes a session linked to a specific PR (for addressing review comments or fixing CI)

**Practical Tips:**
- Always run a subagent reviewer before pushing to catch issues with fresh eyes
- Use `/commit-push-pr` to eliminate manual commit/push/PR steps
- Use `--from-pr` to return to a PR's context later without starting from scratch

---

### Lesson 8: The CLAUDE.md File

**Key Concepts:**
- `CLAUDE.md` is a Markdown file in your project root that Claude reads automatically at the start of every session
- Solves the problem of Claude starting fresh each time and having to re-explore the codebase
- Contents are **appended to your prompt** automatically
- Supports a **hierarchy**: project-level (shared with team) and user-level (personal preferences, applies across all projects)

**Example CLAUDE.md:**
```markdown
# Project
This is a Next.js 15 app using the App Router, Tailwind, and Drizzle ORM.

# Commands
- Dev server: `pnpm dev`
- Run tests: `pnpm test`
- Lint: `pnpm lint`

# Code Style
- Use 2-space indentation
- Prefer named exports
- All API routes go in app/api/
- Use server actions instead of API routes where possible
```

**Practical Tips:**
- When you repeatedly correct Claude on the same thing, ask it to **save that rule to CLAUDE.md**
- Reference project docs with `@file-path` syntax: `@README.md`
- **Start without a CLAUDE.md** — identify where you keep correcting Claude, then add only what's necessary
- Run `/init` when ready to have Claude generate a CLAUDE.md for you
- Commit CLAUDE.md to version control so the whole team benefits

---

### Lesson 9: Subagents

**Key Concepts:**
- Subagents run in **isolated context windows**, in parallel with the main agent
- Main use case: delegate expensive, exploratory tasks (codebase exploration, web research) — subagent returns only a summary to the main context
- Defined in **Markdown files with YAML frontmatter**
- Create via `/agents` → "Create new agent" — Claude generates name, description, prompt, and invocation triggers

**Customization Options:**
- **Persistent memory**: Subagent retains memory across conversations (useful for recurring project work)
- **Preload skills**: List skills by name; note the entire skill is loaded into context (unlike skills in the main conversation)
- Scope, purpose, tool access, and color are all configurable

**Practical Tips:**
- Delegate heavy exploration work to subagents to keep your main context clean
- Restrict reviewer subagents to read-only tools
- Use subagents for tasks where you only need the answer, not the full reasoning trail

---

## Top 15 Practical Tips (Consolidated)

1. **Use the Explore → Plan → Code → Commit workflow** — don't jump straight to writing code. The Plan phase is your cheapest opportunity to course-correct.

2. **Enter Plan Mode with `Shift + Tab`** before complex tasks. Let Claude map out the implementation in read-only mode before touching any files.

3. **Define success criteria explicitly** in your prompt. Claude needs to know what "done" looks like to verify its own work.

4. **Be specific with prompts** — vague prompts cost more context because Claude compensates by exploring more on its own.

5. **Use `/compact` mid-session** when running low on context but need to continue the current feature. Use `/clear` when starting something new.

6. **Maintain a `CLAUDE.md` file** with your stack, commands, and code style conventions. It's loaded automatically and eliminates repeated corrections.

7. **Ask Claude to save corrections to `CLAUDE.md`** when you find yourself giving the same instruction more than once.

8. **Run a subagent code reviewer before every PR** — it has no bias from the session that wrote the code. Restrict it to read-only tools.

9. **Use `/commit-push-pr`** to handle the full commit → push → PR flow in one step.

10. **Use `claude --from-pr <NUMBER>`** to resume work on a PR without losing context — perfect for addressing review comments or CI failures.

11. **Use subagents for exploratory tasks** (e.g., "find all authentication endpoints") — they return a summary to your main context, keeping it clean.

12. **Manage MCP servers** — unrelated servers load all their tools into context by default. Disable unused servers to save context space.

13. **Give Claude a test suite** to validate against continuously. Have Claude write tests if you don't have them. Ensure tests are a reliable source of truth.

14. **Start a project without `CLAUDE.md`** — observe where you correct Claude most, then use `/init` to generate a focused, minimal file.

15. **Use `--from-pr` and session linking** to maintain continuity across separate coding sessions tied to the same pull request.
