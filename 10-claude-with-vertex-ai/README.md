# Claude with Google Cloud's Vertex AI — Course Notes

> Source: https://anthropic.skilljar.com/claude-with-google-clouds-vertex-ai
> Extracted: May 2026 (real text content from 72 lessons)

---

## Course Overview

This course covers building production-grade applications with Claude via **Google Cloud Vertex AI**, from initial setup through advanced patterns including tool use, RAG pipelines, MCP servers, and agent workflows. Key differentiator from the direct Anthropic API: authentication uses **gcloud credentials** (not an API key in code), and Vertex requires `project_id` and `region` configuration alongside model selection.

**Sections covered:**
1. Accessing Claude with the API (Vertex setup, requests, multi-turn, system prompts, temperature, streaming, structured data)
2. Prompt Evaluation (test datasets, eval pipelines, model & code-based graders)
3. Prompt Engineering Techniques (clarity, directness)
4. Tool Use (schemas, multi-turn, batch tool, structured data extraction, built-in tools)
5. Retrieval Augmented Generation (chunking, embeddings, BM25, hybrid search, reranking, contextual retrieval)
6. Advanced Features (extended thinking, images, PDFs, citations, prompt caching)
7. Model Context Protocol — MCP (clients, servers, tools, resources, prompts)
8. Claude Code & Computer Use
9. Agents and Workflows (parallelization, chaining, routing, agents)

---

## Section Notes

### Section: Accessing Claude with the API

**Key Concepts:**

- **Never call Claude from client-side code.** Always route through your own server — API credentials must stay server-side and never be exposed to browsers or clients.
- **Request lifecycle (5 steps):** Request to Server → Request to Vertex → Model Processing → Response to Server → Response to Client.
- **Text generation pipeline inside Claude:** Tokenization → Embedding → Contextualization → Generation. Generation is iterative: each token is selected probabilistically, appended, and the process repeats.
- **Stop conditions:** max_tokens reached, end-of-sequence token generated, or a stop sequence encountered.
- **Response metadata contains:** generated text, token usage (input + output), and stop reason.

**Vertex-Specific Setup (⚡ Vertex-only steps):**

```bash
# Step 1: Enable Anthropic models in Vertex AI Model Garden
# Navigate: console.cloud.google.com/vertex-ai/dashboard → Model Garden → Search "Anthropic" → Enable

# Step 2: Install and configure gcloud CLI
gcloud init
gcloud auth login
gcloud config set project YOUR_PROJECT_ID
gcloud auth application-default login
```

The Anthropic SDK automatically picks up **Application Default Credentials** — no API key needed in code when using Vertex.

**Vertex configuration in code (⚡ Vertex-only):**

```python
import anthropic

# Additional Vertex params vs direct API:
# - project_id
# - region
# The SDK auto-reads gcloud ADC credentials
client = anthropic.AnthropicVertex(project_id="your-project", region="us-east5")
response = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello!"}]
)
```

**Multi-turn Conversations:**

```python
messages = []
# Append user message
messages.append({"role": "user", "content": user_text})
# After getting response, append assistant message
messages.append({"role": "assistant", "content": response_text})
# Send entire history with each request — Claude has no memory; you manage it
```

- Messages must be in chronological order (oldest first).
- Token usage grows with each turn — monitor context window limits and truncate/summarize older messages as needed.

**System Prompts:**

```python
response = client.messages.create(
    model="claude-opus-4-5",
    system="You are a Python code reviewer. Review code for bugs, suggest best practices, and include corrected examples.",
    max_tokens=1024,
    messages=messages
)
```

- The `system` parameter is **separate** from `messages` — clean separation of instructions from conversation.
- Stays constant across all turns. Defines identity, values, behavioral guidelines, and format expectations.
- Avoid contradictory instructions between system prompt and user messages.

**Temperature:**

| Range | Use Case |
|-------|----------|
| 0.0–0.3 | Factual responses, coding, data extraction, content moderation |
| 0.4–0.7 | Summarization, education, problem-solving, constrained creative writing |
| 0.8–1.0 | Brainstorming, creative writing, marketing, jokes |

- Temperature adjusts selection probability across candidate tokens — it doesn't guarantee different outputs, just changes the probability of getting them.

**Response Streaming:**

```python
# Basic streaming
with client.messages.stream(model=..., max_tokens=..., messages=...) as stream:
    for text in stream.text_stream:
        print(text, end="", flush=True)

# Get final assembled message after streaming
final_message = stream.get_final_message()
```

- Stream event types: `MessageStart`, `ContentBlockStart`, `ContentBlockDelta` (actual text), `ContentBlockStop`, `MessageDelta`, `MessageStop`.
- In production, forward chunks via WebSockets or Server-Sent Events for real-time UX.
- Keep the full final message for conversation history storage.

**Structured Data Output (stop sequences + prefilling):**

```python
response = client.messages.create(
    model=...,
    messages=[{"role": "user", "content": "Generate an AWS EventBridge rule in JSON."}],
    stop_sequences=["```"],
    # Prefill assistant message to force Claude into the desired format:
    # add assistant message: {"role": "assistant", "content": "```json\n"}
)
# Claude thinks it already started the JSON block and fills it in, stopping when it hits ```
```

- Works for any structured format: JSON, Python code, CSV, regex.
- Clean up output with `json.loads(text.strip())` to validate and normalize whitespace.

---

### Section: Prompt Evaluation

**Key Concepts:**

- Prompt evaluation = systematic testing against representative test cases with automated scoring.
- Without evaluation, you're guessing. With it, you have data-driven insight into prompt quality and regressions.
- Evaluation transforms prompt development **from guesswork into science**.

**The Eval Workflow (5 steps):**

1. **Define criteria** — What does "good" look like? (format, correctness, completeness, readability)
2. **Generate test data** — Representative inputs including edge cases
3. **Build the eval pipeline** — Load data, run prompt, collect output, score it
4. **Analyze results** — Aggregate scores, identify failure patterns
5. **Iterate** — Fix prompt issues, re-run eval, repeat

**Generating Test Data with Claude:**

```python
# Use Claude to generate your test dataset when you lack real user data
# Prompt Claude:
"Generate 20 test cases for a prompt that converts natural language to Python regular expressions.
Include cases covering different complexity levels and edge cases like empty input."

# Structure each test case as:
{"task": "...", "format": "python|json|regex", "metadata": {...}}
```

**Eval Pipeline Pattern:**

```python
def run_prompt(test_case):
    prompt = f"Please solve the following task: {test_case['task']}"
    messages = [{"role": "user", "content": prompt}]
    return chat(messages)

def run_test_case(test_case):
    output = run_prompt(test_case)
    score = grade(output, test_case)  # model or code grader
    return {"output": output, "test_case": test_case, "score": score}

def run_eval(dataset):
    results = [run_test_case(tc) for tc in dataset]
    avg = statistics.mean(r["score"] for r in results)
    print(f"Average score: {avg}")
    return results
```

**Model-Based Grading:**

```python
def grade_by_model(test_case, output):
    eval_prompt = f"""
    You are a code quality grader. Score the following output 1-10.
    Task: {test_case['task']}
    Output: {output}
    Return JSON: {{"score": <int>, "reasoning": "<str>", "strengths": [...], "weaknesses": [...]}}
    """
    messages = [{"role": "user", "content": eval_prompt}]
    add_assistant_message(messages, "```json")
    result = chat(messages, stop_sequences=["```"])
    return json.loads(result)
```

- Always ask for **reasoning + strengths/weaknesses** alongside score — prevents middling 6/10 defaults.

**Code-Based Grading (syntax validation):**

```python
def validate_json(text):
    try: json.loads(text.strip()); return 10
    except json.JSONDecodeError: return 0

def validate_python(text):
    try: ast.parse(text.strip()); return 10
    except SyntaxError: return 0

def validate_regex(text):
    try: re.compile(text.strip()); return 10
    except re.error: return 0

# Combine model + syntax score:
score = (model_score + syntax_score) / 2
```

**Practical Tips:**

- Test data is a living artifact — add cases from production bugs, remove irrelevant ones.
- The score itself isn't inherently good or bad — what matters is *improving* it over iterations.
- Expect ~30 seconds for a full dataset run with Claude Haiku.

---

### Section: Prompt Engineering Techniques

**Key Concepts:**

- **The first line is the most important part of your prompt.** It sets the stage for everything that follows.
- Two principles: **Clarity** (simple, unambiguous language) and **Directness** (instructions, not questions; action verbs).

**Clear and Direct Prompting:**

```
❌ Weak:  "What should this person eat?"
✅ Strong: "Generate a one-day meal plan for an athlete that meets their dietary restrictions."

❌ Weak:  "I was reading about renewable energy and geothermal sounds neat. What countries use it?"
✅ Strong: "Identify three countries that use geothermal energy. Include generation stats for each."
```

- Start with an **action verb**: Write, Create, Generate, Identify, Analyze, List, Summarize.
- In one real example, rewriting the opening line improved eval scores from 2.32 → 3.92.

---

### Section: Tool Use

**Key Concepts:**

- Tools allow Claude to access real-time information and perform actions beyond its training data.
- Tool use follows a 4-step pattern: Initial Request → Tool Request (Claude decides) → Data Retrieval (your server runs code) → Final Response.

**Tool Functions — Best Practices:**

```python
def get_current_datetime(format: str = "%Y-%m-%d %H:%M:%S") -> str:
    """Get the current date and time."""
    if not format:
        return "Error: format parameter cannot be empty"
    return datetime.now().strftime(format)
```

- Use descriptive function and parameter names.
- Always validate inputs and return meaningful error messages — Claude can retry with corrected parameters.
- Provide 3–4 sentence descriptions in the JSON schema's `description` field.

**Tool Schema — Let Claude Write It:**

```python
# Instead of hand-writing JSON schema, prompt Claude:
"Write a valid JSON schema spec for tool calling for this function. 
Follow the best practices in the attached Anthropic tool use documentation."

# Schema structure:
{
    "name": "get_current_datetime",
    "description": "Get the current date and time. Use when the user asks about current time or needs to calculate relative dates.",
    "input_schema": {
        "type": "object",
        "properties": {
            "format": {
                "type": "string",
                "description": "strftime format string, e.g. '%Y-%m-%d %H:%M:%S'"
            }
        },
        "required": []
    }
}
```

**Making Tool-Enabled API Calls:**

```python
response = client.messages.create(
    model=...,
    max_tokens=...,
    tools=[tool_schema_1, tool_schema_2],  # Pass tool schemas
    messages=messages
)
```

**Handling Multi-Block Messages:**

When Claude uses a tool, the response contains **multiple content blocks**:
- `TextBlock` — Claude's explanation of what it's doing
- `ToolUseBlock` — which tool to call, with what parameters, and a unique ID

```python
# CRITICAL: Store the entire content list, not just text
messages.append({"role": "assistant", "content": response.content})
# Then add tool result:
messages.append({
    "role": "user",
    "content": [{
        "type": "tool_result",
        "tool_use_id": tool_use_block.id,  # Must match the original tool use ID
        "content": str(tool_result),
        "is_error": False
    }]
})
```

**Multi-Turn Tool Conversation Loop:**

```python
def run_with_tools(messages, tools):
    while True:
        response = client.messages.create(
            model=..., max_tokens=..., tools=tools, messages=messages
        )
        messages.append({"role": "assistant", "content": response.content})
        
        if response.stop_reason != "tool_use":
            break  # Claude has a final answer
        
        # Process all tool use blocks
        tool_results = []
        for block in response.content:
            if block.type == "tool_use":
                result = execute_tool(block.name, block.input)
                tool_results.append({
                    "type": "tool_result",
                    "tool_use_id": block.id,
                    "content": str(result)
                })
        messages.append({"role": "user", "content": tool_results})
    
    return response
```

- Check `response.stop_reason == "tool_use"` to detect if Claude needs more tools.
- Always pass `tools` in the follow-up request even if Claude won't need them again — it references them for context.

**Batch Tool Pattern:**

```python
# Claude sometimes calls tools one at a time instead of in parallel.
# Solution: define a "batch" meta-tool that accepts a list of tool invocations.
{
    "name": "batch",
    "description": "Run multiple tools simultaneously when operations are independent.",
    "input_schema": {
        "type": "object",
        "properties": {
            "invocations": {
                "type": "array",
                "items": {"type": "object", "properties": {"name": ..., "input": ...}}
            }
        }
    }
}
# In your tool router, handle "batch" by looping over invocations
```

**Tools for Structured Data Extraction:**

```python
# Use tool_choice to FORCE Claude to call a specific schema tool
response = client.messages.create(
    model=...,
    tools=[extraction_schema],
    tool_choice={"type": "tool", "name": "extract_data"},  # Force specific tool
    messages=messages
)
# Extract structured data directly from tool use block
data = response.content[0].input  # Your structured output
```

- `tool_choice` options: `"auto"` (default), `"any"` (must use a tool), `{"type": "tool", "name": "..."}` (must use this specific tool).

**Built-in Tools:**

- **Text Editor Tool** — Built-in schema; you implement the handlers. Supports: view, view range, replace, create, insert, undo. Schema stub depends on Claude model version (see docs).
- **Web Search Tool** — Fully automatic; Claude handles search logic. Set `max_uses` to limit API calls. Restrict to trusted domains with `allowed_domains`. Response includes `CitationsWebSearchResultLocation` blocks for building verifiable UIs.

---

### Section: Retrieval Augmented Generation (RAG)

**Key Concepts:**

- RAG = break large documents into chunks → embed chunks → store in vector DB → at query time, find relevant chunks → inject into prompt.
- Why: LLMs have context window limits, and large prompts are slower and more expensive. RAG lets you work with documents far larger than any context window.

**Text Chunking Strategies:**

| Strategy | When to Use | Trade-offs |
|----------|-------------|------------|
| **Size-based** (fixed character count + overlap) | Mixed/unknown formats, code | Simple; can cut sentences mid-way; add overlap to preserve context |
| **Structure-based** (split on headers/paragraphs) | Markdown, well-structured docs | Best quality when structure is consistent; fails on plain text |
| **Semantic-based** (NLP similarity grouping) | Highest-quality needs | Expensive; complex; usually overkill |

```python
# Size-based with overlap
def chunk_by_size(text, chunk_size=500, overlap=50):
    chunks = []
    start = 0
    while start < len(text):
        end = start + chunk_size
        chunks.append(text[start:end])
        start = end - overlap
    return chunks

# Structure-based (markdown)
def chunk_by_headers(text):
    return [section for section in text.split('\n## ') if section.strip()]
```

**Text Embeddings on Vertex AI (⚡ Vertex-specific):**

```python
from google.genai import Client  # Google GenAI SDK

# Vertex embedding model
# pip install google-genai
client = Client(vertexai=True, project="your-project", location="us-central1")

def get_embedding(text: str) -> list[float]:
    result = client.models.embed_content(
        model="text-embedding-005",  # Vertex AI embedding model
        contents=text
    )
    return result.embeddings[0].values
```

- Claude **cannot** generate embeddings — use a dedicated embedding model (`text-embedding-005` on Vertex).
- Embeddings are normalized vectors; similarity is measured by **cosine similarity** (1.0 = identical, 0.0 = unrelated, -1.0 = opposite).

**Full RAG Flow:**

```
1. Preprocess: chunk docs → generate embeddings → store in vector DB (with original text)
2. At query time:
   - Embed user query with same model
   - Find top-k most similar chunks (lowest cosine distance)
   - Inject relevant chunks into Claude prompt
3. Claude answers based on retrieved context, not training data alone
```

**Hybrid Search (Vector + BM25):**

- Semantic search misses exact term matches (e.g., incident IDs like `INC-2023-Q4-011`).
- BM25 lexical search handles exact matches but misses semantic similarity.
- **Run both in parallel, merge with Reciprocal Rank Fusion (RRF):**

```python
# RRF formula: score(d) = Σ(1 / (k + rank_i(d)))
# k is typically 60; use lower values (e.g., 1) for clearer weighting
def reciprocal_rank_fusion(result_lists, k=60):
    scores = {}
    for results in result_lists:
        for rank, doc_id in enumerate(results, 1):
            scores[doc_id] = scores.get(doc_id, 0) + 1.0 / (k + rank)
    return sorted(scores, key=scores.get, reverse=True)
```

**Reranking:**

```python
# After hybrid search, use Claude to intelligently reorder results
def rerank(query, candidate_docs):
    # Pass doc IDs (not full text) to Claude for efficiency
    rerank_prompt = f"""
    User query: {query}
    
    Rank these document IDs from most to least relevant:
    {[{"id": d.id, "preview": d.text[:200]} for d in candidate_docs]}
    
    Return only a JSON array of IDs in order of relevance.
    """
    # ...call Claude and parse ordered IDs
```

- Reranking adds latency (extra LLM call) but significantly improves accuracy for intent-sensitive queries.

**Contextual Retrieval:**

```python
# Before storing chunks, add document-level context to each chunk
def add_context_to_chunk(chunk, source_document):
    context_prompt = f"""
    Source document (or excerpt):
    {source_document[:2000]}  # First N chars if too large
    
    Add a 2-3 sentence context snippet that situates this chunk within the larger document:
    {chunk}
    """
    context = chat(context_prompt)
    return f"{context}\n\n{chunk}"  # Prepend context to chunk
```

- Solves the problem where chunks lose their connection to the broader document after splitting.
- Most valuable for complex technical/academic documents with cross-section references.

---

### Section: Advanced Features

**Extended Thinking:**

```python
response = client.messages.create(
    model="claude-opus-4-5",
    max_tokens=5000,  # Must be > thinking_budget
    thinking={"type": "enabled", "budget_tokens": 1024},  # min 1024
    messages=messages
)
# Response content has TWO blocks:
# [0] ThinkingBlock — Claude's reasoning (with cryptographic signature)
# [1] TextBlock — Final answer

# NEVER modify the thinking text — signature validates integrity
# Redacted thinking blocks (safety-flagged) still carry encrypted content;
# include them in conversation history to preserve context
```

- Use extended thinking **only after** standard prompt optimization fails to hit quality targets.
- `max_tokens` must exceed `budget_tokens` (the thinking budget).
- Test redacted thinking handling in dev by including a known trigger string.

**Image Support:**

```python
# Include images as base64 or URL alongside text blocks
message = {
    "role": "user",
    "content": [
        {"type": "image", "source": {"type": "base64", "media_type": "image/jpeg", "data": base64_data}},
        {"type": "text", "text": "Count the marbles in this image. Follow these steps: 1) ..."}
    ]
}
```

- Limits: up to 100 images/request; 5MB max/image; 8000px max (single), 2000px max (multiple).
- Token cost = `(width × height) / 750`.
- **Prompting quality matters enormously** — simple prompts give poor results even on clear images.
- Use step-by-step methodology prompts and one-shot examples for tasks requiring accuracy.

**PDF Support:**

- PDFs treated as document sources similar to plain text.
- Pass as `source` with `type: "document"`.

**Citations:**

```python
messages = [{
    "role": "user",
    "content": [{
        "type": "document",
        "source": {"type": "text", "media_type": "text/plain", "data": document_text},
        "title": "Q4 Financial Report",
        "citations": {"enabled": True}
    }, {
        "type": "text",
        "text": "What are the key risk factors?"
    }]
}]
# Response includes CitationCharLocation or page-based citations
# Each citation: cited_text, document_index, document_title, start/end page
```

- Transforms Claude from a "black box" into a **verifiable, transparent** system.
- Essential for applications where users need to audit or verify information.

**Prompt Caching:**

```python
# Use longhand form to add cache breakpoints
system = [{
    "type": "text",
    "text": "You are a helpful assistant...",
    "cache_control": {"type": "ephemeral"}
}]

# Cache tool schemas (add to last tool in list)
tools_with_cache = [*tools[:-1], {**tools[-1], "cache_control": {"type": "ephemeral"}}]
```

- Cache lives for **5 minutes**; content must be **exactly identical** to get a cache hit.
- Minimum **1024 tokens** to be eligible for caching.
- Cache order: tool schemas → system prompt → message history.
- You can set **up to 4 cache breakpoints** per request.
- First request: `cache_creation_input_tokens` in usage. Subsequent: `cache_read_input_tokens`.
- **Even a single character change invalidates the cache entry.**
- Most valuable for stable content: tool schemas, system prompts, repeated document analysis.

---

### Section: Model Context Protocol (MCP)

**Key Concepts:**

- MCP = communication layer that provides Claude with context and tools **without you writing all the integration code**.
- Instead of writing tool schemas + functions for every external service, you connect to **MCP Servers** that expose pre-built tools.
- You can be: an **MCP Client** (consuming tools from servers), an **MCP Server** (exposing your service's tools), or both (for learning/testing).

**MCP Architecture:**

```
Your App (MCP Client) ←→ MCP Server ←→ External Service (GitHub, AWS, DB, etc.)
```

**MCP Server — Defining Tools (Python SDK):**

```python
from mcp.server.fastmcp import FastMCP
from pydantic import Field

mcp = FastMCP("DocumentMCP", log_level="ERROR")

@mcp.tool(
    name="read_document",
    description="Read the contents of a document by ID. Returns the full text content."
)
def read_document(
    doc_id: str = Field(description="The unique identifier of the document to read")
) -> str:
    if doc_id not in docs:
        raise ValueError(f"Document '{doc_id}' not found")
    return docs[doc_id]

@mcp.tool(name="edit_document", description="Find and replace text in a document.")
def edit_document(
    doc_id: str = Field(description="Document ID to edit"),
    find: str = Field(description="Exact text to find (case-sensitive, whitespace-sensitive)"),
    replace: str = Field(description="Replacement text")
) -> str:
    if doc_id not in docs:
        raise ValueError(f"Document '{doc_id}' not found")
    docs[doc_id] = docs[doc_id].replace(find, replace)
    return f"Updated document '{doc_id}'"
```

- The SDK auto-generates JSON schemas from type hints and `Field` descriptions — no manual schema writing.

**MCP Server Inspector (debug tool):**

```bash
# Start server with inspector
uv run mcp dev mcp_server.py
# Opens browser-based UI to test tools, resources, and prompts interactively
```

**MCP Server — Defining Resources:**

```python
# Direct resource (static URI)
@mcp.resource("docs://documents", mime_type="application/json")
def list_docs() -> list[str]:
    return list(docs.keys())

# Templated resource (URI with parameters)
@mcp.resource("docs://documents/{doc_id}", mime_type="text/plain")
def fetch_doc(doc_id: str) -> str:
    if doc_id not in docs:
        raise ValueError(f"Doc '{doc_id}' not found")
    return docs[doc_id]
```

**MCP Server — Defining Prompts:**

```python
from mcp import types as base

@mcp.prompt(
    name="format",
    description="Rewrites a document in well-structured Markdown format"
)
def format_document(
    doc_id: str = Field(description="ID of the document to format")
) -> list[base.Message]:
    prompt = f"""
    Reformat the document with id '{doc_id}' using proper Markdown syntax.
    Add headers, bullet points, tables, and structure as appropriate.
    Use the 'edit_document' tool to save changes.
    """
    return [base.UserMessage(prompt)]
```

**MCP Client Implementation:**

```python
from mcp import ClientSession, types
from pydantic import AnyUrl
import json

class MCPClient:
    def __init__(self, session):
        self._session = session
    
    async def list_tools(self) -> list[types.Tool]:
        result = await self._session.list_tools()
        return result.tools
    
    async def call_tool(self, tool_name: str, tool_input: dict):
        return await self._session.call_tool(tool_name, tool_input)
    
    async def read_resource(self, uri: str):
        result = await self._session.read_resource(AnyUrl(uri))
        resource = result.contents[0]
        if isinstance(resource, types.TextResourceContents):
            if resource.mimeType == "application/json":
                return json.loads(resource.text)
            return resource.text
    
    async def list_prompts(self) -> list[types.Prompt]:
        result = await self._session.list_prompts()
        return result.prompts
    
    async def get_prompt(self, prompt_name: str, args: dict[str, str]):
        result = await self._session.get_prompt(prompt_name, args)
        return result.messages
```

**MCP Primitive Decision Guide:**

| Primitive | Controlled By | Use When |
|-----------|--------------|----------|
| **Tools** | The AI model (Claude decides when to call) | Extending Claude's capabilities with external actions |
| **Resources** | Your application code | Fetching data for UI (autocomplete, file lists) or adding context to prompts |
| **Prompts** | The user (triggered on demand) | Pre-built workflows, task templates, specialized conversation starters |

> **Key rule:** Tools serve the model. Resources serve your app. Prompts serve your users.

**MCP Message Types:**

- `ListToolsRequest` / `ListToolsResult` — "What tools do you provide?"
- `CallToolRequest` / `CallToolResult` — "Run this tool with these args."
- `ReadResourceRequest` / `ReadResourceResult` — "Give me this resource's data."
- MCP is **transport-agnostic**: stdio (same machine), HTTP, WebSockets all supported.

---

### Section: Claude Code & Computer Use

**Claude Code:**

- CLI tool for AI-powered development (`npm install -g @anthropic-ai/claude-code`).
- Modes: interactive, batch, headless.
- Capabilities: write new code, modify files, multi-file refactoring, debug, write tests.
- Proposes changes with confirmation before modifying files.
- **Extend with MCP servers** to access databases, APIs, custom tools via `claude-code` settings.
- **Parallelization**: run independent tasks in multiple concurrent sessions to reduce total time.
- **Automated debugging**: feed stack traces, error logs, or describe unexpected behavior; Claude traces code and proposes fixes.

**Computer Use:**

- Claude sees screen via periodic screenshots → analyzes with vision → predicts and executes mouse/keyboard actions → feedback loop.
- API sends screenshots to Claude + available actions; Claude responds with action to take.
- Safety: confirmation prompts for destructive actions, rate limiting to prevent runaway automation.
- Use for: GUI automation, navigating websites, operating desktop software — anything requiring keyboard/mouse.

---

### Section: Agents and Workflows

**Key Concepts:**

- **Workflows** = predefined patterns (deterministic, auditable, sequential steps).
- **Agents** = Claude + tools in an autonomous loop (think → act → observe → repeat until goal reached).

**Decision Framework:**

```
Can you define the exact steps? → Use WORKFLOWS
Does the AI need to figure out the steps? → Use AGENTS
Need both flexibility and structure? → Use WORKFLOWS that orchestrate AGENTS
```

**Workflow Patterns:**

| Pattern | How It Works | When to Use |
|---------|-------------|-------------|
| **Parallelization** | Multiple independent operations run concurrently | Processing many documents, batch API calls, parallel analysis |
| **Chaining** | Output of step N feeds as input to step N+1 | Generate-then-refine, search-then-answer, translate-then-review |
| **Routing** | Classify input, direct to appropriate specialist | Customer service triage, document type routing, skill routing |
| **Agents** | Autonomous loop with tool use until goal met | Open-ended problems, unknown solution paths |

**Chaining Example:**

```python
# Generate-Then-Refine pattern
draft = client.messages.create(prompt=initial_prompt)
refined = client.messages.create(
    prompt=f"Improve this draft for clarity and tone:\n\n{draft.content[0].text}"
)
```

**Routing Pattern:**

```python
# Three approaches:
# 1. Rule-based: if/else for clear categories
# 2. Embedding-based: cosine similarity to category examples
# 3. LLM-based: ask Claude to classify and route
classifier_response = client.messages.create(
    prompt=f"Classify this request as 'billing', 'technical', or 'general': {user_message}"
)
```

**Agent Architecture:**

```python
def run_agent(goal: str, tools: list):
    messages = [{"role": "user", "content": goal}]
    while True:
        response = client.messages.create(
            model=..., max_tokens=..., tools=tools, messages=messages
        )
        messages.append({"role": "assistant", "content": response.content})
        
        if response.stop_reason != "tool_use":
            return extract_text(response)  # Goal achieved
        
        # Execute tools and continue loop
        tool_results = execute_all_tools(response.content)
        messages.append({"role": "user", "content": tool_results})
```

- Define clear success criteria and stopping conditions.
- Cache inspection results to avoid redundant tool calls.
- Handle tool failures gracefully — return error messages so Claude can try alternatives.

---

## Top 20 Practical Tips (Consolidated)

1. **⚡ Vertex auth = gcloud ADC, not API keys.** Run `gcloud auth application-default login` and the Anthropic SDK auto-picks up credentials. Never put credentials in client-side code.

2. **⚡ Vertex requires `project_id` and `region`** in addition to the model name. Use `anthropic.AnthropicVertex(project_id=..., region=...)`.

3. **⚡ Use `text-embedding-005` on Vertex AI** for embeddings — Claude itself cannot generate embeddings.

4. **Use stop sequences + assistant prefilling for clean structured output.** Prefill the assistant message with `"```json\n"` and stop on `"```"` to extract pure JSON without commentary.

5. **Measure before optimizing.** Build an eval pipeline first. Score baseline, then iterate. Even restructuring the opening line of a prompt can double the score (2.32 → 3.92 in course example).

6. **Let Claude generate your tool JSON schemas.** Give it the function + Anthropic tool use docs and ask it to write a proper schema. Faster and often better quality than writing by hand.

7. **Always store the full `response.content` (all blocks) in message history** when using tools — not just text. Dropping the `ToolUseBlock` breaks conversation continuity.

8. **Match `tool_use_id` exactly** when returning tool results. Claude uses these IDs to correlate results with requests, especially when multiple tools are called in one response.

9. **Use the Batch Tool pattern** when you need Claude to call multiple independent tools simultaneously. Define a meta-`batch` tool that accepts a list of invocations.

10. **Hybrid search outperforms semantic-only RAG.** Combine vector embeddings (semantic) + BM25 (exact keywords) and merge with Reciprocal Rank Fusion. Especially important for exact IDs, codes, and names.

11. **Add context to RAG chunks before storing.** Use contextual retrieval — ask Claude to write a 2-3 sentence context snippet per chunk that situates it in the source document.

12. **Prompt caching saves cost and reduces latency.** Cache stable content (system prompts, tool schemas) with `cache_control: {type: "ephemeral"}`. Min 1024 tokens; cache lasts 5 minutes; one character change invalidates it.

13. **Enable extended thinking only after other prompt improvements fail.** It costs more (charged for thinking tokens) and adds latency. Never modify thinking block content — its cryptographic signature will break.

14. **Vision prompting requires the same engineering rigor as text prompting.** Simple "count the objects" prompts fail. Use step-by-step methodology prompts and one-shot examples for accuracy.

15. **MCP tools serve the model; resources serve your app; prompts serve your users.** Pick the right primitive: tools for Claude's autonomous use, resources for your UI/context injection, prompts for user-triggered workflows.

16. **Use the MCP Inspector (`uv run mcp dev server.py`) for debugging.** If something works in the inspector but not in your client, the issue is in your client implementation.

17. **For agents: define clear stopping criteria** and implement graceful error handling in tool functions. Return informative error messages so Claude can adapt and retry.

18. **Use workflows when steps are predictable; use agents when the path is unknown.** Hybrid: orchestrate agents as steps within workflows.

19. **Temperature is a dial, not a guarantee.** At `temperature=1.0`, Claude might still occasionally repeat itself. At `temperature=0`, it might occasionally vary. Tune per use-case and verify with eval.

20. **For prompt caching with tools, create a copy before modifying** to avoid mutating your original tool schemas: `tools_with_cache = [*tools[:-1], {**tools[-1], "cache_control": {"type": "ephemeral"}}]`. Reordering tools can break caching.
