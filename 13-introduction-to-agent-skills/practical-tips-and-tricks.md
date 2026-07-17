# Introduction to Agent Skills - Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/introduction-to-agent-skills

---

## 📋 Course at a Glance

Learn to create, configure, and share Claude Code skills. Covers what skills are, creating your first skill, multi-file skill configurations, differences between skills and other Claude Code features, sharing, and troubleshooting.

---

## 🔑 Key Takeaways by Lesson

**From 'What are skills?':**
Skills are folders of instructions that Claude Code can discover and use to handle tasks more accurately. Each skill lives in a SKILL.md file with a name and description in its frontmatter
Claude uses the description to match skills to requests. When you ask Claude to do something, it compares your request against available skill descriptions and activates the ones that match
Personal skills go in ~/.claude/skills and follow you across all projects. Project skills go in .claude/skills inside a repository and are shared with anyone who clones it
Skills load on demand — unlike CLAUDE.md (which loads into every conversation) or slash commands (which require explicit invocation), skills activate automatically when Claude recognizes the situation
If you find yourself explaining the same thing t

**From 'Creating your first skill':**
A skill is a directory containing a SKILL.md file with metadata (name, description) in frontmatter and instructions below
Claude loads only skill names and descriptions at startup, then matches incoming requests against those descriptions using semantic matching
You get a confirmation prompt before Claude loads the full skill content into context
Priority for name conflicts: Enterprise → Personal → Project → Plugins
To update a skill, edit its SKILL.md. To remove one, delete its directory. Always restart Claude Code for changes to take effect

**From 'Configuration and multi-file skills':**
name and description are required — allowed-tools and model are optional but powerful additions
A good description answers two questions: What does the skill do? When should Claude use it?
allowed-tools restricts which tools Claude can use when the skill is active — useful for read-only or security-sensitive workflows
Progressive disclosure: keep SKILL.md under 500 lines and link to supporting files (references, scripts, assets) that Claude reads only when needed
Scripts execute without loading their contents into context — only the output consumes tokens, keeping context efficient

**From 'Skills vs. other Claude Code features':**
CLAUDE.md loads into every conversation and is best for always-on project standards. Skills load on demand and are best for task-specific expertise
Subagents run in isolated execution contexts — use them for delegated work. Skills add knowledge to your current conversation
Hooks are event-driven (fire on file saves, tool calls). Skills are request-driven (activate based on what you're asking)
MCP servers provide external tools and integrations — a different category entirely from skills
Each feature handles its own specialty — combine them rather than forcing everything into one approach

**From 'Sharing skills':**
Project skills in .claude/skills are shared automatically through Git — anyone who clones the repo gets them
Plugins let you distribute skills across repositories via marketplaces for broader community use
Enterprise managed settings deploy skills organization-wide with the highest priority, ideal for mandatory standards and compliance
Subagents don't automatically see your skills — you must explicitly list skills in a custom agent's frontmatter skills field
Built-in agents (Explorer, Plan, Verify) can't access skills at all — only custom subagents defined in .claude/agents can

**From 'Troubleshooting skills':**
Start with the skills validator tool — it catches structural problems before you spend time debugging other things
If a skill doesn't trigger, the cause is almost always the description — add trigger phrases that match how you actually phrase requests
If a skill doesn't load, check that SKILL.md is inside a named directory (not at the skills root) and the file name is exactly SKILL.md
If the wrong skill gets used, your descriptions are too similar — make them more distinct
For runtime errors, check dependencies, file permissions (chmod +x), and path separators (use forward slashes everywhere)

---

## 📌 Lesson-by-Lesson Insights

### What are skills?

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
What are skills?

---

### Creating your first skill

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Creating your first skill

---

### Configuration and multi-file skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Configuration and multi-file skills

---

### Skills vs. other Claude Code features

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Skills vs. other Claude Code features

---

### Sharing skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Sharing skills

---

### Troubleshooting skills

Introduction to agent skills
 What are skills?
 Creating your first skill
 Configuration and multi-file skills
 Skills vs. other Claude Code features
 Sharing skills
 Troubleshooting skills
Troubleshooting skills

---

## 🚀 How to Apply This in Real Projects

1. **Start small** — Pick one concept from each lesson and apply it immediately
2. **Iterate** — First attempts rarely perfect; refine based on results
3. **Document patterns** — Keep notes on what prompts/approaches work best for your use case
4. **Build on examples** — Use course examples as templates for your own work
5. **Connect concepts** — Look for how lessons build on each other to form complete workflows

---

*Tips extracted from course content at https://anthropic.skilljar.com/introduction-to-agent-skills*
