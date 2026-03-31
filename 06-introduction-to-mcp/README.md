# Introduction to Model Context Protocol — Course Notes

> Source: https://anthropic.skilljar.com/introduction-to-model-context-protocol
> Extracted: March 2026 (real text content from 12 lessons)

## Course Overview

This course introduces the **Model Context Protocol (MCP)** — a standardized communication layer that connects AI models like Claude to external tools, data, and services. The course covers MCP architecture, building an MCP server with the Python SDK, implementing an MCP client, and working with the three core MCP primitives: **Tools**, **Resources**, and **Prompts**. The hands-on project builds a CLI document management system backed by a custom MCP server.

---

## Lesson Notes

### Lesson 2: Introducing MCP

**Key Concepts:**
- MCP is a communication layer that provides Claude with context and tools without tedious integration code
- Shifts the burden of tool definitions and execution from your server to specialized MCP servers
- **Architecture:** MCP Client (your server) ↔ MCP Servers (contain tools, prompts, resources) ↔ External services
- Without MCP, you'd need to author, test, and maintain all integration code yourself (e.g., all of GitHub's API surface)
- MCP Servers wrap external services (like GitHub) and expose a standardized set of tools
- Anyone can create an MCP server; service providers often ship official ones (e.g., AWS)

**Key Distinctions:**
- **MCP vs. direct API calls:** MCP gives you pre-defined tool schemas and functions; calling APIs directly means you write all tool definitions yourself
- **MCP vs. tool use:** These are complementary, not the same. MCP defines *who implements* the tools (someone else); tool use is *how Claude calls* them

**Practical Tips:**
- Use MCP servers when integrating with well-known services to avoid reinventing integration code
- Look for official MCP server implementations from service providers before building your own

---

### Lesson 3: MCP Clients

**Key Concepts:**
- The MCP client is the communication bridge between your application and MCP servers
- Handles message exchange and protocol details so your app doesn't have to
- **Transport agnostic** — can communicate via stdio (most common, same machine), HTTP, WebSockets, and other protocols
- Key message types:
  - `ListToolsRequest` / `ListToolsResult` — discover what tools a server provides
  - `CallToolRequest` / `CallToolResult` — execute a tool and receive results

**Complete Request Flow (12 steps):**
1. User submits a query to your server
2. Your server needs to know available tools for Claude
3. Server asks the MCP client for tools
4. MCP client sends `ListToolsRequest` → receives `ListToolsResult`
5. Server sends user query + tools to Claude
6. Claude decides to call a tool
7. Server asks MCP client to execute the tool
8. MCP client sends `CallToolRequest` → MCP server calls external API
9. External API responds → data flows back as `CallToolResult`
10. Server sends tool results to Claude
11. Claude formulates final answer
12. Server delivers response to user

**Practical Tips:**
- Each component in the flow has a single, clear responsibility — keep it that way
- The MCP client abstracts server communication complexity; focus your app logic at a higher level
- Understanding this full flow is essential before building clients/servers

---

### Lesson 5: Defining Tools with MCP

**Key Concepts:**
- The official Python MCP SDK uses **decorators** for tool definition — no manual JSON schema writing
- Python type hints + Pydantic `Field` descriptions auto-generate proper schemas Claude can understand
- Server initialization is one line:
  ```python
  from mcp.server.fastmcp import FastMCP
  mcp = FastMCP("DocumentMCP", log_level="ERROR")
  ```
- Tool registration is automatic through decorators

**Code Pattern — Read Tool:**
```python
@mcp.tool(
    name="read_doc_contents",
    description="Read the contents of a document and return it as a string."
)
def read_document(
    doc_id: str = Field(description="Id of the document to read")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]
```

**Code Pattern — Edit Tool (find & replace):**
```python
@mcp.tool(
    name="edit_document",
    description="Edit a document by replacing a string in the document's content with a new string."
)
def edit_document(
    doc_id: str = Field(description="Id of the document that will be edited"),
    old_str: str = Field(description="The text to replace. Must match exactly, including whitespace."),
    new_str: str = Field(description="The new text to insert in place of the old text.")
):
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    docs[doc_id] = docs[doc_id].replace(old_str, new_str)
```

**Practical Tips:**
- Use `Field(description=...)` from Pydantic for every parameter — descriptions directly influence how well Claude uses your tools
- Raise `ValueError` for invalid inputs; Python exceptions integrate naturally with the SDK's error handling
- Type hints provide automatic validation — use them consistently

---

### Lesson 6: The Server Inspector

**Key Concepts:**
- The Python MCP SDK includes a **built-in browser-based inspector** for debugging and testing without a full application
- Launch with: `mcp dev mcp_server.py` → opens at `http://127.0.0.1:6274`
- Inspector maintains server state between tool calls (edits persist across tests)

**Using the Inspector:**
- Click **Connect** to initialize your server
- Navigate to **Tools** → click **List Tools** to see all available tools
- Select a tool, fill in parameters, click **Run Tool** to test
- Test multi-tool workflows to verify complex interactions (e.g., edit then read)

**Practical Tips:**
- Use the inspector as your primary development tool — faster than writing test scripts
- Test edge cases and error conditions (missing doc IDs, bad inputs) before connecting to a full app
- The inspector interface is actively updated — focus on core functionality (Connect, Tools, Resources, Prompts tabs)

---

### Lesson 8: Implementing a Client

**Key Concepts:**
- In most real projects you build *either* a client *or* a server — rarely both
- Two components: your custom **MCP Client class** (for ease of use) + the SDK's **Client Session** (actual connection)
- Client session requires careful resource management — wrap it in a class to handle cleanup automatically
- The client serves two primary purposes:
  1. Get list of available tools to send to Claude
  2. Execute tools when Claude requests them

**Core Client Functions:**
```python
# List all available tools from the MCP server
async def list_tools(self) -> list[types.Tool]:
    result = await self.session().list_tools()
    return result.tools

# Execute a specific tool
async def call_tool(
    self,
    tool_name: str,
    tool_input: dict
) -> types.CallToolResult | None:
    return await self.session().call_tool(tool_name, tool_input)
```

**Testing:**
```bash
uv run mcp_client.py   # verify client connects and lists tools
uv run main.py         # test full end-to-end flow
```

**Practical Tips:**
- Always wrap `ClientSession` in your own class to ensure proper connection cleanup
- Test the client standalone (`uv run mcp_client.py`) before wiring it into the full application
- The client is just a thin bridge — keep it simple; business logic belongs elsewhere

---

### Lesson 9: Defining Resources

**Key Concepts:**
- Resources expose **read-only data** from your server, analogous to GET handlers in HTTP
- Use them when you want to *fetch information* rather than *perform actions*
- Two resource types:
  - **Direct resources** — static URIs, no parameters
  - **Templated resources** — URI contains parameters, parsed and passed as kwargs

**Code Pattern — Direct Resource:**
```python
@mcp.resource(
    "docs://documents",
    mime_type="application/json"
)
def list_docs() -> list[str]:
    return list(docs.keys())
```

**Code Pattern — Templated Resource:**
```python
@mcp.resource(
    "docs://documents/{doc_id}",
    mime_type="text/plain"
)
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc with id {doc_id} not found")
    return docs[doc_id]
```

**MIME type guidance:**
- `"application/json"` — structured data
- `"text/plain"` — plain text
- `"application/pdf"` — binary files

**Practical Tips:**
- The SDK auto-serializes return values — just return Python objects, not JSON strings
- Use resources (not tools) when data is read-only and doesn't require Claude's decision to fetch
- Test via MCP Inspector: Resources tab for static, Resource Templates tab for parameterized
- Resources are ideal for UI features like autocomplete — fetch the list without a tool call round-trip

---

### Lesson 10: Accessing Resources

**Key Concepts:**
- Resource contents can be injected **directly into prompts**, bypassing the need for tool calls
- Enables immediate responses — data is part of the initial context, not fetched mid-conversation
- The client's `read_resource` function handles MIME-type-aware content parsing

**Implementation:**
```python
import json
from pydantic import AnyUrl

async def read_resource(self, uri: str) -> Any:
    result = await self.session().read_resource(AnyUrl(uri))
    resource = result.contents[0]
    if isinstance(resource, types.TextResourceContents):
        if resource.mimeType == "application/json":
            return json.loads(resource.text)
    return resource.text
```

**Response structure:** `result.contents[0]` contains the content with `.text` and `.mimeType` fields.

**Practical Tips:**
- Parse JSON resources into Python objects (not raw strings) so they're immediately usable
- Use `@mention` style UX patterns with resources — inject content before Claude even sees the message
- Resources deliver better UX than tool-based fetching when the user explicitly selects a document

---

### Lesson 11: Defining Prompts

**Key Concepts:**
- MCP prompts are **pre-built, tested instruction templates** that clients can use instead of writing prompts from scratch
- Users get better results from carefully crafted, evaluated prompts than ad-hoc requests
- Prompts use the same decorator pattern as tools and resources
- Return a list of `base.Message` objects (can include multiple user/assistant turns)

**Code Pattern:**
```python
@mcp.prompt(
    name="format",
    description="Rewrites the contents of the document in Markdown format."
)
def format_document(
    doc_id: str = Field(description="Id of the document to format")
) -> list[base.Message]:
    prompt = f"""
    Your goal is to reformat a document to be written with markdown syntax.
    The id of the document you need to reformat is: <document_id> {doc_id} </document_id>
    Add in headers, bullet points, tables, etc as necessary. Feel free to add in structure.
    Use the 'edit_document' tool to edit the document..."""
    return [base.UserMessage(prompt)]
```

**Practical Tips:**
- Encode domain expertise into prompts — users shouldn't need to be prompt engineering experts
- Test prompts in the MCP Inspector to verify variable interpolation before deploying
- Use XML-style tags (e.g., `<document_id>`) to clearly delineate injected variables in prompts
- Prompts exposed as slash commands (`/format`) create discoverable, consistent UX

---

### Lesson 12: Prompts in the Client

**Key Concepts:**
- Two client-side prompt functions: `list_prompts()` and `get_prompt()`
- `get_prompt` handles variable interpolation — pass arguments dict, get back formatted messages

**Implementation:**
```python
async def list_prompts(self) -> list[types.Prompt]:
    result = await self.session().list_prompts()
    return result.prompts

async def get_prompt(self, prompt_name, args: dict[str, str]):
    result = await self.session().get_prompt(prompt_name, args)
    return result.messages
```

**Workflow:**
1. Write and evaluate a prompt for your server's domain
2. Define it with `@mcp.prompt` decorator
3. Clients request it with `get_prompt(name, {"doc_id": "plan.md"})`
4. Arguments become keyword args in your prompt function
5. Returns formatted messages ready for the AI model

**Practical Tips:**
- Arguments dict keys must match your prompt function's parameter names exactly
- The returned messages go directly to Claude — no additional formatting needed
- Use slash commands (`/`) in CLI UIs to expose prompts as discoverable commands

---

### Lesson 14: MCP Review — Choosing the Right Primitive

**Key Concepts:**
The three MCP server primitives are each controlled by a different layer of your stack:

| Primitive | Controlled By | When to Use |
|-----------|--------------|-------------|
| **Tools** | The AI model (Claude) | Give Claude new capabilities to use autonomously |
| **Resources** | Your application code | Fetch data for UI elements or to augment prompt context |
| **Prompts** | The user | Pre-defined workflows triggered by user actions (slash commands, buttons) |

**Real-world examples (Claude's own interface):**
- Workflow buttons below chat input → **Prompts**
- "Add from Google Drive" → **Resources** (app controls what to show and inject)
- Claude running code / calculations → **Tools** (model decides when to call)

**Decision guide:**
- Need to give Claude new capabilities? → **Tools**
- Need to get data into your app for UI or context? → **Resources**
- Want predefined workflows users can trigger? → **Prompts**

**Practical Tips:**
- Don't default everything to tools — resources are more efficient for read-only data access
- Prompts belong on the server, not hardcoded in client UIs — centralizing them means one update improves all clients
- When designing a feature, ask "who triggers this?" — model → tool, app → resource, user → prompt

---

## Top 15 Practical Tips (Consolidated)

1. **Use MCP servers instead of DIY integrations** — for well-known services (GitHub, AWS, etc.), official MCP servers save significant implementation and maintenance work.

2. **Understand the 12-step request flow** — knowing how queries travel from user → app → MCP client → MCP server → external API → Claude → back is essential for debugging.

3. **Use Python decorators, not raw JSON schemas** — the FastMCP SDK's `@mcp.tool`, `@mcp.resource`, and `@mcp.prompt` decorators auto-generate correct schemas from type hints.

4. **Always add `Field(description=...)` to every parameter** — parameter descriptions directly influence how accurately Claude selects and uses your tools.

5. **Wrap `ClientSession` in your own class** — proper connection cleanup prevents resource leaks; don't use `ClientSession` directly in application code.

6. **Use the MCP Inspector early and often** (`mcp dev mcp_server.py`) — it's faster than writing test scripts and shows exactly what Claude receives.

7. **Test tools in sequence via the Inspector** — edit a document, then immediately read it back to verify state is maintained correctly.

8. **Choose resources over tools for read-only data** — resources bypass the tool-call round-trip and can be injected directly into prompts for faster responses.

9. **Set correct MIME types on resources** — `application/json`, `text/plain`, `application/pdf` etc. let clients automatically parse content correctly.

10. **Inject resources directly into prompts** — use `@mention`-style UX to pre-load document content before Claude sees the message, enabling immediate answers.

11. **Encode domain expertise into prompts** — invest time crafting and testing prompts on the server; users get expert-level results without needing to understand prompt engineering.

12. **Use XML-style tags in prompt templates** — clearly delimit injected variables (e.g., `<document_id>{doc_id}</document_id>`) to avoid ambiguity in prompts.

13. **Centralize prompts on the server** — update once, all clients improve; don't hardcode prompt logic in individual client applications.

14. **Apply the "who triggers this?" test** for primitive selection: model → Tool, application code → Resource, user action → Prompt.

15. **Use `uv` for Python environment management** — the course uses `uv run` for all commands; it handles virtual environments and dependencies cleanly for MCP projects.
