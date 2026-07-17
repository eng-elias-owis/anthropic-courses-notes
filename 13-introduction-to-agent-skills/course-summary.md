# Introduction to Agent Skills - Course Summary

**Course URL:** https://anthropic.skilljar.com/introduction-to-agent-skills

---

## 🎯 Course Overview

Learn to create, configure, and share Claude Code skills. Covers what skills are, creating your first skill, multi-file skill configurations, differences between skills and other Claude Code features, sharing, and troubleshooting.

---

## 📚 Table of Contents

  📌 What are skills?
  📌 Creating your first skill
  📌 Configuration and multi-file skills
  📌 Skills vs. other Claude Code features
  📌 Sharing skills
  📌 Troubleshooting skills

---

## 📖 Lesson Content

#### 1. What are skills?

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
What are skills?

What you'll learn

Estimated time: 15 minutes

By the end of this lesson you'll be able to:

Define what Claude Code skills are and how they work
Explain where skills live (personal vs. project directories)
Distinguish between skills, CLAUDE.md, and slash commands
Identify scenarios where skills are the right customization tool
What are skills?

(3 minutes)

This video introduces skills — reusable markdown files that teach Claude Code how to handle specific tasks automatically. Instead of repeating instructions every time you ask Claude to review a PR or write a commit message, you write a skill once and Claude applies it whenever the task comes up. The video covers what skills are, where they live, and how they compare to other Claude Code customization options.

Key takeaways
Skills are folders of instructions that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a SKILL.md file with a name and description in its frontmatter
Claude uses the description to match skills to requests. When you ask Claude to do something, it compares your request against available skill descriptions and activates the ones that match
Personal skills go in ~/.claude/skills and follow you across all projects. Project skills go in .claude/skills inside a repository and are shared with anyone who clones it
Skills load on demand — unlike CLAUDE.md (which loads into every conversation) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
If you find yourself explaining the same thing to Claude repeatedly, that's a skill waiting to be written

Every time you explain your team's coding standards to Claude, you're repeating yourself. Every PR review, you re-describe how you want feedback structured. Every commit message, you remind Claude of your preferred format. Skills fix this.

A skill is a markdown file that teaches Claude how to do something once. Claude then applies that knowledge automatically whenever it's relevant.

What Skills Are

Skills are folders of instructions and resources that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a SKILL.md file with a name and description in its frontmatter.


> *(See full lesson at course URL)*

#### 2. Creating your first skill

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Creating your first skill

What you'll learn

Estimated time: 20 minutes

By the end of this lesson you'll be able to:

Create a skill from scratch with proper frontmatter structure
Test and verify that a skill loads correctly in Claude Code
Explain how Claude Code matches incoming requests to available skills
Describe the skill priority hierarchy (Enterprise, Personal, Project, Plugins)
Creating your first skill

(4 minutes)

This video walks through building a skill from scratch — a personal PR description skill that works across all your projects. You'll see exactly how to structure the SKILL.md file, test it, and understand how Claude Code discovers and matches skills to your requests. The video also covers the priority hierarchy that determines which skill wins when names conflict.

Key takeaways
A skill is a directory containing a SKILL.md file with metadata (name, description) in frontmatter and instructions below
Claude loads only skill names and descriptions at startup, then matches incoming requests against those descriptions using semantic matching
You get a confirmation prompt before Claude loads the full skill content into context
Priority for name conflicts: Enterprise → Personal → Project → Plugins
To update a skill, edit its SKILL.md. To remove one, delete its directory. Always restart Claude Code for changes to take effect

Let's walk through creating a skill from scratch, then look at how Claude Code actually loads and matches skills behind the scenes.

Creating a Skill

We'll build a personal skill that teaches Claude how to write PR descriptions in a consistent format. Since it's a personal skill, it lives in your home directory and works across all your projects.

First, create a directory for your skill inside the skills folder. The directory name should match your skill name:

mkdir -p ~/.claude/skills/pr-description

Then create a SKILL.md file inside that directory. The file has two parts separated by frontmatter dashes:

---
name: pr-description
description: Writes pull request descriptions. Use when creating a PR, writing a PR, or when the user asks to summarize changes for a pull request.
---

When writing a PR description:

1. Run `git diff main...HEAD` to see all changes on this branch
2. Write a description following this format:

## What

> *(See full lesson at course URL)*

#### 3. Configuration and multi-file skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Configuration and multi-file skills

What you'll learn

Estimated time: 20 minutes

By the end of this lesson you'll be able to:

Configure advanced skill metadata fields including allowed-tools and model
Write effective skill descriptions that reliably trigger on the right requests
Use allowed-tools to restrict what Claude can do when a skill is active
Organize complex skills using progressive disclosure and multi-file structures
Configuration and multi-file skills

(4 minutes)

This video covers the advanced techniques that make skills more powerful: the full set of metadata fields, how to write descriptions that trigger reliably, restricting tool access for security-sensitive workflows, and organizing larger skills across multiple files using progressive disclosure. You'll learn how to keep your skills efficient while still supporting complex use cases.

Key takeaways
name and description are required — allowed-tools and model are optional but powerful additions
A good description answers two questions: What does the skill do? When should Claude use it?
allowed-tools restricts which tools Claude can use when the skill is active — useful for read-only or security-sensitive workflows
Progressive disclosure: keep SKILL.md under 500 lines and link to supporting files (references, scripts, assets) that Claude reads only when needed
Scripts execute without loading their contents into context — only the output consumes tokens, keeping context efficient

A basic skill works with just a name and description, but there are several advanced techniques that can make your skills much more effective in Claude Code. Let's walk through the key fields, best practices for descriptions, tool restrictions, and how to structure larger skills.

Skill Metadata Fields

The agent skills open standard supports several fields in the SKILL.md frontmatter. Two are required, and the rest are optional:

name (required) — Identifies your skill. Use lowercase letters, numbers, and hyphens only. Maximum 64 characters. Should match your directory name.
description (required) — Tells Claude when to use the skill. Maximum 1,024 characters. This is the most important field because Claude uses it for matching.
allowed-tools (optional) — Restricts which tools Claude can use when the skill is active.

> *(See full lesson at course URL)*

#### 4. Skills vs. other Claude Code features

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Skills vs. other Claude Code features

What you'll learn

Estimated time: 15 minutes

By the end of this lesson you'll be able to:

Compare skills to CLAUDE.md, subagents, hooks, and MCP servers
Choose the right Claude Code customization feature for a given use case
Design a complementary setup that combines multiple features effectively
Skills vs. other Claude Code features

(3 minutes)

Claude Code offers several customization options, and choosing the wrong one can lead to unnecessary complexity. This video breaks down when to use skills versus CLAUDE.md, subagents, hooks, and MCP servers. You'll learn the key differences between each option and how they complement each other in a typical development setup.

Key takeaways
CLAUDE.md loads into every conversation and is best for always-on project standards. Skills load on demand and are best for task-specific expertise
Subagents run in isolated execution contexts — use them for delegated work. Skills add knowledge to your current conversation
Hooks are event-driven (fire on file saves, tool calls). Skills are request-driven (activate based on what you're asking)
MCP servers provide external tools and integrations — a different category entirely from skills
Each feature handles its own specialty — combine them rather than forcing everything into one approach

Claude Code offers several customization options: Skills, CLAUDE.md, subagents, hooks, and MCP servers. They solve different problems, and knowing when to use each prevents you from building the wrong thing. Let's break them down.

CLAUDE.md vs Skills

CLAUDE.md loads into every conversation, always. If you want Claude to use TypeScript strict mode in your project, put it in your CLAUDE.md file.

Skills load on demand. When Claude matches a request to a skill, that skill's instructions join the conversation. Your PR review checklist doesn't need to be in context when you're writing new code — it activates when you ask for a review.

Use CLAUDE.md for:

Project-wide standards that always apply
Constraints like "never modify the database schema"
Framework preferences and coding style

Use Skills for:

Task-specific expertise
Knowledge that's only relevant sometimes
Detailed procedures that would clutter every conversation
Skills vs Subagents


> *(See full lesson at course URL)*

#### 5. Sharing skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Sharing skills

What you'll learn

Estimated time: 20 minutes

By the end of this lesson you'll be able to:

Share skills with your team by committing them to a Git repository
Distribute skills across projects through plugins and marketplaces
Deploy skills organization-wide using enterprise managed settings
Configure custom subagents to use specific skills
Sharing skills

(4 minutes)

Skills become much more valuable when they're shared across a team or organization. This video covers the three main distribution methods — repository commits, plugins, and enterprise managed settings — and explains how to configure custom subagents to use skills. You'll learn which approach fits which scenario and how to handle an important gotcha: subagents don't inherit skills automatically.

Key takeaways
Project skills in .claude/skills are shared automatically through Git — anyone who clones the repo gets them
Plugins let you distribute skills across repositories via marketplaces for broader community use
Enterprise managed settings deploy skills organization-wide with the highest priority, ideal for mandatory standards and compliance
Subagents don't automatically see your skills — you must explicitly list skills in a custom agent's frontmatter skills field
Built-in agents (Explorer, Plan, Verify) can't access skills at all — only custom subagents defined in .claude/agents can

Skills become much more valuable when they're shared. A PR review skill that only you use is helpful, but that same skill shared across your entire team standardizes code review and creates a consistent experience across your organization. Let's look at the different ways you can distribute skills.

Committing Skills to Your Repository

The simplest sharing method is committing skills directly to your repository. Place them in .claude/skills, and anyone who clones the repo gets those skills automatically — no extra installation needed.

When you push updates, everyone gets them on the next pull. This approach works well for:

Team coding standards
Project-specific workflows
Skills that reference your codebase structure

The .claude directory contains your agents, hooks, skills, and settings — all version-controlled and shared with the team through normal Git workflows.

Distributing Skills Through Plugins


> *(See full lesson at course URL)*

#### 6. Troubleshooting skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Troubleshooting skills

What you'll learn

Estimated time: 15 minutes

By the end of this lesson you'll be able to:

Use the skills validator to catch structural issues before debugging
Diagnose and fix common skill triggering and loading problems
Resolve skill priority conflicts between enterprise, personal, project, and plugin skills
Debug runtime errors including missing dependencies, permissions, and path issues
Troubleshooting skills

(4 minutes)

When skills don't work as expected, the problem usually falls into a few predictable categories. This video walks through each one — from skills that don't trigger to priority conflicts to runtime failures — and gives you a systematic troubleshooting approach. You'll also learn about the skills validator tool and how to use claude --debug to diagnose loading issues.

Key takeaways
Start with the skills validator tool — it catches structural problems before you spend time debugging other things
If a skill doesn't trigger, the cause is almost always the description — add trigger phrases that match how you actually phrase requests
If a skill doesn't load, check that SKILL.md is inside a named directory (not at the skills root) and the file name is exactly SKILL.md
If the wrong skill gets used, your descriptions are too similar — make them more distinct
For runtime errors, check dependencies, file permissions (chmod +x), and path separators (use forward slashes everywhere)

When skills don't work, the problem usually falls into one of a few categories: the skill doesn't trigger, doesn't load, has conflicts, or fails at runtime. The good news is that most fixes are pretty straightforward.

Use the Skills Validator

The first thing to try is the agent skills verifier command. Installation steps vary by operating system, but using uv is the easiest way to get it set up quickly.

Once installed, either navigate to your skill directory or run the command from anywhere. The validator will catch structural problems before you spend time debugging other things.

Skill Doesn't Trigger

Your skill exists and passes validation, but Claude isn't using it when you expect. The cause is almost always the description.


> *(See full lesson at course URL)*


---

*Summary generated from course content at https://anthropic.skilljar.com/introduction-to-agent-skills*
