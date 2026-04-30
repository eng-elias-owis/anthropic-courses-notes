# Claude with Amazon Bedrock — Course Notes

> Source: https://anthropic.skilljar.com/claude-with-amazon-bedrock
> Extracted: April 2026 (real text content from 65 lessons)

---

## Course Overview

This course teaches how to build production-ready AI applications using Claude models on AWS Bedrock. It covers the full stack: API access via `boto3`, prompt engineering and evaluation, tool use, Retrieval Augmented Generation (RAG), advanced Claude features (extended thinking, images, prompt caching), and the Model Context Protocol (MCP). Code examples are in Python using the `boto3` Bedrock Runtime client.

**Bedrock-specific notes:**
- Access Claude via `boto3.client('bedrock-runtime', region_name=...)` — not the Anthropic Python SDK
- Model IDs must match your AWS region; use **Cross-Region Inference Profiles** to avoid region issues
- The `converse` and `converse_stream` methods abstract over model-specific APIs
- Embedding models (e.g., Amazon Titan) are accessed via `client.invoke_model()`

---

## Section Notes

### Section: Course Introduction

**Key Concepts:**
- The course targets developers building real applications with Claude on AWS Bedrock
- Three model families with distinct trade-offs:
  - **Claude Opus** — highest intelligence, reasoning/extended thinking support, higher cost & latency; best for complex autonomous tasks
  - **Claude Sonnet** — balanced intelligence/speed/cost, strong coding, most popular for production
  - **Claude Haiku** — fastest, cheapest, no reasoning support; best for real-time user-facing tasks
- Teams often use **multiple models in the same app**: Haiku for UI interactions, Sonnet for business logic, Opus for deep reasoning tasks

**Practical Tips:**
- Don't pick one model for everything; route different tasks to the right model
- Haiku is ideal for generating test datasets (fast + cheap)
- Use evaluations to determine if you actually need Opus vs. Sonnet for your use case

---

### Section: Working with the API

**Key Concepts:**
- **Request flow**: User → your server → Bedrock client → Claude model → response back
- **Three required components** for any request: Bedrock Runtime Client, Model ID, User Message
- **No state storage**: Bedrock and Claude do NOT store messages. You must maintain conversation history yourself and send the full history with every request
- **Message structure**: `{'role': 'user'/'assistant', 'content': [{'text': '...'}]}` — content is a list to support multimodal (text + images)
- **Inference Profiles**: solve regional availability by auto-routing to a region where the model is hosted. Find IDs under "Cross-region inference" in the Bedrock console, not the model catalog

**Practical Tips & Code Patterns:**

```python
import boto3

client = boto3.client('bedrock-runtime', region_name='us-west-2')
model_id = 'us.anthropic.claude-sonnet-4-5...'  # Use inference profile ID

# Basic request
response = client.converse(
    modelId=model_id,
    messages=[{'role': 'user', 'content': [{'text': 'Hello!'}]}]
)
text = response['output']['message']['content'][0]['text']
```

```python
# Multi-turn conversation helpers
def add_user_message(messages, text):
    messages.append({'role': 'user', 'content': [{'text': text}]})

def add_assistant_message(messages, text):
    messages.append({'role': 'assistant', 'content': [{'text': text}]})

def chat(messages, system=None, temperature=1.0, stop_sequences=[]):
    params = {
        'modelId': model_id,
        'messages': messages,
        'inferenceConfig': {'temperature': temperature, 'stopSequences': stop_sequences}
    }
    if system:
        params['system'] = [{'text': system}]
    response = client.converse(**params)
    return response['output']['message']['content'][0]['text']
```

- **System prompts** are passed as `system=[{"text": "..."}]` — give Claude a role, not a list of rules
- **Temperature**: 0–0.3 for factual/code tasks, 0.4–0.7 for summarization, 0.8–1.0 for creative/brainstorming. Default is 1.0
- **Streaming**: use `converse_stream`; iterate over `response["stream"]`, filter for `contentBlockDelta` events

```python
response = client.converse_stream(messages=messages, modelId=model_id)
for event in response['stream']:
    if 'contentBlockDelta' in event:
        print(event['contentBlockDelta']['delta']['text'], end='')
```

- **Prefilled assistant messages**: add an assistant message at the end of the list to steer Claude's response direction

```python
add_assistant_message(messages, "```json")
text = chat(messages, stop_sequences=["```"])
import json
data = json.loads(text.strip())
```

- **Stop sequences**: pass `stopSequences=["5"]` in `inferenceConfig` to halt generation at a specific token
- Combine prefilling + stop sequences to extract clean structured output (JSON, code, CSV) without Claude's preamble

---

### Section: Prompt Evaluations

**Key Concepts:**
- **Prompt engineering** = craft of writing better prompts; **Prompt evaluation** = automated, objective measurement of prompt quality
- Three paths after writing a prompt:
  1. Test once → risky (users behave unpredictably)
  2. Test a few times → still risky
  3. **Run through an evaluation pipeline with scoring** → recommended for production
- Typical eval workflow: draft prompt → create dataset → run through Claude → grade outputs → iterate
- **Grader types**:
  - **Code graders**: syntax validation (JSON, Python AST, regex compile) — fast, deterministic
  - **Model graders**: ask Claude to score output quality (1–10), with strengths/weaknesses/reasoning — flexible but can be inconsistent
  - **Human graders**: most flexible, most tedious

**Practical Tips & Code Patterns:**

```python
# Model grader — always ask for reasoning, not just a score
eval_prompt = f"""
You are an expert code reviewer. Evaluate this AI-generated solution.
Task: {task}
Solution: {solution}
Return JSON with: strengths (array), weaknesses (array), reasoning (string), score (1-10)
"""
add_assistant_message(messages, "```json")
eval_text = chat(messages, stop_sequences=["```"])
grade = json.loads(eval_text)

# Code grader for syntax validation
def validate_json(text):
    try: json.loads(text.strip()); return 10
    except: return 0

def validate_python(text):
    import ast
    try: ast.parse(text.strip()); return 10
    except SyntaxError: return 0
```

- **Combine scores**: `score = (model_score + syntax_score) / 2` — weight based on what matters most
- Dataset generation: use Claude Haiku (fast + cheap) to auto-generate diverse test cases
- Use `statistics.mean()` across test cases for an overall prompt score
- Build a `PromptEvaluator` class with `max_concurrent_tasks` parameter for parallel execution (start at 3 to avoid rate limits)
- **Always compare scores across iterations** — a single score in isolation is meaningless; track improvement over prompts v1, v2, v3…

---

### Section: Prompt Engineering

**Key Concepts:**
- Prompt engineering is iterative: write → evaluate → apply technique → re-evaluate
- Four core techniques covered: clarity/directness, specificity, XML structure, and examples (few-shot)

**Technique 1 — Be Clear and Direct:**
- First line is the most important; use action verbs: "Write," "Generate," "Create," "Identify"
- Bad: "What should this person eat?" → Good: "Generate a one-day meal plan for an athlete that meets their dietary restrictions."
- Result: score jumped from 2.32 → 3.92 with just this change

**Technique 2 — Be Specific:**
- Two approaches: **quality guidelines** (what the output should look like) and **process steps** (how Claude should think)
- Example quality guidelines: "Include accurate daily calorie amount," "Show protein/fat/carb amounts," "List portion sizes in grams"
- Result: score jumped from 3.92 → 7.86 with specific guidelines
- Use process steps when dealing with complex reasoning, decision-making, or critical thinking tasks

**Technique 3 — Structure with XML Tags:**
- Use XML tags as delimiters when prompts have lots of interpolated content or multiple distinct sections
- Tag names are arbitrary — use descriptive names: `<athlete_information>`, `<my_code>`, `<docs>`
- Most useful when large data blocks might be confused with instructions

```xml
<athlete_information>
- Height: {height}
- Weight: {weight}
- Goal: {goal}
- Dietary restrictions: {restrictions}
</athlete_information>
```

**Technique 4 — Provide Examples (Few-Shot/Multi-Shot):**
- One-shot/multi-shot prompting: provide `<sample_input>` + `<ideal_output>` pairs
- Especially useful for: corner cases (sarcasm, ambiguity), complex output formats, specific JSON structures
- Find best examples from your eval HTML report (highest-scoring outputs)
- Add explanation after `<ideal_output>` to help Claude understand *why* it's good

```xml
Be careful with tweets that contain sarcasm. For example:
<sample_input>Oh yeah, I really needed a flight delay tonight!</sample_input>
<ideal_output>Negative</ideal_output>
```

---

### Section: Tool Use

**Key Concepts:**
- Tools let Claude access external data/actions it can't do alone (weather, current time, setting reminders, database writes)
- Tool use is **multi-turn**: User → send tools list to Claude → Claude requests a tool → your server executes → return result to Claude → Claude responds
- You define tools as: (1) Python functions + (2) JSON schema describing them to Claude
- JSON Schema is not AI-specific; use online converters to generate schemas from sample data, then add descriptions

**Practical Tips & Code Patterns:**

```python
# Tool function best practices
from datetime import datetime

def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"):
    """Well-named, typed, returns meaningful value or raises on bad input"""
    return datetime.now().strftime(date_format)

# Tool schema structure for Bedrock
tool_schema = {
    "toolSpec": {
        "name": "get_weather",
        "description": "Gets current weather for a location. Use when user asks about weather. Returns temperature and conditions. Examples: 'London, UK', 'New York, US'",
        "inputSchema": {
            "json": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "City and country, e.g. 'Paris, France'"
                    }
                },
                "required": ["location"]
            }
        }
    }
}
```

- **Description quality matters**: 3–4 sentences explaining what the tool does, when to use it, what it returns, and example values
- Claude will retry tool calls if they return an error — make error messages helpful
- You can ask Claude to write parameter descriptions for you by pasting the function
- **JSON Schema generation workflow**: write sample data dict → convert to JSON → use online JSON→JSON Schema converter → remove `$schema` → add descriptions

---

### Section: Retrieval Augmented Generation

**Key Concepts:**
- RAG solves the problem of large documents that don't fit in a prompt, or that degrade performance when fully included
- Core flow: chunk doc → embed chunks → store in vector DB → embed user query → cosine similarity search → inject top chunks into prompt
- **Text embeddings**: numerical vectors (e.g., 1024 floats) representing semantic meaning. Similar texts → similar vectors
- **Cosine similarity**: ranges -1 to 1; values near 1 = very similar. `cosine_distance = 1 - cosine_similarity` (lower = better match)
- **BM25 (lexical search)**: keyword-frequency-based; essential for exact term matches (IDs, codes, names) that semantic search misses
- **Hybrid search**: run vector + BM25 in parallel, merge with **Reciprocal Rank Fusion (RRF)**
- **Reranking**: after hybrid search, pass merged results to Claude to reorder by relevance — increases accuracy at cost of latency
- **Contextual retrieval**: before storing chunks, ask Claude to generate a short context snippet for each chunk describing its role in the full document; prepend to chunk before embedding

**Practical Tips & Code Patterns:**

```python
# Generate embedding with Amazon Titan (Bedrock-specific)
def generate_embedding(text, model_id="amazon.titan-embed-text-v2:0", dimensions=1024):
    body = json.dumps({"inputText": text, "dimensions": dimensions, "normalize": True})
    response = client.invoke_model(
        modelId=model_id, body=body,
        accept="application/json", contentType="application/json"
    )
    return json.loads(response["body"].read())["embedding"]
```

- Note: you may need to request access to Titan embed models in the Bedrock console
- Store original text alongside each embedding — numbers alone are useless without the source text
- Chunk strategies: by equal size, by headers/sections (most common), by semantic meaning
- **RRF formula**: `score(d) = Σ 1/(k + rank_i(d))` — k=60 is standard, k=1 gives cleaner separation
- Re-ranker trick: assign each chunk a unique ID; ask Claude to return only IDs in relevance order (much faster than returning full text)
- **Contextual retrieval prompt**: "Generate a short snippet to situate this chunk within the overall document"

---

### Section: Features of Claude

**Key Concepts:**

**Extended Thinking:**
- Adds a reasoning step before the final response (Claude's internal monologue)
- Response includes two parts: `reasoningContent` (thinking) + `text` (final answer)
- Costs more (you pay for thinking tokens) and adds latency — only use when eval scores show you need it
- **Minimum budget**: 1024 tokens; tune with evaluations
- Signatures on reasoning content prevent tampering; redacted content is encrypted but still passable in follow-up turns

```python
additional_model_fields = {
    "thinking": {"type": "enabled", "budget_tokens": 2048}
}
```

**Image Support:**
- Up to 20 images per request; max 3.75 MB, max 8000×8000 px
- Token cost: `(width × height) / 750`
- Include image as a content part: `{"image": {"format": "png", "source": {"bytes": image_bytes}}}`
- **All prompt engineering techniques apply to vision** — structured prompts, step-by-step analysis, few-shot examples dramatically improve visual accuracy
- Real-world use case example: satellite imagery fire risk assessment for insurance

**Prompt Caching (Bedrock-specific implementation):**
- Saves Claude's preprocessing work for reuse in follow-up requests
- Cache lives for **5 minutes**; only useful for repeated identical content
- Initial request **writes** to cache; follow-up requests **read** from cache (cheaper + faster)
- Must add explicit **cache points** — not enabled automatically
- **Minimum 1024 tokens** before cache point to activate caching
- Cache points work in user messages, system prompts, and tool definitions
- Content before the cache point must be **100% identical** — even one character change invalidates the cache

```python
# Adding a cache point to a user message
user_message = {
    'role': 'user',
    'content': [
        {'text': long_system_content},
        {'cachePoint': {'type': 'default'}}  # Cache everything above this
    ]
}
```

**PDF Support:**
- Similar to image handling but uses `"document"` object type
- Required fields: `format: "pdf"`, `name: "filename_without_extension"`, `source: {"bytes": file_bytes}`
- Claude can extract, summarize, answer questions, handle multi-page PDFs with mixed content

**Citations:**
- Enable with `citations: {"enabled": True}` in document config
- Response includes citation data: source document, page numbers, exact text used
- Critical for trust in professional/research contexts — users can verify Claude's claims

---

### Section: MCP (Model Context Protocol)

**Key Concepts:**
- MCP is a communication layer that provides Claude with tools/context without requiring you to write all integration code yourself
- **MCP Servers** wrap external services (GitHub, AWS, databases) and expose pre-built tools, resources, and prompts
- **MCP Clients** (your app) connect to MCP servers via stdio, HTTP, WebSockets, etc.
- MCP is transport-agnostic; most common: client + server on same machine via stdio
- Key message types: `ListToolsRequest/Result` and `CallToolRequest/Result`

**Three MCP Primitives — who controls each:**

| Primitive | Controlled by | Use when |
|-----------|--------------|----------|
| **Tools** | Claude (model) | Give Claude capabilities to take actions |
| **Resources** | Your app code | Get data into UI or inject context into prompts |
| **Prompts** | User (explicit trigger) | Offer predefined high-quality workflows |

**Practical Tips & Code Patterns:**

```python
# MCP Server with Python SDK (FastMCP)
from mcp.server.fastmcp import FastMCP
from pydantic import Field

mcp = FastMCP("document-server")
documents = {"doc1": "content here..."}

@mcp.tool()
def read_doc_contents(doc_id: str = Field(description="ID of document to read")) -> str:
    """Reads the contents of a document by ID."""
    if doc_id not in documents:
        raise ValueError(f"Document {doc_id} not found")
    return documents[doc_id]

@mcp.tool()
def edit_document(
    doc_id: str = Field(description="Document to edit"),
    old_text: str = Field(description="Text to find"),
    new_text: str = Field(description="Replacement text")
) -> str:
    """Find-and-replace in a document."""
    documents[doc_id] = documents[doc_id].replace(old_text, new_text)
    return "Updated successfully"
```

```python
# Resources — expose data for app/UI consumption
@mcp.resource("docs://documents")
def list_documents():
    """List all available documents."""
    return list(documents.keys())

@mcp.resource("docs://documents/{doc_id}")
def get_document(doc_id: str):
    """Get a specific document by ID."""
    return documents.get(doc_id, "Not found")
```

```python
# Prompts — pre-built workflows users can trigger
@mcp.prompt()
def format_document(doc_id: str) -> list:
    """Prompt to reformat a document into markdown."""
    return [{"role": "user", "content": f"Reformat document {doc_id} into clean markdown using available tools."}]
```

- **Use `mcp dev mcp_server.py`** to launch the built-in browser inspector for testing tools without a full app
- MCP tool schemas differ from Bedrock's expected format — use a `to_bedrock_tools()` conversion function
- In production, you implement either a client OR a server, not both
- **Resources** are ideal for: autocomplete lists, document pickers, read-only reference data
- **Prompts** are ideal for: slash commands, workflow buttons, battle-tested domain-specific instructions

---

## Top 20 Practical Tips (Consolidated)

1. **Use inference profile IDs** (not model IDs) to avoid regional availability errors in Bedrock. Find them under "Cross-region inference" in the console.

2. **Claude has no memory** — always send the full conversation history with every request. Build helper functions (`add_user_message`, `add_assistant_message`) to manage the message list.

3. **Prefill + stop sequences = clean structured output**: start the assistant message with ` ```json ` and set stop sequence to ` ``` ` to extract raw JSON/code without preamble.

4. **Temperature is a dial, not a toggle**: use 0.0–0.3 for code/data extraction, 0.7–1.0 for creative tasks. Default is 1.0.

5. **System prompts should give Claude a role**, not a list of rules. "You are an AWS cloud support specialist" beats a 20-bullet instruction list.

6. **Never test a prompt just once** — build an evaluation pipeline with at least 10–20 test cases before shipping to production.

7. **Always ask model graders for reasoning alongside the score** — without it, models default to middling scores (~6). Ask for strengths + weaknesses + reasoning + score.

8. **Combine model graders + code graders** for code-generation tasks: `final_score = (model_score + syntax_score) / 2`.

9. **Generate test datasets with Claude Haiku** — it's fast and cheap, and the generated cases are diverse enough to catch real edge cases.

10. **Start prompts with an action verb**: "Generate," "Write," "Create," "Identify." The first line is the most important part of any prompt.

11. **Be specific with guidelines**: list exact output requirements (caloric totals, portion sizes in grams, meal timing). Specificity doubled eval scores in the course examples.

12. **Use XML tags** when interpolating large data blocks or multiple content types — they prevent Claude from confusing instructions with data.

13. **Tool descriptions matter as much as the function**: write 3–4 sentences covering what it does, when to use it, what it returns, and valid input examples. If stuck, ask Claude to write them.

14. **RAG = chunk → embed → store → query → retrieve → inject**. Store original text alongside embeddings — embeddings alone are useless.

15. **Hybrid search beats pure vector search**: combine semantic embeddings with BM25 lexical search for exact ID/keyword matches, merge with Reciprocal Rank Fusion.

16. **Contextual retrieval** improves RAG accuracy by pre-processing each chunk with Claude to add a short context snippet about its role in the full document.

17. **Prompt caching requires 1024+ tokens** before the cache point and 100% identical preceding content. Cache system prompts and tool lists — they rarely change.

18. **Extended thinking is a last resort** — optimize prompts and evaluate first. Only add extended thinking if eval scores are still insufficient after prompt work.

19. **Apply prompt engineering to images too** — structured step-by-step analysis instructions dramatically improve vision accuracy over simple "what do you see?" prompts.

20. **MCP primitive selection**: tools = let Claude decide when to act, resources = your app fetches data for UI/context, prompts = user triggers predefined workflows via slash commands.
