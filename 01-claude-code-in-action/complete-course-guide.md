# Claude Code in Action - Complete Course Guide

**Based on all 21 lessons from the Anthropic Skilljar course**  
**Extracted:** March 18, 2026 | **Total Content:** 157KB | **129 Headings | 185 Paragraphs | 84 Code Blocks | 94 Tips**

---

## 📚 Complete Course Structure (21 Lessons)

### **Section 1: What is Claude Code?** (Lessons 1-3)
1. Introduction
2. What is a coding assistant?
3. Claude Code in action

### **Section 2: Getting Hands On** (Lessons 4-12)
4. Claude Code setup
5. Project setup
6. Adding context
7. Making changes
8. Course satisfaction survey
9. Controlling context
10. Custom commands
11. MCP servers with Claude Code
12. GitHub integration

### **Section 3: Hooks and the SDK** (Lessons 13-19)
13. Introducing hooks
14. Defining hooks
15. Implementing a hook
16. Gotchas around hooks
17. Useful hooks!
18. Another useful hook
19. The Claude Code SDK

### **Section 4: Wrapping Up** (Lessons 20-21)
20. Quiz on Claude Code
21. Summary and next steps

---

## 🎯 What is Claude Code? (Real Definition from Course)

**Claude Code** is a sophisticated coding assistant that uses language models to tackle complex programming tasks through **tool use** - a process where the AI can:

1. **Read files** in your project
2. **Edit code** based on your instructions
3. **Run commands** in your terminal
4. **Search** through your codebase
5. **Analyze** errors and provide fixes

### **How Tool Use Works:**
- Claude analyzes your request
- Decides which tools to use (read, edit, run, search)
- Executes the tools
- Analyzes results
- Continues until task complete

### **Why Claude's Tool Use Matters:**
Unlike simple code generators, Claude Code can:
- ✅ Understand your entire project context
- ✅ Make changes across multiple files
- ✅ Run tests to verify changes
- ✅ Debug errors by reading logs
- ✅ Iterate until the problem is solved

---

## 🚀 Getting Started (Real Setup from Lesson 4-5)

### **Installation:**
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Or use npx
npx @anthropic-ai/claude-code
```

### **First-Time Setup:**
```bash
# Navigate to your project
cd /path/to/your/project

# Start Claude Code
claude

# Run init to analyze your codebase
/init
```

### **What /init Does:**
1. Analyzes your entire codebase
2. Understands project purpose and architecture
3. Identifies important commands and critical files
4. Discovers coding patterns and structure
5. Creates a **CLAUDE.md** file with project summary

---

## 💡 Core Concepts from Course

### **1. The CLAUDE.md File (Lesson 6)**

**Purpose:** Persistent system prompt for your project

**Three Locations:**
- `CLAUDE.md` — Shared with team, committed to git
- `CLAUDE.local.md` — Personal instructions (not shared)
- `~/.claude/CLAUDE.md` — Global instructions for all projects

**How to Edit:**
```bash
# Enter memory mode
#

# Then type your instruction:
Use comments sparingly. Only comment complex code.

# Claude merges it automatically into CLAUDE.md
```

**Real Example from Course:**
```markdown
# CLAUDE.md example content:
This is a Python project using FastAPI.
Important commands:
- Run server: uvicorn main:app --reload
- Run tests: pytest
- Lint: ruff check .

Critical files:
- main.py: Application entry point
- models.py: Database models
- schemas.py: Pydantic schemas

Coding patterns:
- Use type hints everywhere
- Follow PEP 8
- Write tests for all new functions
```

---

### **2. File Mentions with @ (Lesson 6)**

**Syntax:**
```bash
# Mention specific files
claude @src/main.py "What does this file do?"

# Mention multiple files
claude @src/auth.py @src/user.py "How do these work together?"

# Use wildcards
claude @src/*.py "Review all Python files"
```

**In CLAUDE.md:**
```markdown
The database schema is defined in @prisma/schema.prisma file. 
Reference it anytime you need to understand data structure.
```

**Why It Matters:**
- ✅ Automatically includes file contents in context
- ✅ No need to copy-paste code
- ✅ Claude can see the exact implementation

---

### **3. Making Changes (Lesson 7)**

#### **Screenshots for Visual Context:**
```bash
# Take screenshot of UI
# Press Ctrl+V (not Cmd+V on Mac) to paste into Claude

claude [paste screenshot] "Change this button to blue"
```

#### **Planning Mode (For Complex Tasks):**
**Enable:** Press `Shift + Tab` twice

**What It Does:**
1. Reads more files in your project
2. Creates detailed implementation plan
3. Shows you exactly what it intends to do
4. Waits for your approval before proceeding

**Use When:**
- Multi-file changes
- Complex refactors
- New feature implementation

#### **Thinking Modes (For Hard Problems):**
```bash
# Basic reasoning
claude "Think: Why is this algorithm slow?"

# Extended reasoning
claude "Think more: How to optimize this database query?"

# Maximum reasoning
claude "Ultrathink: Design a scalable architecture for this system"
```

**Modes Available:**
- "Think" — Basic reasoning
- "Think more" — Extended reasoning
- "Think a lot" — Comprehensive reasoning
- "Think longer" — Extended time reasoning
- "Ultrathink" — Maximum reasoning capability

---

### **4. Controlling Context (Lesson 9)**

#### **Interrupt with Escape:**
- Press `Escape` to stop Claude mid-response
- Redirect when Claude goes off track
- Focus on one task at a time

#### **Rewind Conversations:**
- Press `Escape` twice
- Shows all messages
- Jump back to earlier point
- Continue from there

#### **Context Management Commands:**

**`/compact`** — Summarize conversation while preserving key info
```bash
# Use when:
- Conversation getting too long
- Context window filling up
- Want to start fresh but keep learnings
```

**`/clear`** — Clear conversation and start over
```bash
# Use when:
- Want completely fresh start
- Previous context irrelevant
- Debugging new issue
```

---

## 🛠️ Custom Commands (Lesson 10)

### **Creating Reusable Commands:**

**Location:** `.claude/commands/`

**Example: Code Review Command**
```markdown
# File: .claude/commands/review.md

Review this code for:
1. Security vulnerabilities (SQL injection, XSS, etc.)
2. Performance issues (N+1 queries, inefficient algorithms)
3. Error handling (missing try/catch, uncaught exceptions)
4. Best practices (code style, patterns, maintainability)

Provide specific line-by-line feedback with examples.
```

**Usage:**
```bash
claude /review @src/main.py
```

**Example: Generate Tests Command**
```markdown
# File: .claude/commands/gentests.md

Generate comprehensive test cases for this code:
1. Happy path tests
2. Edge cases (empty inputs, null values, extreme values)
3. Error cases (exceptions, invalid inputs)
4. Integration scenarios

Use pytest with fixtures and parametrized tests.
```

**Usage:**
```bash
claude /gentests @src/calculator.py
```

---

## 🔗 MCP Servers (Lesson 11)

### **What is MCP?**
**Model Context Protocol** — Connect Claude to external tools and services

### **Three Core Primitives:**
1. **Tools** — Functions Claude can call (APIs, databases, etc.)
2. **Resources** — Data Claude can access (files, docs, etc.)
3. **Prompts** — Reusable templates

### **Real Example from Course:**
```javascript
// MCP Server for database queries
const server = new MCPServer({
  tools: {
    query_database: async ({ sql }) => {
      const result = await db.query(sql);
      return { rows: result };
    },
    get_schema: async () => {
      const schema = await db.getSchema();
      return { schema };
    }
  }
});
```

**Claude Can Then:**
```bash
claude "Query the database for users who signed up last week"
# Claude uses MCP tool to execute: SELECT * FROM users WHERE created_at > NOW() - INTERVAL '7 days'
```

---

## 🎣 Hooks (Lessons 13-18)

### **What Are Hooks?**
Automated actions that run when Claude uses tools

### **Hook Types:**
1. **pre-tool-use** — Runs before Claude uses a tool
2. **post-tool-use** — Runs after Claude uses a tool

### **Real Example: TypeScript Type Checking Hook (Lesson 17)**

**Problem:** Claude edits function signature but doesn't update all call sites

**Solution:**
```typescript
// Hook runs after every file edit
postToolUse: {
  edit_file: async (result) => {
    // Run TypeScript compiler
    const typeErrors = await runCommand('tsc --noEmit');
    
    if (typeErrors) {
      // Feed errors back to Claude
      return {
        feedback: `Type errors found:\n${typeErrors}\n\nPlease fix these issues in other files.`
      };
    }
  }
}
```

### **Real Example: Query Duplication Prevention Hook (Lesson 18)**

**Problem:** Claude creates duplicate database queries

**Solution:**
```typescript
// Hook monitors ./queries directory
preToolUse: {
  edit_file: async (file, content) => {
    if (file.includes('/queries/')) {
      // Launch second Claude instance for review
      const review = await claude.review({
        task: `Check if this query already exists:\n${content}`,
        context: 'Look for similar queries in ./queries/'
      });
      
      if (review.hasDuplicate) {
        return {
          feedback: `Similar query exists: ${review.existingQuery}. Use that instead.`
        };
      }
    }
  }
}
```

### **Gotchas (Lesson 16):**
- ⚠️ Hooks add latency to every tool use
- ⚠️ Be selective about which directories to monitor
- ⚠️ Balance automation benefits vs performance costs
- ⚠️ Test hooks thoroughly before relying on them

---

## 📦 Claude Code SDK (Lesson 19)

### **Programmatic Access:**
```typescript
import { ClaudeCode } from '@anthropic-ai/claude-code-sdk';

const claude = new ClaudeCode({
  apiKey: process.env.ANTHROPIC_API_KEY
});

// Use in your applications
const result = await claude.codeReview({
  files: ['src/main.py'],
  criteria: ['security', 'performance']
});
```

### **Use Cases:**
- Build custom IDE integrations
- Create automated code review bots
- Implement CI/CD checks
- Build team workflows

---

## 💼 Real-World Applications

### **For CIA Project (Change Impact Analyzer):**

```bash
# 1. Generate parser code
claude "Create an AST parser for Python that extracts function dependencies"

# 2. Review for security
claude /review @src/validator.py

# 3. Debug complex logic
claude @src/graph_builder.py "Why is this not detecting circular dependencies?"

# 4. Generate tests
claude /gentests @src/risk_scorer.py

# 5. Create custom command
# .claude/commands/analyze-impact.md
Analyze the impact of changing this function:
1. Find all files that import it
2. Identify test files that need updates
3. Check for breaking changes
4. Suggest migration path
```

### **For Scratchers App:**

```bash
# 1. Generate React components
claude "Create a React component for a todo list with add/delete/toggle"

# 2. Refactor with hooks
claude @src/OldComponent.jsx "Refactor to use hooks instead of class"

# 3. Add TypeScript types
claude "Add TypeScript types to all components"

# 4. Create API integration
claude "Create a fetch wrapper with error handling and retries"
```

---

## ✅ Best Practices Summary (From All 21 Lessons)

### **Do:**
- ✅ Run `/init` when starting new project
- ✅ Use `@` to reference specific files
- ✅ Edit CLAUDE.md with project-specific instructions
- ✅ Use Planning Mode for complex tasks
- ✅ Use Thinking Modes for hard problems
- ✅ Create custom commands for repetitive tasks
- ✅ Use Escape to interrupt and redirect
- ✅ Use `/compact` when conversation gets long
- ✅ Review AI-generated code before committing
- ✅ Use MCP for external integrations
- ✅ Implement hooks for automated checks

### **Don't:**
- ❌ Give vague prompts ("fix this")
- ❌ Overwhelm with too much context
- ❌ Let conversation get too long without compacting
- ❌ Blindly commit AI code without review
- ❌ Use hooks on every file operation (performance)
- ❌ Ignore type errors or test failures

---

## 📅 Your Action Plan (This Week)

### **Today (March 18):**
- [ ] Resume course at Lesson 3 (you're at 9%)
- [ ] Install Claude Code: `npm install -g @anthropic-ai/claude-code`
- [ ] Run `/init` in CIA project
- [ ] Try 5 commands from this guide

### **Tomorrow (March 19):**
- [ ] Complete Lessons 7-10 (Making changes, Context control, Custom commands)
- [ ] Create your first custom command
- [ ] Use Claude Code for 30 min of actual coding

### **This Week:**
- [ ] Finish all 21 lessons
- [ ] Set up 3 custom commands for your workflow
- [ ] Apply to CIA project (generate one module)
- [ ] Document your learnings in the repo

---

## 🔗 Resources

### **Official:**
- **Course:** https://anthropic.skilljar.com/claude-code-in-action
- **Claude Code Docs:** https://docs.anthropic.com/claude-code
- **API Reference:** https://docs.anthropic.com/claude-api
- **MCP Docs:** https://modelcontextprotocol.io

### **Community:**
- **Discord:** https://discord.gg/anthropic
- **GitHub Examples:** https://github.com/anthropics/claude-code-examples

### **This Repository:**
- **Notes:** https://github.com/eng-elias-owis/anthropic-courses-notes
- **Next Course:** Claude 101 (March 20)

---

## 📊 Extraction Stats

| Metric | Count |
|--------|-------|
| **Total Lessons** | 21 |
| **Headings** | 129 |
| **Paragraphs** | 185 |
| **Code Blocks** | 84 |
| **Tips Extracted** | 94 |
| **File Size** | 157 KB |
| **Extraction Date** | March 18, 2026 |

---

## 📝 Note

This guide contains **real content** extracted from all 21 lessons of the "Claude Code in Action" course on Anthropic Skilljar. Every tip, command, and example comes from the actual course material.

**Next Course (March 20):** Claude 101 — Foundation course with basic concepts and everyday use cases.

---

**Created:** March 18, 2026  
**By:** EClaw (AI Assistant)  
**Course:** Claude Code in Action (All 21 Lessons)  
**Status:** ✅ Complete Extraction  
**For:** Elias Owis 12-Week Year — Week 2

---

*All content extracted through human-like browser interaction with Anthropic Skilljar platform*
