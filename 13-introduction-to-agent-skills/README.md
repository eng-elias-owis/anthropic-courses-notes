# Introduction to Agent Skills — Course Notes

> **Source:** https://anthropic.skilljar.com/introduction-to-agent-skills
> **Extracted:** July 2026
> **Author:** Anthropic Academy
> **Estimated Time:** ~105 minutes total (15 + 20 + 20 + 15 + 20 + 15)
> **Lessons:** 6
> **Format:** Video lessons + written notes

---

## 📚 Course Overview

This course teaches you how to **build, configure, and share Skills in Claude Code** — reusable markdown instructions that Claude automatically applies to the right tasks at the right time. Skills are folders of instructions that Claude Code can discover and use to handle tasks more accurately, eliminating the need to repeat the same instructions every time you ask Claude to review a PR, write a commit message, follow a brand guideline, or run a team-specific workflow.

The course is designed for developers using Claude Code who find themselves **re-explaining the same conventions, standards, or processes** to Claude over and over. It walks you from "what is a skill?" through creating your first skill, configuring advanced metadata fields, choosing between skills and other Claude Code customization features (CLAUDE.md, subagents, hooks, MCP), distributing skills across teams via Git/plugins/enterprise settings, and finally a systematic troubleshooting workflow for when things don't work.

### What Makes This Course Useful

- **Practical and code-first** — every lesson is anchored in real `SKILL.md` files, frontmatter, directory structures, and CLI commands you can copy
- **Progressive complexity** — starts with a 4-line skill and ends with multi-file progressive-disclosure architectures, subagent integration, and enterprise deployment
- **Covers the entire skill lifecycle** — creation → configuration → comparison with other features → distribution → troubleshooting
- **Teaches a decision framework** — by the end you know not just *how* to build skills, but *when* a skill is the right tool vs. CLAUDE.md, subagents, hooks, or MCP

### The Core Idea

> "If you find yourself explaining the same thing to Claude repeatedly, that's a skill waiting to be written."

Skills are how you encode **task-specific expertise** once and let Claude apply it automatically whenever the situation arises. Unlike CLAUDE.md (which loads into every conversation and burns context) or slash commands (which require explicit invocation), skills are **automatic and on-demand**: only the skill name and description sit in context at startup, and the full instructions load only when Claude matches your request.

---

## 🎯 Key Concepts (at a glance)

| Concept | Summary |
|---|---|
| **Skill** | A folder containing a `SKILL.md` file with `name` and `description` in YAML frontmatter, plus optional instructions, references, scripts, and assets |
| **Personal skills** | Live in `~/.claude/skills` and follow you across all projects |
| **Project skills** | Live in `.claude/skills` inside a repo and are shared via Git |
| **Skill matching** | Claude compares your request against all available skill *descriptions* and activates matching ones on demand |
| **Skill priority** | Enterprise → Personal → Project → Plugins (highest to lowest) |
| **Frontmatter fields** | `name` (required), `description` (required), `allowed-tools` (optional), `model` (optional) |
| **Progressive disclosure** | Keep `SKILL.md` under ~500 lines; link to `references/`, `scripts/`, and `assets/` that load only when needed |
| **Skills vs. CLAUDE.md** | Skills are on-demand and task-specific; CLAUDE.md is always-on and conversation-wide |
| **Skills vs. subagents** | Skills add knowledge to the current conversation; subagents run in an isolated context |
| **Skills vs. hooks** | Skills are request-driven; hooks are event-driven (file save, tool call) |
| **Sharing methods** | Git repo, plugins/marketplaces, enterprise managed settings |
| **Subagent + skills** | Subagents don't inherit skills automatically — you must list them in the agent's `skills` frontmatter |
| **Skills validator** | `agent-skills-verifier` (or similar) catches structural issues before runtime debugging |
| **`claude --debug`** | CLI flag to see skill loading errors |

---

## 📖 Table of Contents

1. [What are skills?](#lesson-1-what-are-skills) — 15 min
2. [Creating your first skill](#lesson-2-creating-your-first-skill) — 20 min
3. [Configuration and multi-file skills](#lesson-3-configuration-and-multi-file-skills) — 20 min
4. [Skills vs. other Claude Code features](#lesson-4-skills-vs-other-claude-code-features) — 15 min
5. [Sharing skills](#lesson-5-sharing-skills) — 20 min
6. [Troubleshooting skills](#lesson-6-troubleshooting-skills) — 15 min

Then: [Course Summary](#-course-summary) · [Quick Reference: Commands & Syntax](#-quick-reference-commands--syntax)

---

## Lesson 1: What are skills?

**Estimated time:** 15 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Define what Claude Code skills are and how they work
- Explain where skills live (personal vs. project directories)
- Distinguish between skills, CLAUDE.md, and slash commands
- Identify scenarios where skills are the right customization tool

### Key Takeaways

- Skills are **folders of instructions** that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a `SKILL.md` file with a `name` and `description` in its YAML frontmatter
- Claude uses the **description** to match skills to requests. When you ask Claude to do something, it compares your request against available skill descriptions and activates the ones that match
- **Personal skills** go in `~/.claude/skills` and follow you across all projects. **Project skills** go in `.claude/skills` inside a repository and are shared with anyone who clones it
- Skills **load on demand** — unlike CLAUDE.md (which loads into every conversation) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
- If you find yourself explaining the same thing to Claude repeatedly, **that's a skill waiting to be written**

### Detailed Notes

Every PR review, you re-describe how you want feedback structured. Every commit, you remind Claude of your preferred format. Every docs request, you re-explain your style guide. Skills fix this repetition. A skill is a markdown file that teaches Claude how to do something once; Claude then applies that knowledge automatically whenever the relevant task comes up.

A skill is a folder containing a `SKILL.md` file. The frontmatter holds the metadata — a `name` and a `description` — and everything below the closing `---` is the actual instruction body. The description is the most important field, because Claude uses it to decide whether the skill applies to your current request. When you ask Claude to review a PR, it reads your request, compares it semantically to all available skill descriptions, and activates the ones that match. Only the name and description sit in context at startup; the full instructions load only after a match is confirmed.

Skills live in one of two main locations. **Personal skills** go in `~/.claude/skills` (or `C:/Users/<your-user>/.claude/skills` on Windows) and follow you across every project — your commit-message style, your doc format, your preferred code-explanation style. **Project skills** go in `.claude/skills` inside the root of a repository, get committed to version control, and are automatically available to anyone who clones the repo. Project skills are where team standards, brand guidelines, and codebase-specific workflows live.

The key differentiator from other Claude Code customization features is **automatic, on-demand, task-specific loading**. CLAUDE.md files load into every conversation whether they're relevant or not, which burns context. Slash commands require you to type them explicitly. Skills do neither: Claude activates them only when it recognizes a matching situation, and only the full body of the matched skill joins the conversation. The rule of thumb: if you find yourself explaining the same thing to Claude repeatedly, that's a skill waiting to be written. Use it for code review standards, commit message formats, brand guidelines, documentation templates, and debugging checklists — anywhere specialized knowledge applies to a specific recurring task.

### Code Example — Minimal Skill

```markdown
---
name: pr-review
description: Reviews pull requests for code quality. Use when reviewing PRs or checking code changes.
---

When reviewing a pull request:
1. Check for missing tests on new behavior
2. Flag any breaking changes in the public API
3. Verify error messages are user-friendly
```

That's a complete skill: three lines of frontmatter, then the instructions Claude follows when it activates.

---

## Lesson 2: Creating your first skill

**Estimated time:** 20 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Create a skill from scratch with proper frontmatter structure
- Test and verify that a skill loads correctly in Claude Code
- Explain how Claude Code matches incoming requests to available skills
- Describe the skill priority hierarchy (Enterprise, Personal, Project, Plugins)

### Key Takeaways

- A skill is a **directory** containing a `SKILL.md` file with metadata (`name`, `description`) in frontmatter and instructions below
- Claude loads only skill **names and descriptions** at startup, then matches incoming requests against those descriptions using semantic matching
- You get a **confirmation prompt** before Claude loads the full skill content into context
- **Priority for name conflicts:** Enterprise → Personal → Project → Plugins
- To update a skill, edit its `SKILL.md`. To remove one, delete its directory. **Always restart Claude Code** for changes to take effect

### Detailed Notes

The lesson walks through building a **personal PR description skill** end to end. The skill is personal (it lives in `~/.claude/skills`) so it works across all your projects. You start by creating a directory whose name matches the skill name — `mkdir -p ~/.claude/skills/pr-description` — then write a `SKILL.md` file inside it. The file has two parts separated by frontmatter dashes: the YAML metadata on top, the actual instructions below.

The name identifies the skill. The description is the **matching criteria** — it tells Claude when the skill should activate. Every word of the description matters because Claude uses semantic matching: a request like "write a PR description for my changes" should overlap semantically with phrases like "Use when creating a PR, writing a PR, or when the user asks to summarize changes for a pull request." You should write the description the way you would describe the trigger to a colleague.

The matching flow has three steps. When Claude Code starts, it scans four locations for skills (enterprise, personal, project, plugins) but only loads the **name and description** of each — not the full content. When you send a request, Claude compares your message against all available descriptions. Once a match is found, Claude asks you to **confirm loading the skill** — this confirmation step keeps you aware of what context is about to be pulled in. After you confirm, Claude reads the full `SKILL.md` and follows its instructions.

When names collide, there is a strict priority order: **Enterprise > Personal > Project > Plugins**. An enterprise `code-review` skill always wins over your personal `code-review` skill. This is by design: it lets organizations enforce mandatory standards while still allowing individual customization. To avoid surprise conflicts, use descriptive names like `frontend-review` or `backend-review` instead of generic `review`. To update a skill, edit its `SKILL.md`; to remove one, delete its directory. Claude Code caches the skill list at startup, so **you must restart Claude Code for any changes to take effect**.

### Code Example — PR Description Skill

Directory: `~/.claude/skills/pr-description/SKILL.md`

```markdown
---
name: pr-description
description: Writes pull request descriptions. Use when creating a PR, writing a PR, or when the user asks to summarize changes for a pull request.
---

When writing a PR description:

1. Run `git diff main...HEAD` to see all changes on this branch
2. Write a description following this format:

## What
One sentence explaining what this PR does.

## Why
Brief context on why this change is needed

## Changes
- Bullet points of specific changes made
- Group related changes together
- Mention any files deleted or renamed
```

To test: restart Claude Code, make some branch changes, then say "write a PR description for my changes." Claude will indicate it matched the skill, prompt you to confirm, then produce a description in the exact format every time.

---

## Lesson 3: Configuration and multi-file skills

**Estimated time:** 20 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Configure advanced skill metadata fields including `allowed-tools` and `model`
- Write effective skill descriptions that reliably trigger on the right requests
- Use `allowed-tools` to restrict what Claude can do when a skill is active
- Organize complex skills using progressive disclosure and multi-file structures

### Key Takeaways

- `name` and `description` are **required** — `allowed-tools` and `model` are optional but powerful additions
- A good description answers **two questions**: What does the skill do? When should Claude use it?
- `allowed-tools` restricts which tools Claude can use when the skill is active — useful for read-only or security-sensitive workflows
- **Progressive disclosure:** keep `SKILL.md` under 500 lines and link to supporting files (references, scripts, assets) that Claude reads only when needed
- Scripts execute without loading their contents into context — only the output consumes tokens, keeping context efficient

### Detailed Notes

A basic skill works with just `name` and `description`, but the frontmatter supports two more optional fields that unlock powerful use cases. `allowed-tools` is a comma-separated list that restricts which tools Claude can invoke when the skill is active — set it to `Read, Grep, Glob, Bash` and the skill becomes effectively read-only, perfect for onboarding tours, code audits, or any workflow where you want guardrails. `model` lets you pin a specific Claude model for the skill (e.g. `sonnet`) when you need a particular cost/quality profile. If you omit `allowed-tools`, Claude uses its normal permission model.

The **description** is the most important field, and writing it well is the single biggest lever for skill reliability. A good description answers two questions: what does the skill do, and when should Claude use it. If a skill isn't triggering when you expect, the cause is almost always that your description doesn't overlap semantically with how you actually phrase requests. The fix is to add trigger phrases and keywords that match the language you use — and to test with variations like "help me profile this," "why is this slow?", "make this faster." If any variation fails to trigger, add those keywords to the description.

As skills grow, cramming everything into one file has two problems: it eats context window space and it becomes a maintenance nightmare. The open standard solves this with **progressive disclosure**. Keep essential instructions in `SKILL.md` (rule of thumb: under 500 lines) and put detailed material in separate files that Claude reads only when needed. The recommended directory layout adds three subfolders:

- `scripts/` — executable code
- `references/` — additional documentation
- `assets/` — images, templates, or other data files

In `SKILL.md`, link to the supporting files with clear instructions about *when* to load them. The mental model is **table of contents in the context window, not the entire document**. For example, a `codebase-onboarding` skill might link to `architecture-guide.md` and instruct Claude to read it only when someone asks about system design — never when they're asking where to add a new component.

A particularly powerful progressive-disclosure trick: **scripts run without loading their contents into context**. The script executes and only its output consumes tokens. The key instruction in `SKILL.md` is to tell Claude to *run* the script, not *read* it. This is ideal for environment validation, deterministic data transformations, and any operation that's more reliable as tested code than as generated code.

### Code Example — Restricted-Tool Skill

```markdown
---
name: codebase-onboarding
description: Helps new developers understand the system works.
allowed-tools: Read, Grep, Glob, Bash
model: sonnet
---

When onboarding a new developer:

1. Run `scripts/check-env.sh` to verify their environment is set up correctly
2. Read `references/architecture-guide.md` only when the developer asks about system design
3. Use Grep to find example usages of key modules — do not list every file
```

The `allowed-tools` line means Claude can only use Read/Grep/Glob/Bash without permission prompts — no Write, no Edit, no destructive operations.

### Multi-File Layout

```
my-skill/
├── SKILL.md              # Required. Under ~500 lines. The entry point.
├── scripts/              # Executable code — runs without loading into context
│   └── validate.sh
├── references/           # Additional docs — loaded on demand
│   └── architecture-guide.md
└── assets/               # Templates, images, data files
    └── template.md
```

---

## Lesson 4: Skills vs. other Claude Code features

**Estimated time:** 15 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Compare skills to CLAUDE.md, subagents, hooks, and MCP servers
- Choose the right Claude Code customization feature for a given use case
- Design a complementary setup that combines multiple features effectively

### Key Takeaways

- **CLAUDE.md** loads into every conversation and is best for always-on project standards. **Skills** load on demand and are best for task-specific expertise
- **Subagents** run in isolated execution contexts — use them for delegated work. **Skills** add knowledge to your current conversation
- **Hooks** are event-driven (fire on file saves, tool calls). **Skills** are request-driven (activate based on what you're asking)
- **MCP servers** provide external tools and integrations — a different category entirely from skills
- Each feature handles its own specialty — **combine them** rather than forcing everything into one approach

### Detailed Notes

Claude Code offers five customization surfaces — Skills, CLAUDE.md, subagents, hooks, and MCP servers — and they solve different problems. Choosing the wrong one leads to unnecessary complexity, so the lesson walks through each pair comparison.

**CLAUDE.md vs Skills.** CLAUDE.md loads into *every* conversation, always. If you want Claude to always use TypeScript strict mode in your project, that goes in CLAUDE.md. Skills load on demand: only the matching ones activate, and only the body of the activated skill joins the conversation. Use CLAUDE.md for project-wide standards, hard constraints ("never modify the database schema"), and framework preferences. Use skills for task-specific expertise, knowledge that applies only sometimes, and detailed procedures that would clutter every conversation. A useful sanity check: scan your CLAUDE.md and ask whether anything in it would work better as a skill that loads only when relevant.

**Skills vs Subagents.** Skills add knowledge to your *current* conversation — the skill's instructions join the existing context. Subagents run in a *separate* context, receive a task, work on it independently, and return results. Use subagents when you want to delegate a task to an isolated execution context, when you need different tool access than the main conversation, or when you want isolation between delegated work and your main context. Use skills when you want to enhance Claude's knowledge for the task at hand and the expertise applies throughout the conversation.

**Skills vs Hooks.** Hooks are event-driven — a hook runs a linter every time Claude saves a file, or validates input before a particular tool call. Skills are request-driven — they activate based on what you're asking. Use hooks for operations that should run on every file save, validation before specific tool calls, and automated side effects. Use skills for knowledge that informs how Claude reasons about requests and guidelines that affect Claude's decision-making.

**MCP servers** are a different category — they provide external tools and integrations (databases, browsers, internal APIs). A typical production setup combines all five: CLAUDE.md for always-on project standards, skills for task-specific expertise that loads on demand, hooks for automated operations triggered by events, subagents for isolated delegated work, and MCP servers for external tool access. The principle: **don't force everything into skills when another option fits better** — and you can use multiple at a time. Skills are the right tool when you have knowledge that Claude should apply automatically when the topic is relevant, and they combine cleanly with the rest.

### Decision Framework

| Need | Use |
|---|---|
| "Always follow these rules in this project" | **CLAUDE.md** |
| "Apply this expertise when the topic comes up" | **Skill** |
| "Delegate this task with isolated context" | **Subagent** |
| "Run this on every file save / before this tool" | **Hook** |
| "Let Claude talk to my database / API" | **MCP server** |

---

## Lesson 5: Sharing skills

**Estimated time:** 20 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Share skills with your team by committing them to a Git repository
- Distribute skills across projects through plugins and marketplaces
- Deploy skills organization-wide using enterprise managed settings
- Configure custom subagents to use specific skills

### Key Takeaways

- **Project skills** in `.claude/skills` are shared automatically through Git — anyone who clones the repo gets them
- **Plugins** let you distribute skills across repositories via marketplaces for broader community use
- **Enterprise managed settings** deploy skills organization-wide with the highest priority, ideal for mandatory standards and compliance
- **Subagents don't automatically see your skills** — you must explicitly list skills in a custom agent's frontmatter `skills` field
- **Built-in agents** (Explorer, Plan, Verify) **can't access skills at all** — only custom subagents defined in `.claude/agents` can

### Detailed Notes

A PR review skill that only you use is helpful, but the same skill shared across your entire team standardizes reviews and creates a consistent experience across the organization. The lesson covers three distribution methods plus the subagent-integration gotcha.

**Committing to your repository** is the simplest path. Place skills in `.claude/skills`, commit them, and anyone who clones the repo gets them automatically — no installation step. When you push updates, everyone gets them on the next pull. This is the right approach for team coding standards, project-specific workflows, and skills that reference your codebase structure. The whole `.claude` directory (agents, hooks, skills, settings) is version-controlled alongside your code.

**Plugins** are the way to extend Claude Code with functionality designed to be shared across teams and projects. In your plugin project, create a `skills` directory that follows the same file structure as `.claude/skills` — each skill gets its own folder with a `SKILL.md` inside. After you distribute the plugin to a marketplace, other users can discover and install it into their Claude Code. Use plugins when your skills aren't too project-specific and can be useful to community members beyond your immediate team.

**Enterprise managed settings** are the right choice for mandatory standards, security requirements, compliance workflows, and coding practices that *must* be consistent across the organization. The keyword is "must." Enterprise skills take the highest priority — they override personal, project, and plugin skills with the same name. The managed settings file also supports `strictKnownMarketplaces` to control where plugins can be installed from, which is a useful guardrail for regulated environments.

The **subagent gotcha** trips up almost everyone the first time: subagents don't automatically see your skills. When you delegate a task to a subagent, it starts with a fresh, clean context. Three important distinctions: built-in agents (`Explorer`, `Plan`, `Verify`) **cannot access skills at all**; only custom subagents you define can use skills; and even then you must explicitly list them. Skills are loaded when the subagent starts, not on demand like in the main conversation. To create a custom subagent with skills, add an agent markdown file in `.claude/agents` (the `/agents` command walks you through it interactively). The generated frontmatter includes a `skills` field listing which skills to load. This pattern works really well for isolated task delegation with specific expertise, for differentiating subagents (frontend reviewer vs. backend reviewer), and for enforcing standards in delegated work without relying on prompts.

### Code Example — Custom Subagent with Skills

File: `.claude/agents/frontend-security-accessibility-reviewer.md`

```markdown
---
name: frontend-security-accessibility-reviewer
description: "Use this agent when you need to review frontend code for accessibility..."
tools: Bash, Glob, Grep, Read, WebFetch, WebSearch, Skill
model: sonnet
color: blue
skills: accessibility-audit, performance-check
---
```

When you delegate to this subagent, both skills are loaded at start and applied to every review. Make sure the skills exist in `.claude/skills` first, then either create a new subagent or add the `skills` field to an existing agent's markdown file.

### Code Example — Enterprise `strictKnownMarketplaces`

```json
{
  "strictKnownMarketplaces": [
    {
      "source": "github",
      "repo": "acme-corp/approved-plugins"
    },
    {
      "source": "npm",
      "package": "@acme-corp/compliance-plugins"
    }
  ]
}
```

This restricts which plugin marketplaces can be installed from in an enterprise environment.

### Distribution Method Comparison

| Method | Audience | Use when |
|---|---|---|
| **Git repo** (`.claude/skills`) | Your team | Team standards, project-specific workflows |
| **Plugin / marketplace** | Community | Cross-project, broadly useful skills |
| **Enterprise managed settings** | Whole org | Mandatory standards, compliance, security |
| **Custom subagent + skills** | Isolated delegated work | Specific expertise needed in a sub-context |

---

## Lesson 6: Troubleshooting skills

**Estimated time:** 15 minutes

### Learning Objectives

By the end of this lesson you'll be able to:

- Use the skills validator to catch structural issues before debugging
- Diagnose and fix common skill triggering and loading problems
- Resolve skill priority conflicts between enterprise, personal, project, and plugin skills
- Debug runtime errors including missing dependencies, permissions, and path issues

### Key Takeaways

- **Start with the skills validator** — it catches structural problems before you spend time debugging other things
- If a skill **doesn't trigger**, the cause is almost always the description — add trigger phrases that match how you actually phrase requests
- If a skill **doesn't load**, check that `SKILL.md` is inside a named directory (not at the skills root) and the file name is exactly `SKILL.md`
- If the **wrong skill gets used**, your descriptions are too similar — make them more distinct
- For **runtime errors**, check dependencies, file permissions (`chmod +x`), and path separators (use forward slashes everywhere)

### Detailed Notes

When skills don't work, the problem usually falls into one of four predictable categories: the skill doesn't trigger, doesn't load, has conflicts, or fails at runtime. The lesson walks through each with a systematic fix.

**Start with the skills validator.** The first thing to try is the `agent-skills-verifier` (or equivalent) command. Installation steps vary by OS, but `uv` is the easiest way to set it up. Once installed, navigate to your skill directory (or run from anywhere) and the validator will catch structural problems — missing frontmatter, malformed YAML, wrong file name — before you burn time on the more interesting debugging rabbit holes. The validator is the highest-leverage tool in this entire course.

**Skill doesn't trigger.** The skill exists and passes validation, but Claude isn't using it when you expect. The cause is almost always the description. Claude uses semantic matching, so your request needs to overlap with the description's *meaning*. If there's not enough overlap, no match. The fix: check your description against how you actually phrase requests, add trigger phrases users would actually say, test with variations like "help me profile this," "why is this slow?", "make this faster" — and if any variation fails to trigger, add those keywords to the description.

**Skill doesn't load.** If your skill doesn't appear when you ask Claude "what skills are available," check the structural requirements: the `SKILL.md` file must be inside a **named directory**, not at the skills root, and the file name must be **exactly `SKILL.md`** — all caps on "SKILL", lowercase "md". Run `claude --debug` to see loading errors. Look for messages mentioning your skill name; sometimes this alone points straight to the problem.

**Wrong skill gets used.** If Claude uses the wrong skill or seems confused between similar skills, your descriptions are probably too similar. Make them distinct. Specificity helps Claude decide when to use your skill *and* prevents conflicts with other similar-sounding skills.

**Skill priority conflicts.** If your personal skill is being ignored, a higher-priority skill probably has the same name. An enterprise `code-review` skill beats your personal `code-review` skill every time. Your options: rename your skill to something more distinct (usually the easier path), or talk to your admin about the enterprise skill.

**Plugin skills not appearing.** Installed a plugin but can't see its skills? Clear the cache, restart Claude Code, and reinstall. If skills still don't appear, the plugin structure is probably wrong — this is when the validator tool really earns its keep.

**Runtime errors.** The skill loads but fails during execution. Common causes: missing dependencies (external packages must be installed — add dependency info to the description so Claude knows what's needed), permission issues (scripts need execute permission — run `chmod +x` on any script your skill references), and path separators (use forward slashes everywhere, even on Windows).

### Quick Troubleshooting Checklist

| Symptom | First thing to check |
|---|---|
| **Not triggering** | Improve description; add trigger phrases users actually say |
| **Not loading** | Verify path (named dir), file name (`SKILL.md` exact case), YAML syntax; run `claude --debug` |
| **Wrong skill used** | Make descriptions more distinct from each other |
| **Being shadowed** | Check priority hierarchy (Enterprise > Personal > Project > Plugins); rename if needed |
| **Plugin skills missing** | Clear cache, restart Claude Code, reinstall |
| **Runtime failure** | Check dependencies, file permissions (`chmod +x`), path separators (forward slashes) |

### Validator Setup (uv)

```bash
# Install via uv (recommended)
uv tool install agent-skills-verifier

# Then run from anywhere, or navigate to your skill directory
agent-skills-verifier ~/.claude/skills/my-skill
```

---

## 🧭 Course Summary

### Core Themes

1. **Automation through declarative instructions.** Skills let you encode "do this the same way every time" as a markdown file, replacing repeated prompting with one-time authoring and automatic activation.
2. **Context economy via description-driven matching.** Skills keep your context window lean by loading only `name` and `description` at startup, then pulling in full instructions only when a request semantically matches.
3. **Progressive disclosure for complex skills.** Multi-file layouts (`scripts/`, `references/`, `assets/`) let skills scale to thousands of lines of supporting material without ever loading it all into context.
4. **Layered customization surfaces.** Skills complement CLAUDE.md, subagents, hooks, and MCP — each handles a different specialty, and the right answer is usually a combination, not a single feature.
5. **Distribution as a first-class concern.** Skills gain value through sharing — Git for teams, plugins for communities, enterprise managed settings for mandatory standards.
6. **Systematic debugging.** The validator + `claude --debug` + a six-row troubleshooting checklist cover nearly every failure mode you'll encounter.

### What You Can Build After This Course

- **Personal skills** that follow you across every project: commit-message formatter, PR-description writer, code-review checklist, doc style enforcer, debugging playbook
- **Team standards as project skills** in `.claude/skills`, version-controlled and shared via Git: brand guidelines, framework preferences, release process checklists, security review templates
- **Read-only and security-sensitive skills** that use `allowed-tools` to enforce guardrails: code auditors, environment validators, compliance checkers
- **Large multi-file skills** using progressive disclosure for onboarding, architecture reviews, migration playbooks, or any domain with deep reference material
- **Plugins** that bundle skills for marketplace distribution — useful for cross-project tooling, open-source contributions, or shared libraries
- **Custom subagents** with explicit `skills` fields for delegated work that needs specific expertise (e.g. a frontend-security-accessibility-reviewer that always loads `accessibility-audit` and `performance-check`)
- **An enterprise rollout plan** with managed settings, `strictKnownMarketplaces`, and a clear priority hierarchy

### Mindset Shifts

- **From "tell Claude every time" to "tell Claude once, encoded as a skill"** — every recurring instruction is a skill opportunity
- **From "put everything in CLAUDE.md" to "match the customization surface to the use case"** — CLAUDE.md for always-on, skills for on-demand, hooks for events, subagents for delegation, MCP for external tools
- **From "the skill is the file" to "the skill is the directory"** — `SKILL.md` is the entry point; supporting `references/`, `scripts/`, and `assets/` make it scale
- **From "ship the skill" to "validate, then ship"** — the validator tool is cheap insurance against structural bugs

---

## 📋 Quick Reference: Commands & Syntax

### Required Frontmatter (the minimum)

```yaml
---
name: skill-name
description: One-sentence summary. Tell Claude what the skill does AND when to use it.
---
```

### Full Frontmatter (all supported fields)

```yaml
---
name: skill-name              # Required. Lowercase, numbers, hyphens. Max 64 chars. Match dir name.
description: ...              # Required. Max 1024 chars. The matching criteria.
allowed-tools: Read, Grep     # Optional. Restricts tools when skill is active.
model: sonnet                 # Optional. Pins a specific Claude model.
---
```

### Directory Structure

```
my-skill/                     # Directory name matches `name`
├── SKILL.md                  # Required. Under ~500 lines. The entry point.
├── scripts/                  # Optional. Executable code.
│   └── check-env.sh          #   Runs without loading contents into context.
├── references/               # Optional. Additional docs loaded on demand.
│   └── architecture-guide.md
└── assets/                   # Optional. Templates, images, data files.
    └── template.md
```

### Where Skills Live

| Scope | Path | Shared via |
|---|---|---|
| **Personal** | `~/.claude/skills/` (Unix) or `C:/Users/<you>/.claude/skills/` (Windows) | Follows you across all projects |
| **Project** | `<repo>/.claude/skills/` | Git (auto with clone) |
| **Plugin** | `<plugin>/skills/` | Plugin marketplace |
| **Enterprise** | Managed settings | Org-wide deployment (highest priority) |

### Skill Priority Hierarchy

**Enterprise → Personal → Project → Plugins** (highest to lowest)

When two skills share a name, the higher-priority one wins. To avoid surprise shadowing, use descriptive names like `frontend-review` instead of `review`.

### Lifecycle Commands

```bash
# Create a skill
mkdir -p ~/.claude/skills/my-skill
# ... write SKILL.md ...

# Update a skill — just edit the file, then restart Claude Code
vim ~/.claude/skills/my-skill/SKILL.md

# Remove a skill — delete the directory
rm -rf ~/.claude/skills/my-skill

# Always restart Claude Code after any skill change
```

### Validator Command

```bash
# Install (uv, recommended)
uv tool install agent-skills-verifier

# Run from inside the skill directory
cd ~/.claude/skills/my-skill
agent-skills-verifier .

# Or run from anywhere
agent-skills-verifier ~/.claude/skills/my-skill
```

### Debugging

```bash
# See skill loading errors
claude --debug
# Look for lines mentioning your skill name
```

### Custom Subagent with Skills

File: `.claude/agents/<agent-name>.md`

```markdown
---
name: my-reviewer
description: "Use this agent when..."
tools: Bash, Glob, Grep, Read, Skill
model: sonnet
color: blue
skills: skill-a, skill-b        # Required: skills are NOT inherited automatically
---
```

Built-in agents (Explorer, Plan, Verify) **cannot use skills**. Only custom subagents in `.claude/agents` can.

### Enterprise Managed Settings (excerpt)

```json
{
  "strictKnownMarketplaces": [
    { "source": "github", "repo": "acme-corp/approved-plugins" },
    { "source": "npm",   "package": "@acme-corp/compliance-plugins" }
  ]
}
```

### Description Writing Checklist

A good description answers two questions:

- [ ] **What** does the skill do? (one short clause)
- [ ] **When** should Claude use it? (trigger phrases, keywords users would actually say)

If a skill isn't triggering, the description is the most likely culprit. Add the trigger phrases you actually use in real requests.

### Troubleshooting Cheat Sheet

| Symptom | Fix |
|---|---|
| Not triggering | Improve description; add trigger phrases |
| Not loading | `SKILL.md` must be in a named dir, exact-case filename; run `claude --debug` |
| Wrong skill used | Make descriptions more distinct |
| Being shadowed | Check priority order; rename if needed |
| Plugin skills missing | Clear cache, restart, reinstall |
| Runtime failure | Check deps, `chmod +x` scripts, use forward slashes |

---

## 🔗 Resources

- **Course URL:** https://anthropic.skilljar.com/introduction-to-agent-skills
- **Author:** Anthropic Academy
- **Related Anthropic skills spec:** Agent Skills open standard (YAML frontmatter: `name`, `description`, `allowed-tools`, `model`)
- **Validator tool:** `agent-skills-verifier` (install via `uv tool install`)
- **Companion concepts in this repo:** `08-claude-code-101` (general Claude Code features), `05-introduction-to-mcp` (MCP servers — a different customization surface mentioned in Lesson 4)

---

*Notes generated from the Anthropic Academy course "Introduction to Agent Skills" — 6 lessons, ~105 minutes. All code examples are reproduced from the course material.*
