# Model Context Protocol — Advanced Topics - Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/model-context-protocol-advanced-topics

---

## 📋 Course at a Glance

Advanced MCP patterns including sampling, roots, elicitation, complex server architectures, debugging, testing, security considerations, and production-ready MCP deployment.

---

## 📌 Lesson-by-Lesson Insights

### Let's get started!

Video-only lesson. No additional text content available below the video player.

---

### Sampling

Sampling allows a server to access a language model like Claude through a connected MCP client. Instead of the server directly calling Claude, it asks the client to make the call on its behalf. This shifts the responsibility and cost of text generation from the server to the client.

---

### Sampling walkthrough

Let's get a better sense of how to implement this feature by walking through a sample project.

---

### Log and progress notifications

Logging and progress notifications are simple to implement but make a huge difference in user experience when working with MCP servers. They help users understand what's happening during long-running operations instead of wondering if something has broken.

---

### Notifications walkthrough

Tool functions automatically receive 'Context' as their last argument. This object has methods for logging and reporting progress to the client.

---

### Roots

Roots are a way to grant MCP servers access to specific files and folders on your local machine. Think of them as a permission system that says "Hey, MCP server, you can access these files" - but they do much more than just grant permission.

---

### Roots walkthrough

Ideally, a user will dictate which files/folders can be accessed by the MCP server.

---

### JSON message types

MCP (Model Context Protocol) uses JSON messages to handle communication between clients and servers. Understanding these message types is crucial for working with MCP, especially when dealing with different transport methods like the streamable HTTP transport.

---

## 🚀 How to Apply This in Real Projects

1. **Start small** — Pick one concept from each lesson and apply it immediately
2. **Iterate** — First attempts rarely perfect; refine based on results
3. **Document patterns** — Keep notes on what prompts/approaches work best for your use case
4. **Build on examples** — Use course examples as templates for your own work
5. **Connect concepts** — Look for how lessons build on each other to form complete workflows

---

*Tips extracted from course content at https://anthropic.skilljar.com/model-context-protocol-advanced-topics*
