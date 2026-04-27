# MCP: Advanced Topics — Course Notes

> Source: https://anthropic.skilljar.com/model-context-protocol-advanced-topics
> Extracted: April 2026 (real text content from 13 lessons)

---

## Course Overview

This course covers advanced MCP (Model Context Protocol) features beyond the basics: **Sampling** (letting servers delegate LLM calls to clients), **Notifications** (logging and progress), **Roots** (file-system permission scoping), and **Transports** (STDIO vs StreamableHTTP with stateful/stateless modes). It includes implementation walkthroughs and downloadable project files.

---

## Lesson Notes

### Lesson: Sampling

**Key Concepts:**
- Sampling allows an MCP server to request text generation from a language model *through* the connected client — the server never calls Claude directly
- The server doesn't need its own API key or billing; the client pays for token usage
- Ideal for public/shared MCP servers where you don't want arbitrary users running up AI costs on the server side

**How It Works (flow):**
1. Server completes its work (e.g. fetches Wikipedia articles)
2. Server constructs a prompt and calls `ctx.session.create_message()`
3. Client receives the request, calls Claude via its own Anthropic SDK connection
4. Client returns a `CreateMessageResult` to the server
5. Server uses the generated text in its response

**Code Patterns:**

*Server side — request sampling inside a tool:*
```python
@mcp.tool()
async def summarize(text_to_summarize: str, ctx: Context):
    prompt = f"""
    Please summarize the following text:
    {text_to_summarize}
    """
    result = await ctx.session.create_message(
        messages=[
            SamplingMessage(
                role="user",
                content=TextContent(type="text", text=prompt)
            )
        ],
        max_tokens=4000,
        system_prompt="You are a helpful research assistant",
    )
    if result.content.type == "text":
        return result.content.text
    else:
        raise ValueError("Sampling failed")
```

*Client side — implement and register a sampling callback:*
```python
async def sampling_callback(
    context: RequestContext, params: CreateMessageRequestParams
):
    text = await chat(params.messages)  # call Claude via Anthropic SDK
    return CreateMessageResult(
        role="assistant",
        model=model,
        content=TextContent(type="text", text=text),
    )

async with ClientSession(read, write, sampling_callback=sampling_callback) as session:
    await session.initialize()
```

**Practical Tips:**
- MCP messages are not guaranteed to be compatible with a specific LLM SDK format — write conversion logic to transform `SamplingMessage` objects into your SDK's format before passing them to Claude
- The server can use the returned text however it wants: return it, feed it into another tool call, or use it as part of a multi-step workflow
- Don't forget to pass the callback into `ClientSession`; omitting it means sampling requests silently fail

---

### Lesson: Log and Progress Notifications

**Key Concepts:**
- Without notifications, users see nothing during long-running tools — they can't tell if the server is working or hung
- The `Context` object (auto-injected as last argument to tool functions) provides all notification methods
- Notifications are purely UX enhancements — entirely optional but highly recommended for any tool taking more than a second

**Key Methods:**
| Method | Purpose |
|---|---|
| `context.info(msg)` | Send an informational log message |
| `context.warning(msg)` | Send a warning log message |
| `context.debug(msg)` | Send a debug log message |
| `context.error(msg)` | Send an error log message |
| `context.report_progress(current, total)` | Report numeric progress |

**Code Patterns:**

*Server side — emit logs and progress within a tool:*
```python
@mcp.tool(name="research", description="Research a given topic")
async def research(
    topic: str = Field(description="Topic to research"),
    *,
    context: Context
):
    await context.info("About to do research...")
    await context.report_progress(20, 100)
    sources = await do_research(topic)

    await context.info("Writing report...")
    await context.report_progress(70, 100)
    results = await generate_report(sources)

    return results
```

*Client side — define and wire up callbacks:*
```python
async def logging_callback(params: LoggingMessageNotificationParams):
    print(params.data)

async def print_progress_callback(
    progress: float, total: float | None, message: str | None
):
    if total is not None:
        percentage = (progress / total) * 100
        print(f"Progress: {progress}/{total} ({percentage:.1f}%)")
    else:
        print(f"Progress: {progress}")

async with ClientSession(read, write, logging_callback=logging_callback) as session:
    await session.initialize()
    await session.call_tool(
        name="add",
        arguments={"a": 1, "b": 3},
        progress_callback=print_progress_callback,
    )
```

**Practical Tips:**
- `logging_callback` goes into `ClientSession()`; `progress_callback` goes into `call_tool()` — they are wired up at different levels
- For web apps, push updates via WebSockets or SSE; for CLI apps, just print to terminal; for desktop apps, update a progress bar widget
- You can call `report_progress()` with just a current value (no total) for indeterminate progress

---

### Lesson: Roots

**Key Concepts:**
- Roots are a permission system that tells an MCP server which specific files/folders on the local machine it may access
- They also solve the UX problem of users needing to type full file paths — Claude can call `list_roots`, then `read_dir` to locate files automatically
- The MCP SDK does **not** enforce root restrictions automatically — you must implement the check yourself

**How It Works (flow):**
1. User provides allowed paths as CLI arguments (or via config)
2. Client converts paths into `Root` objects (URIs must start with `file://`)
3. Server calls `ctx.session.list_roots()` when it needs to know what's accessible
4. Server tools call a custom `is_path_allowed()` helper before accessing any file

**Implementation Pattern:**

*Client side — register roots callback:*
```python
# Convert user-provided paths to Root objects
roots = [Root(uri=f"file://{path}") for path in user_paths]

async def list_roots_callback() -> ListRootsResult:
    return ListRootsResult(roots=roots)

async with ClientSession(read, write, list_roots_callback=list_roots_callback) as session:
    await session.initialize()
```

*Server side — enforce access control:*
```python
async def is_path_allowed(requested_path: str, ctx: Context) -> bool:
    roots_result = await ctx.session.list_roots()
    for root in roots_result.roots:
        allowed = root.uri.replace("file://", "")
        if requested_path.startswith(allowed):
            return True
    return False

@mcp.tool()
async def read_file(path: str, ctx: Context):
    if not await is_path_allowed(path, ctx):
        raise PermissionError(f"Access to {path} is not permitted")
    # ... proceed with file read
```

**Practical Tips:**
- All root URIs must use the `file://` scheme per the MCP specification
- Implement `is_path_allowed()` as a shared helper and call it in **every** tool that touches the filesystem
- You can expose roots to Claude either via a dedicated `list_roots` tool or by injecting them directly into a system prompt
- Roots provide both security (limit blast radius) and UX (Claude can find files without full paths)

---

### Lesson: JSON Message Types

**Key Concepts:**
- All MCP communication is JSON messages; the full specification lives in the official MCP GitHub repo (written in TypeScript for clarity, not execution)
- Messages fall into two categories: **Request-Result pairs** (synchronous, expect a response) and **Notifications** (one-way, no response expected)
- Both clients and servers can initiate communication — MCP is a **bidirectional** protocol

**Request-Result pairs (examples):**
- `Call Tool Request` → `Call Tool Result`
- `List Prompts Request` → `List Prompts Result`
- `Read Resource Request` → `Read Resource Result`
- `Initialize Request` → `Initialize Result`

**Notification messages (examples):**
- `Progress Notification` — updates on long-running operations
- `Logging Message Notification` — system log messages
- `Tool List Changed Notification` — available tools changed
- `Resource Updated Notification` — a resource was modified

**Practical Tips:**
- The bidirectional nature of MCP (servers can send requests to clients) is what makes certain features work — and what HTTP transports must work around
- Consult the official MCP spec repo (not just SDK docs) for the canonical message format

---

### Lesson: The STDIO Transport

**Key Concepts:**
- STDIO transport: the client launches the server as a **subprocess** and communicates via stdin/stdout
- Either party can send a message at any time; true bidirectional communication with no inherent limitations
- Only works when client and server are on the **same machine**

**MCP Connection Handshake (required before any other messages):**
1. Client sends `Initialize Request`
2. Server responds with `Initialize Result` (including server capabilities)
3. Client sends `Initialized Notification` (no response expected)

**Four communication patterns with STDIO:**
1. Client → Server request: client writes to stdin
2. Server → Client response: server writes to stdout
3. Server → Client request: server writes to stdout
4. Client → Server response: client writes to stdin

**Practical Tips:**
- You can test an MCP server without a client by running it directly (`uv run server.py`) and pasting raw JSON into the terminal
- STDIO is the gold standard for local development and testing; use it to understand the "ideal" MCP behavior before moving to HTTP transports
- STDIO's simplicity makes it perfect for Claude Desktop integrations

---

### Lesson: The StreamableHTTP Transport

**Key Concepts:**
- StreamableHTTP enables **remote** MCP servers accessible over HTTP — clients and servers no longer need to be on the same machine
- HTTP is client-initiated by design, so server-to-client communication requires a workaround (Server-Sent Events / SSE)
- Two config flags can cripple functionality: `stateless_http` and `json_response`

**What breaks when `stateless_http=True` or `json_response=True`:**
- Progress notifications stop working
- Logging notifications stop working
- Server-initiated requests (sampling, list roots) fail
- SSE streaming is disabled

**MCP message types affected by HTTP's limitations:**
- `Create Message` requests (sampling)
- `List Roots` requests
- `Progress Notification`
- `Logging Message Notification`
- `Initialized Notification`
- `Cancelled Notification`

**Practical Tips:**
- If your server works locally with STDIO but breaks after HTTP deployment, check the `stateless_http` and `json_response` flags first
- Design servers to gracefully degrade when deployed in stateless mode (avoid relying on sampling or progress notifications)
- Only enable these restrictive flags when your deployment architecture actually requires them

---

### Lesson: StreamableHTTP In Depth

**Key Concepts:**
- StreamableHTTP uses **Server-Sent Events (SSE)** to work around HTTP's one-directional limitation
- Session IDs (`mcp-session-id` header) are used to identify clients and route server-to-client messages
- Tool calls create **two separate SSE connections** simultaneously

**StreamableHTTP Connection Setup:**
1. Client sends `Initialize Request` (POST)
2. Server responds with `Initialize Result` + `mcp-session-id` header
3. Client sends `Initialized Notification` with session ID
4. Client makes a GET request → establishes a persistent SSE stream for server-to-client messages

**Dual SSE Connection Model (during tool calls):**
| Connection | Purpose | Lifetime |
|---|---|---|
| Primary SSE (GET) | Server-initiated requests, some notifications | Stays open indefinitely |
| Tool-specific SSE (POST) | Progress notifications, logging, tool result | Closes after tool result sent |

**Message Routing:**
- Progress notifications → primary SSE connection
- Logging messages + tool results → tool-specific SSE connection

**Practical Tips:**
- Include the `mcp-session-id` header in every request after initialization — it's mandatory for the server to correlate client connections
- The dual-SSE model is the source of complexity in StreamableHTTP debugging; check which connection a message was expected on
- Setting `stateless_http=True` eliminates the SSE workaround entirely — no session IDs, no GET SSE stream, no server-to-client messages

---

### Lesson: State and the StreamableHTTP Transport

**Key Concepts:**
- `stateless_http=True` is designed for **horizontal scaling** behind load balancers, where different POST requests may hit different server instances
- Without stateless mode, a load balancer can route the GET SSE connection and POST tool calls to different instances, breaking server-to-client communication
- `json_response=True` disables streaming for POST responses — you get only the final result, no intermediate messages

**Trade-offs of `stateless_http=True`:**

| Feature | Stateful (default) | Stateless |
|---|---|---|
| Session IDs | ✅ Required | ❌ Not issued |
| Sampling (server → LLM via client) | ✅ Works | ❌ Broken |
| Progress reports | ✅ Works | ❌ Broken |
| Resource subscriptions | ✅ Works | ❌ Broken |
| Client initialization handshake | Required | Not required |
| Horizontal scaling / load balancers | Complex | ✅ Easy |

**Trade-offs of `json_response=True`:**
- Disables streaming on POST responses
- No intermediate progress or log messages during tool execution
- Simpler integration with systems expecting plain JSON

**When to use each flag:**

*Use `stateless_http=True` when:*
- Running multiple server instances behind a load balancer
- Tools don't require AI model sampling
- Server-to-client notifications are not needed

*Use `json_response=True` when:*
- You don't need streaming responses
- Integrating with systems that expect standard JSON HTTP responses

**Practical Tips:**
- Always test with the same transport you'll use in production — behavior differences between stateful and stateless can be dramatic
- Avoid building features that depend on sampling or progress notifications if you anticipate needing stateless HTTP for scale
- `stateless_http` and `json_response` can be set independently; you can use one without the other

---

## Top 15 Practical Tips (Consolidated)

1. **Use sampling for public servers** — never give a public MCP server its own API key; let clients pay for AI usage via the sampling callback pattern.

2. **Convert MCP messages before LLM calls** — `SamplingMessage` objects are MCP-format, not SDK-format; write explicit conversion logic before passing them to the Anthropic SDK (or any other LLM SDK).

3. **Wire callbacks at the right level** — `logging_callback` goes in `ClientSession()`; `progress_callback` goes in `call_tool()`. Getting these swapped is a common mistake.

4. **Always emit progress on long-running tools** — even rough estimates (20%, 70%, 100%) dramatically improve user experience compared to silent operations.

5. **Implement `is_path_allowed()` as a shared helper** — call it at the top of every tool that touches the filesystem; the MCP SDK will not protect you automatically.

6. **Root URIs must use `file://` prefix** — this is a hard MCP spec requirement; paths without it will fail.

7. **Expose roots to Claude via a tool or system prompt** — Claude needs to know what directories are available before it can resolve filenames to full paths.

8. **Use STDIO for development, HTTP for production** — but test with the same transport you'll deploy with; don't assume STDIO behavior transfers to HTTP.

9. **Always send the `mcp-session-id` header** in StreamableHTTP — every request after initialization must include it for proper routing.

10. **Check `stateless_http` and `json_response` first** when StreamableHTTP features break — these two flags are the most common cause of missing notifications and failed sampling.

11. **Design for graceful degradation** — if your server might be deployed in stateless HTTP mode, don't build core functionality around sampling or progress notifications.

12. **The MCP handshake is mandatory** — three messages must complete (Initialize Request → Initialize Result → Initialized Notification) before any other MCP messages can be sent.

13. **Servers can initiate requests to clients** — MCP is bidirectional; design clients to handle server-side requests (sampling, list roots), not just respond to their own tool calls.

14. **Use `stateless_http=True` for horizontal scaling** — when load balancers distribute traffic across multiple server instances, stateless mode eliminates the SSE coordination problem.

15. **Sampling is a chain, not a shortcut** — the server asks the client, the client calls Claude, Claude responds to the client, the client returns to the server; each step must be implemented correctly for the chain to work.
