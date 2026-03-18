# Claude Code in Action - Practical Tips & Tricks

**Real-world techniques, commands, and workflows extracted from the course**

---

## 🚀 Quick Start Commands (Use These Daily)

### **1. Opening Claude Code**
```bash
# In your project directory
claude

# Or with a specific prompt
claude "Review the codebase for security issues"
```

### **2. Context-Aware Questions**
```bash
# Ask about specific files using @
claude @src/main.py "What does this function do?"

# Ask about multiple files
claude @src/parser.py @src/analyzer.py "How do these work together?"

# Ask about the whole project
claude "Explain the architecture of this project"
```

### **3. Code Generation**
```bash
# Generate new code
claude "Create a Python function to parse YAML config files"

# With specific requirements
claude "Write a test suite for the RiskScorer class using pytest"

# With style constraints
claude "Generate a React component for a todo list using TypeScript and Tailwind"
```

---

## 💡 Pro Tips from the Course

### **Tip 1: Use Natural Language, Not Code Speak**
❌ Bad: "Implement function foo(bar)"  
✅ Good: "Create a function that takes a risk score and returns a human-readable explanation"

**Why:** Claude understands intent better than technical jargon.

---

### **Tip 2: Provide Context Gradually**
1. Start with high-level: "What does this project do?"
2. Then dive deeper: "How does the risk scoring work?"
3. Finally specifics: "Refactor the calculate_risk function to handle edge cases"

**Why:** Claude builds context as you go, giving better answers.

---

### **Tip 3: Use the @ Symbol Liberally**
```bash
# Compare two implementations
claude @src/old_version.py @src/new_version.py "Which is better and why?"

# Debug with context
claude @src/main.py @tests/test_main.py "Why is this test failing?"

# Review before commit
claude @src/ "Review all changes for bugs"
```

**Why:** @ gives Claude exact context, reducing hallucinations.

---

### **Tip 4: Iterate on Answers**
1. First pass: "Generate a function to validate email addresses"
2. Refine: "Make it handle international emails too"
3. Polish: "Add type hints and docstrings"
4. Test: "Write unit tests for this function"

**Why:** One-shot perfection is rare. Iteration gets better results.

---

### **Tip 5: Ask Claude to Explain Its Own Code**
```bash
# After getting code, ask for explanation
claude "Explain how this regex pattern works"
claude "What are the edge cases in this function?"
claude "How would you improve this code's performance?"
```

**Why:** Understanding > copy-pasting. You'll catch bugs and learn.

---

## 🛠️ Advanced Techniques

### **Technique 1: Custom Commands (Saves Hours)**

Create reusable commands in `.claude/commands/`:

```markdown
# .claude/commands/review.md
Review this code for:
1. Security vulnerabilities
2. Performance issues  
3. Best practice violations
4. Missing error handling

Provide specific line-by-line feedback.
```

**Usage:**
```bash
claude /review @src/main.py
```

---

### **Technique 2: MCP Server Integration**

Connect external tools to Claude Code:

```javascript
// mcp-server.js - Custom tool for your workflow
const server = new MCPServer({
  tools: {
    analyze_code: async ({ filepath }) => {
      // Run your custom analysis
      return { complexity: calculateComplexity(filepath) };
    }
  }
});
```

**Benefits:**
- Connect to your CI/CD pipeline
- Query your database directly
- Run custom linters/checks
- Integrate with Jira/GitHub

---

### **Technique 3: Hooks for Automation**

Create `.claude/hooks/pre-commit`:
```bash
#!/bin/bash
# Auto-review before commits
claude /review --staged-files
```

**Use Cases:**
- Pre-commit code review
- Auto-generate commit messages
- Run tests on changed files only
- Update documentation

---

### **Technique 4: Context Control**

When working with large projects:

```bash
# Limit context to specific directories
claude --context src/core "Refactor the error handling"

# Exclude certain files
claude --ignore "*.test.js,*.md" "Update the API"

# Focus on recent changes
claude --since "2 hours ago" "Review recent commits"
```

**Why:** Large codebases overwhelm context windows. Be selective.

---

## 🔧 Debugging Workflow

### **Step-by-Step Debugging with Claude:**

1. **Describe the error:**
   ```bash
   claude "I'm getting 'KeyError: risk_score' in line 42. What's wrong?"
   ```

2. **Show the context:**
   ```bash
   claude @src/risk_analyzer.py "Explain why this KeyError happens"
   ```

3. **Get a fix:**
   ```bash
   claude "Fix this KeyError and handle the missing key gracefully"
   ```

4. **Verify the fix:**
   ```bash
   claude "Write a test case for this edge case"
   ```

---

## 📋 Real-World Examples

### **Example 1: Code Review (CIA Project)**
```bash
# Review a PR
claude @src/ "Review all changes in this PR for:
1. Security issues with user input
2. SQL injection risks
3. Proper error handling
4. Test coverage gaps"

# Get specific feedback
claude @src/validator.py "Is this validation logic comprehensive enough?"
```

---

### **Example 2: Refactoring (Scratchers App)**
```bash
# Understand first
claude @src/old_component.jsx "Explain what this component does"

# Plan the refactor
claude "How would you refactor this to use React hooks instead of classes?"

# Execute
claude "Refactor this to functional components with hooks"

# Add tests
claude "Write tests for this refactored component"
```

---

### **Example 3: Documentation**
```bash
# Generate API docs
claude @src/api.py "Generate OpenAPI/Swagger documentation for these endpoints"

# Create README
claude "Write a comprehensive README for this project"

# Add inline docs
claude @src/complex_module.py "Add comprehensive docstrings to all functions"
```

---

## ⚠️ Common Mistakes to Avoid

### **❌ Mistake 1: Vague Prompts**
```bash
# Bad
claude "Fix this"
claude "Make it better"

# Good
claude "Fix the NullPointerException in line 15 by adding a null check"
claude "Optimize this function to reduce time complexity from O(n²) to O(n)"
```

---

### **❌ Mistake 2: Too Much Context**
```bash
# Bad - overwhelming
claude @src/ @tests/ @docs/ config/ "Fix everything"

# Good - focused
claude @src/auth.py "Fix the login timeout issue"
```

---

### **❌ Mistake 3: Not Reviewing AI Output**

**Always:**
- Read generated code before committing
- Run tests on AI-generated code
- Verify security implications
- Check for hallucinated APIs/functions

---

## 🎯 Best Practices Checklist

- [ ] **Start broad, then narrow** — Get overview before diving deep
- [ ] **Use @ for precision** — Reference specific files/functions
- [ ] **Iterate** — Don't expect perfection on first try
- [ ] **Verify** — Always review AI-generated code
- [ ] **Test** — Run tests on any AI-modified code
- [ ] **Document** — Ask Claude to explain its changes
- [ ] **Custom commands** — Build reusable workflows
- [ ] **Hooks** — Automate repetitive tasks
- [ ] **MCP** — Connect your tools
- [ ] **Context control** — Manage large projects wisely

---

## 🔗 Integration Cheat Sheet

### **VS Code:**
```bash
# Install extension
# Cmd+Shift+P → "Claude: Open Chat"
# Or use inline: select code → right click → "Ask Claude"
```

### **JetBrains (IntelliJ, PyCharm):**
```bash
# Install plugin
# Tools → Claude Code
```

### **Terminal:**
```bash
# Any directory
claude

# With options
claude --context src/ --ignore "*.test.js"
```

---

## 📚 Resources from Course

### **Official Documentation:**
- **Claude Code Docs:** https://docs.anthropic.com/claude-code
- **API Reference:** https://docs.anthropic.com/claude-api
- **MCP Docs:** https://modelcontextprotocol.io

### **Community:**
- **Discord:** https://discord.gg/anthropic
- **GitHub Examples:** https://github.com/anthropics/claude-code-examples

### **Your Projects:**
- **CIA Project:** Use for parser generation, code review, debugging
- **Scratchers App:** Use for component generation, testing, docs

---

## 🚀 Action Plan for This Week

### **Today:**
- [ ] Install Claude Code in your IDE
- [ ] Try 5 basic commands from this guide
- [ ] Complete Lesson 3-4 in the course

### **Tomorrow:**
- [ ] Use Claude Code for 30 min of actual coding
- [ ] Create your first custom command
- [ ] Complete Lesson 5-7

### **This Week:**
- [ ] Finish "Getting Hands On" section
- [ ] Apply to CIA project (generate one module)
- [ ] Set up 2-3 custom commands

---

**These tips are based on the Claude Code in Action course curriculum and real-world best practices. For complete learning, finish the interactive lessons at:** https://anthropic.skilljar.com/claude-code-in-action

---

*Next practical guide: Claude 101 (March 20)* 🦅
