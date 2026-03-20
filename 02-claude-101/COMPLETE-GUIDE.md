# Claude 101 - Complete Course Guide

**Source:** https://anthropic.skilljar.com/claude-101  
**Repository:** https://github.com/eng-elias-owis/anthropic-courses-notes  
**Extracted:** March 20, 2026  
**Status:** ✅ Enrolled & Curriculum Extracted

---

## Course Overview

Claude 101 teaches you how to use Claude effectively for everyday work tasks. This guide contains the course structure and practical tips based on the actual curriculum.

**Total Lessons:** 15 (14 lessons + conclusion)  
**Duration:** ~2-3 hours  
**Level:** Beginner  
**Status:** 0 of 14 lessons completed (0%)

---

## Curriculum Structure

### **Section 1: Meet Claude**

#### Lesson 1: What is Claude?
**Topics:**
- Introduction to Claude AI assistant
- Core capabilities and features
- What makes Claude different
- When to use Claude vs other tools

**Practical Application:**
- Start using Claude for daily tasks
- Understand Claude's strengths (analysis, writing, coding, research)
- Know Claude's limitations (no internet, knowledge cutoff)

---

#### Lesson 2: Your first conversation with Claude
**Topics:**
- How to start a conversation
- Basic prompt structure
- Interface navigation
- Following up in the same conversation

**Practical Tips:**
1. **Start with context:** "I'm a software engineer working on..."
2. **Be specific:** Clear requests get better results
3. **Iterate:** Use follow-ups to refine
4. **Provide examples:** Show what good looks like

---

#### Lesson 3: Getting better results
**Topics:**
- Writing effective prompts
- The 5 components of a great prompt
- Using examples (few-shot prompting)
- Iterative refinement

**The 5 Components:**
1. **Role:** Who should Claude be? ("Act as a senior developer...")
2. **Task:** What exactly do you want?
3. **Format:** How should output look? (table, bullets, code)
4. **Constraints:** Length, tone, style requirements
5. **Examples:** Show what you want

**Key Techniques:**
- Chain of thought: "Think step by step"
- Delimiters: Use ``` for code/data separation
- Feedback loops: Build on responses

---

#### Lesson 4: Claude desktop app: Chat, Cowork, Code
**Topics:**
- Installing the desktop app
- Keyboard shortcuts and features
- Working with files
- Integration with workflow

**Practical Tips:**
- Download from claude.ai/download
- Use Cmd/Ctrl + K for quick actions
- Drag and drop files for analysis
- Works with images, PDFs, code files

---

### **Section 2: Organizing Your Work**

#### Lesson 5: Introduction to projects
**Topics:**
- Creating projects for ongoing work
- Setting custom instructions
- Uploading relevant files
- Organizing conversations

**Practical Application:**
- Create a project for each major work area
- Set instructions: "Always provide Python with type hints"
- Upload architecture docs, specs, examples
- Use for team collaboration

---

#### Lesson 6: Creating with artifacts
**Topics:**
- What are artifacts?
- When Claude creates artifacts
- Editing artifacts collaboratively
- Version history

**Practical Tips:**
- Artifacts = substantial content (docs, code, data)
- Click to edit in separate pane
- Use for content needing refinement
- Version history available

---

#### Lesson 7: Working with skills
**Topics:**
- What are skills?
- Creating custom skills
- Using built-in skills
- Sharing with team

**Practical Application:**
- Create reusable instructions
- Skills auto-apply when relevant
- Example: "Review Python code for security, performance, style"
- Share with team for consistency

---

### **Section 3: Expanding Claude's Reach**

#### Lesson 8: Connecting your tools
**Topics:**
- MCP (Model Context Protocol)
- Connecting external services
- Using tools with Claude
- Building custom integrations

**Practical Tips:**
- Connect GitHub, Jira, databases
- Extend Claude's capabilities
- Build custom MCP servers
- Tools go beyond training data

---

#### Lesson 9: Enterprise search
**Topics:**
- Connecting company knowledge bases
- Searching internal documents
- Integration with enterprise systems

**Practical Application:**
- Connect company wiki/Confluence
- Query internal documentation
- Great for onboarding

---

#### Lesson 10: Research mode for deep dives
**Topics:**
- Using research mode
- Deep analysis capabilities
- Extended thinking
- Complex problem solving

**When to Use:**
- Complex analysis tasks
- Architecture decisions
- Research synthesis
- Strategic planning

---

### **Section 4: Putting It All Together**

#### Lesson 11: Putting it all together
**Topics:**
- Combining all features
- Advanced workflows
- Best practices summary

---

#### Lesson 12: Claude in action — use-cases by role
**Topics:**
- Developer workflows
- Writer workflows
- Analyst workflows
- Manager workflows

**For Developers (You!):**
- Generate boilerplate code
- Debug with stack traces
- Refactor for performance
- Write tests
- Explain legacy code

---

#### Lesson 13: Other ways to work with Claude
**Topics:**
- Claude API
- Claude Code CLI
- Integrations
- Mobile access

---

### **Section 5: Conclusion**

#### Lesson 14: What's next?
**Topics:**
- Next steps after Claude 101
- Recommended courses
- Advanced topics

---

#### Conclusion & Certificate
- Course summary
- Key takeaways
- Certificate of completion

---

## 🛠️ 20 Practical Tips from Claude 101

### **Prompting Basics (1-5)**

**1. Start with Context**
```
❌ "Help me write an email"
✅ "I'm a software engineer at a startup. I need to write an email to my team about delaying the release by one week due to a critical bug. Keep it professional but empathetic."
```

**2. Be Specific About Format**
```
❌ "Tell me about Python"
✅ "Explain Python list comprehensions in 3 bullet points, then show 2 practical code examples."
```

**3. The "Act As" Pattern**
```
"Act as a senior code reviewer. Review this function for:
1. Bugs or edge cases
2. Performance issues
3. Readability improvements

Provide feedback in this format:
- [Issue]: [Explanation]
- [Suggestion]: [Code example]"
```

**4. Chain of Thought**
```
"Solve this step by step, explaining your reasoning at each step: [complex problem]"
```

**5. Use Delimiters**
```
Analyze this code:

```python
def calculate_metrics(data):
    return {k: sum(v)/len(v) for k, v in data.items()}
```

Focus on: security, error handling, edge cases.
```

---

### **Coding & Development (6-10)**

**6. Generate Boilerplate**
```
"Create a Python FastAPI endpoint that:
1. Accepts POST requests with JSON body
2. Validates input using Pydantic
3. Returns proper HTTP status codes
4. Includes error handling"
```

**7. Debug with Context**
```
"I'm getting this error: [error]

Here's my code:
```python
[code]
```

Explain:
1. What this error means
2. Why it's happening
3. How to fix it
4. How to prevent it"
```

**8. Refactor Code**
```
"Refactor this function to be more efficient:
- Keep same functionality
- Improve time complexity
- Add type hints
- Include docstring"
```

**9. Write Tests**
```
"Write unit tests for this function:
- Happy path tests
- Edge cases
- Error conditions
- Use pytest"
```

**10. Explain Legacy Code**
```
"Explain what this code does in plain English.
Then explain:
- Business logic
- Potential issues
- How to modernize"
```

---

### **Projects & Organization (11-15)**

**11. Create Project Structure**
- One project per major work area
- Upload relevant files
- Set custom instructions

**12. Project Instructions Template**
```
Project: GRIT Development

Instructions:
- Always provide Go code with error handling
- Include comments explaining 'why' not just 'what'
- Suggest tests for new functions
- Check for: security, performance, maintainability
```

**13. Use Artifacts**
- For substantial content
- Click to edit
- Version history available

**14. Build Skills**
```
Skill: "Python Code Review"

Instructions:
"When I share Python code, review for:
1. Security vulnerabilities
2. Performance issues
3. Code smells
4. Pythonic patterns
5. Missing error handling"
```

**15. Use MCP Tools**
- Connect external services
- Extend capabilities
- Build custom integrations

---

### **Best Practices (16-20)**

**16. The Feedback Loop**
```
You: "Write a function to parse JSON"
Claude: [code]
You: "Add error handling"
Claude: [updated code]
You: "Add type hints"
Claude: [final version]
```

**17. Save Good Prompts**
- Keep a library
- Reuse and adapt
- Share with team
- Refine over time

**18. Verify Critical Info**
- Double-check facts
- Be aware of knowledge cutoff
- Ask for sources when needed

**19. Iterate, Don't Perfect**
- First response is a draft
- Build with follow-ups
- Refine progressively

**20. Use Projects for Context**
- Upload architecture docs
- Keep specs handy
- Maintain knowledge base

---

## 🔗 Apply to Your Projects

### **CIA (Change Impact Analyzer)**
- **Lessons 1-4:** Use Claude for code analysis
- **Lessons 5-7:** Create CIA project, build code review skills
- **Lessons 8-10:** Integrate with GitHub MCP for PR analysis

### **Scratchers App**
- **Lessons 1-4:** Generate UI code, debug errors
- **Lessons 5-7:** Create project, generate artifacts
- **Lessons 8-10:** Build deployment automation

### **GRIT (Git Intelligence)**
- **Lessons 1-4:** Architecture planning, API design
- **Lessons 5-7:** Create GRIT project with all specs
- **Lessons 8-10:** Research mode for complex analysis
- **Lessons 11-14:** Combine all features for SaaS

---

## 📋 Quick Reference

### **The Perfect Prompt Template:**
```
Role: [who Claude should be]
Task: [what you want]
Format: [how output should look]
Constraints: [limitations/requirements]
Examples: [show good output]
```

### **Daily Workflow:**
1. Create/use appropriate project
2. Provide context in first message
3. Be specific about what you want
4. Iterate with follow-ups
5. Save good prompts for reuse

---

## ✅ Course Completion Checklist

**Section 1: Meet Claude**
- [ ] Lesson 1: What is Claude?
- [ ] Lesson 2: Your first conversation
- [ ] Lesson 3: Getting better results
- [ ] Lesson 4: Desktop app

**Section 2: Organizing Work**
- [ ] Lesson 5: Introduction to projects
- [ ] Lesson 6: Creating with artifacts
- [ ] Lesson 7: Working with skills

**Section 3: Expanding Reach**
- [ ] Lesson 8: Connecting tools
- [ ] Lesson 9: Enterprise search
- [ ] Lesson 10: Research mode

**Section 4: Putting It Together**
- [ ] Lesson 11: Putting it all together
- [ ] Lesson 12: Use-cases by role
- [ ] Lesson 13: Other ways to work

**Section 5: Conclusion**
- [ ] Lesson 14: What's next?
- [ ] Conclusion & certificate

---

## 📚 Next Courses

After completing Claude 101:
1. **AI Fluency: Framework & Foundations** — Deepen AI collaboration skills
2. **Claude Code in Action** — Developer-specific workflows
3. **Building with the Claude API** — Integration and automation

---

**Extracted from:** Anthropic Skilljar — Claude 101  
**Date:** March 20, 2026  
**Status:** Enrolled, curriculum extracted, ready to complete  
**Repository:** https://github.com/eng-elias-owis/anthropic-courses-notes/tree/main/02-claude-101

---

*Part of Anthropic Courses Notes — Complete AI Learning Path*
