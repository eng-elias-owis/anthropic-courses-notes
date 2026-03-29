# Building with the Claude API — Course Notes

> Source: https://anthropic.skilljar.com/claude-with-the-anthropic-api
> Extracted: March 2026 (real text content from 65+ lessons)

## Course Overview

A comprehensive, hands-on course covering the full Claude API stack — from making your first API call to building production-grade agents. Topics include multi-turn conversations, system prompts, temperature, streaming, prompt evaluation, prompt engineering techniques (XML tags, few-shot examples, clarity), tool use, RAG with hybrid search, Claude features (extended thinking, vision, PDF, citations, prompt caching, code execution), Model Context Protocol (MCP), and agentic patterns (evaluator-optimizer, parallelization, chaining, routing).

---

## Section Notes

### Section: Accessing Claude with the API

**Key Concepts:**

- Claude has no built-in conversation memory — each API request is stateless. You must send the full conversation history on every call.
- The `client.messages.create()` function requires three key params: `model`, `max_tokens` (a safety ceiling, not a target), and `messages`.
- Two message roles: `"user"` and `"assistant"`. Messages are dicts with `role` and `content`.
- Extract the response text with `message.content[0].text`.
- System prompts customize Claude's persona/tone/constraints without being part of the conversation history. Pass them as a `system=` param.
- Temperature (0–1) controls output randomness. Low = deterministic (factual tasks, code); high = creative (brainstorming, marketing).
- Response streaming sends text chunks as they're generated — dramatically improves perceived latency for chat UIs.
- Assistant message **prefilling** + **stop sequences** = clean structured output (JSON, code, CSV) without extra commentary.
- API keys should always be stored in `.env` files (via `python-dotenv`), never hard-coded.

**Practical Tips & Code Patterns:**

- Install: `%pip install anthropic python-dotenv`
- Store key: `ANTHROPIC_API_KEY="your-key"` in `.env`, then `load_dotenv()` at startup.
- Basic client setup:
  ```python
  from anthropic import Anthropic
  client = Anthropic()
  model = "claude-sonnet-4-0"
  ```
- Multi-turn conversation helpers:
  ```python
  def add_user_message(messages, text):
      messages.append({"role": "user", "content": text})

  def add_assistant_message(messages, text):
      messages.append({"role": "assistant", "content": text})

  def chat(messages, system=None, temperature=1.0, stop_sequences=[]):
      params = {"model": model, "max_tokens": 1000,
                "messages": messages, "temperature": temperature}
      if system: params["system"] = system
      if stop_sequences: params["stop_sequences"] = stop_sequences
      return client.messages.create(**params).content[0].text
  ```
- Claude's API does **not** accept `system=None` — conditionally include it.
- Streaming (simplified SDK interface):
  ```python
  with client.messages.stream(model=model, max_tokens=1000, messages=messages) as stream:
      for text in stream.text_stream:
          print(text, end="")
      final_message = stream.get_final_message()  # for storage
  ```
- Prefilling + stop sequence for clean JSON:
  ```python
  add_assistant_message(messages, "```json")
  text = chat(messages, stop_sequences=["```"])
  clean = json.loads(text.strip())
  ```
- Temperature guide:
  - 0.0–0.3: factual responses, code, data extraction
  - 0.4–0.7: summarization, educational content, constrained creative writing
  - 0.8–1.0: brainstorming, creative writing, jokes

---

### Section: Prompt Evaluation

**Key Concepts:**

- Three paths after writing a prompt: (1) test once and ship — risky; (2) test a few times and tweak — still risky; (3) run through an evaluation pipeline with objective scoring — the right approach for production.
- A typical eval workflow has 5 steps: Draft prompt → Create eval dataset → Feed through Claude → Grade outputs → Iterate on prompt and repeat.
- Grader types: **code graders** (syntax validation, length checks), **model graders** (quality, instruction following, helpfulness), **human graders** (most flexible but slow).
- When asking for model-graded scores, always ask for `strengths`, `weaknesses`, and `reasoning` alongside a numeric score — otherwise models default to middling scores (~6).
- Use faster/cheaper models (e.g., Haiku) for dataset generation and grading to reduce cost and time.
- A code grader for code generation should check: format (only code, no prose), syntax validity (use `ast.parse`, `json.loads`, `re.compile`), and task following (model grader).
- Combine code + model scores (e.g., average them); adjust weights based on what matters most.

**Practical Tips & Code Patterns:**

- Generate test datasets with Claude itself:
  ```python
  # Ask Claude to generate an array of JSON test case objects
  # Use prefilling + stop sequences to get clean JSON back
  add_assistant_message(messages, "```json")
  text = chat(messages, stop_sequences=["```"])
  dataset = json.loads(text)
  ```
- Save dataset to file: `json.dump(dataset, open('dataset.json','w'), indent=2)`
- Core eval pipeline:
  ```python
  def run_eval(dataset):
      results = []
      for test_case in dataset:
          output = run_prompt(test_case)
          score = grade(test_case, output)
          results.append({"output": output, "test_case": test_case, "score": score})
      print("Avg:", mean([r["score"] for r in results]))
      return results
  ```
- Syntax validation helpers:
  ```python
  def validate_json(text):
      try: json.loads(text.strip()); return 10
      except: return 0

  def validate_python(text):
      try: ast.parse(text.strip()); return 10
      except SyntaxError: return 0
  ```
- Model grader: ask for `{"strengths": [...], "weaknesses": [...], "reasoning": "...", "score": N}` — structured output forces the model to justify its score.
- Low initial scores (2–4/10) are normal for naive prompts; use the score as a baseline, not a judgment.

---

### Section: Prompt Engineering Techniques

**Key Concepts:**

- **Clarity and directness**: The first line of your prompt is the most important. Use simple, unambiguous language and lead with action verbs (`Write`, `Generate`, `Identify`).
- **Specificity**: Provide output quality guidelines (length, format, elements) and/or step-by-step process instructions. Adding guidelines can more than double eval scores.
- **XML tags**: Use custom XML tags (e.g., `<athlete_information>`, `<my_code>`, `<docs>`) to create clear boundaries when mixing different content types or large data blocks.
- **Examples (few-shot prompting)**: Providing one or more input/output examples is one of the most effective techniques, especially for handling edge cases like sarcasm in sentiment analysis.
- Iterative improvement cycle: Set goal → Write initial prompt → Evaluate → Apply technique → Re-evaluate → Repeat.

**Practical Tips & Code Patterns:**

- Replace vague first lines like "I need help with..." with direct action lines: `"Generate a one-day meal plan for an athlete that meets their dietary restrictions."`
- Adding specificity example (guideline list):
  ```
  Guidelines:
  1. Include accurate daily calorie amount
  2. Show protein, fat, and carb amounts
  3. Specify when to eat each meal
  4. Use only foods that fit restrictions
  5. List all portion sizes in grams
  ```
  This change alone took an eval score from 3.92 → 7.86.
- XML tag structure for multi-variable prompts:
  ```xml
  <athlete_information>
  - Height: 6'2"
  - Weight: 180 lbs
  - Goal: Build muscle
  - Dietary restrictions: Vegetarian
  </athlete_information>
  Generate a meal plan based on the athlete information above.
  ```
- Few-shot example structure (always use XML tags):
  ```xml
  <sample_input>Oh yeah, I really needed a flight delay tonight! Excellent!</sample_input>
  <ideal_output>Negative</ideal_output>
  This example is negative because the tone is sarcastic.
  ```
- Use your **highest-scoring eval outputs** as few-shot examples — they're already validated as ideal.
- Process steps work best for: troubleshooting, decision-making, multi-angle analysis.
- Output quality guidelines work best for: almost every prompt.

---

### Section: Tool Use with Claude

**Key Concepts:**

- Tools extend Claude beyond its training data to access real-time or external information.
- Tool use follows a structured back-and-forth: user question + tool schemas → Claude requests tool → your server executes function → results sent back → Claude generates final response.
- When Claude wants to call a tool, `response.stop_reason == "tool_use"` and `response.content` contains both text blocks and `ToolUse` blocks.
- Each `ToolUse` block has a unique `id` that must be matched when returning `tool_result` blocks.
- Tool result blocks must go inside a `user` message with `type: "tool_result"`, `tool_use_id`, `content` (stringified), and `is_error`.
- Always include tool schemas in follow-up requests even when not expecting another tool call — Claude needs them to parse conversation history.
- Claude can request multiple tool calls in one response; handle all of them before sending results back.
- **Built-in tools**: text editor tool (`str_replace_editor`) and web search tool — you provide a schema stub; Anthropic handles the implementation.
- **Fine-grained tool calling**: add `fine_grained=True` to disable JSON validation buffering for faster streaming, but your code must handle partial/invalid JSON.

**Practical Tips & Code Patterns:**

- Tool function best practices: use descriptive names, validate inputs, raise errors with clear messages (Claude learns from error messages and may retry).
- Example tool function:
  ```python
  def get_current_datetime(date_format="%Y-%m-%d %H:%M:%S"):
      if not date_format:
          raise ValueError("date_format cannot be empty")
      return datetime.now().strftime(date_format)
  ```
- Let Claude generate JSON schemas for you: paste your function into Claude and ask it to write a schema following Anthropic's tool use docs.
- Use `ToolParam` type from `anthropic.types` for type safety.
- Multi-turn conversation loop with tools:
  ```python
  def run_conversation(messages, tools):
      while True:
          response = chat(messages, tools=tools)
          add_assistant_message(messages, response)
          if response.stop_reason != "tool_use":
              break
          tool_results = run_tools(response)
          add_user_message(messages, tool_results)
      return messages
  ```
- Tool router pattern:
  ```python
  def run_tool(tool_name, tool_input):
      if tool_name == "get_current_datetime":
          return get_current_datetime(**tool_input)
      elif tool_name == "add_duration_to_datetime":
          return add_duration_to_datetime(**tool_input)
      # add more tools here...
  ```
- Error handling in tool execution:
  ```python
  try:
      output = run_tool(tool_request.name, tool_request.input)
      block = {"type": "tool_result", "tool_use_id": tool_request.id,
               "content": json.dumps(output), "is_error": False}
  except Exception as e:
      block = {"type": "tool_result", "tool_use_id": tool_request.id,
               "content": f"Error: {e}", "is_error": True}
  ```
- Web search schema:
  ```python
  {"type": "web_search_20250305", "name": "web_search", "max_uses": 5,
   "allowed_domains": ["nih.gov"]}  # restrict to trusted sources
  ```
- Text editor schema (model-dependent):
  ```python
  {"type": "text_editor_20250124", "name": "str_replace_editor"}  # claude-3-7-sonnet
  ```

---

### Section: RAG and Agentic Search

**Key Concepts:**

- RAG (Retrieval Augmented Generation) breaks large documents into chunks and retrieves only the most relevant pieces for each query — solving context length, cost, and performance problems.
- Chunking strategies: **size-based** (simplest, always works, use overlap), **structure-based** (great for Markdown/HTML, requires structured docs), **sentence-based** (good middle ground), **semantic-based** (best quality, most expensive).
- Embeddings convert text to numeric vectors. Cosine similarity measures semantic relatedness (1 = identical, -1 = opposite).
- **BM25 lexical search** complements semantic search by finding exact term matches — crucial for IDs, technical terms, specific phrases.
- **Hybrid search** (semantic + BM25) with **reciprocal rank fusion (RRF)** produces better results than either approach alone.
- RRF formula: `RRF_score(d) = Σ(1 / (k + rank_i(d)))` — documents that rank well in multiple indexes rise to the top.
- A `Retriever` class that wraps multiple `SearchIndex` objects (same API: `add_document()`, `search()`) enables clean extensible hybrid search.

**Practical Tips & Code Patterns:**

- Simple size-based chunker with overlap:
  ```python
  def chunk_by_char(text, chunk_size=150, chunk_overlap=20):
      chunks = []
      start_idx = 0
      while start_idx < len(text):
          end_idx = min(start_idx + chunk_size, len(text))
          chunks.append(text[start_idx:end_idx])
          start_idx = end_idx - chunk_overlap if end_idx < len(text) else len(text)
      return chunks
  ```
- Structure-based chunking (Markdown):
  ```python
  def chunk_by_section(text):
      return re.split(r"\n## ", text)
  ```
- Sentence-based chunking with overlap:
  ```python
  sentences = re.split(r"(?<=[.!?])\s+", text)
  # group into chunks of max_sentences_per_chunk with overlap_sentences overlap
  ```
- RAG full pipeline: chunk → embed → store in vector DB → embed query → cosine similarity search → retrieve top-k chunks → build prompt with chunks → call Claude.
- When to use RAG vs. stuffing: use RAG for very large docs, multiple docs, or when you need to optimize cost/latency. Simple stuffing is fine for small docs.
- Poor chunking = poor RAG results. A medical doc containing the word "bug" can pollute software engineering search results.
- Cosine distance = `1 - cosine_similarity` (smaller = more similar, used in many vector DBs).

---

### Section: Features of Claude

**Key Concepts:**

- **Extended Thinking**: Claude's "scratch pad" for complex reasoning. Adds thinking blocks to the response. Costs more tokens and increases latency. Use only after standard prompting + prompt engineering fails to meet accuracy requirements. Minimum budget: 1024 tokens; `max_tokens` must exceed `thinking_budget`. Redacted thinking blocks (safety-triggered) can be passed back to Claude in subsequent turns.
- **Image support**: Up to 100 images per request; max 5MB each; 8000px max (single image) or 2000px (multiple). Tokens = (width × height) / 750. Send as base64 or URL inside image content blocks. Apply the same prompt engineering techniques (step-by-step instructions, few-shot) to get accurate visual analysis.
- **PDF support**: Nearly identical to image handling. Use `type: "document"`, `media_type: "application/pdf"`. Claude can extract text, images, charts, and table data from PDFs.
- **Citations**: Enable with `"citations": {"enabled": True}` and a `"title"` on the document block. Returns structured response with `cited_text`, `document_index`, `document_title`, `start_page_number`, `end_page_number`. Works with both PDFs and plain text. Builds user trust by showing exactly where each claim comes from.
- **Prompt caching**: Stores system prompts and fixed context in a KV cache (max 200K tokens, 5-minute TTL, LRU eviction). First request = cache creation cost; subsequent requests = significantly reduced cost. Cache is sensitive — changing even one character invalidates it.
- **Files API + Code Execution**: Upload files with Files API, reference by `file_id`. Code execution runs Python in an isolated Docker container (no network access). Combine them: upload CSV → ask Claude to analyze → Claude writes and runs code → downloads generated plots via Files API.

**Practical Tips & Code Patterns:**

- Enable extended thinking:
  ```python
  params["thinking"] = {"type": "enabled", "budget": thinking_budget}
  # max_tokens must be > thinking_budget; minimum budget is 1024
  ```
- Send image to Claude:
  ```python
  with open("image.png", "rb") as f:
      image_bytes = base64.standard_b64encode(f.read()).decode("utf-8")
  add_user_message(messages, [
      {"type": "image", "source": {"type": "base64", "media_type": "image/png", "data": image_bytes}},
      {"type": "text", "text": "What do you see?"}
  ])
  ```
- Send PDF:
  ```python
  {"type": "document", "source": {"type": "base64", "media_type": "application/pdf", "data": file_bytes}}
  ```
- Enable citations on a document:
  ```python
  {"type": "document", "source": {...}, "title": "earth.pdf", "citations": {"enabled": True}}
  ```
- Prompt caching for system prompt:
  ```python
  params["system"] = [{"type": "text", "text": system, "cache_control": {"type": "ephemeral"}}]
  ```
- Cache tool schemas (add to last tool to cache all preceding):
  ```python
  tools_clone = tools.copy()
  last = tools_clone[-1].copy()
  last["cache_control"] = {"type": "ephemeral"}
  tools_clone[-1] = last
  ```
- Code execution tool schema: `{"type": "code_execution_20250522", "name": "code_execution"}`
- Step-by-step image prompts massively outperform simple single-line questions for counting, analysis, and assessment tasks.
- Extended thinking is incompatible with message prefilling and temperature — check the full compatibility list in Anthropic docs.

---

### Section: Model Context Protocol (MCP)

**Key Concepts:**

- MCP is a communication layer that shifts the burden of tool definition and execution from your server to specialized MCP servers. Instead of writing every integration yourself, you connect to MCP servers that expose pre-built tools, prompts, and resources.
- MCP is transport-agnostic: stdio (local), HTTP, WebSockets, etc.
- Three MCP components: **Tools** (actions), **Prompts** (reusable message templates), **Resources** (data access, like GET endpoints).
- Message types: `ListToolsRequest/Result`, `CallToolRequest/Result`, `ReadResourceRequest/Result`, `ListPromptsRequest/Result`, `GetPromptRequest/Result`.
- The Python MCP SDK (`FastMCP`) auto-generates JSON schemas from type hints and Pydantic `Field` descriptors — no manual schema writing required.
- **Resources** come in two types: direct (static URI like `docs://documents`) and templated (parameterized like `docs://documents/{doc_id}`).
- **Prompts** are pre-built, tested message templates that give better results than ad-hoc user prompts. They can accept parameters and return lists of messages.
- In real projects you implement either a client OR a server, not both. Build both only to understand the protocol.

**Practical Tips & Code Patterns:**

- Initialize a FastMCP server:
  ```python
  from mcp.server.fastmcp import FastMCP
  mcp = FastMCP("DocumentMCP", log_level="ERROR")
  ```
- Define a tool with auto-generated schema:
  ```python
  @mcp.tool(name="read_doc_contents", description="Read the contents of a document.")
  def read_document(doc_id: str = Field(description="Id of the document to read")):
      if doc_id not in docs:
          raise ValueError(f"Doc with id {doc_id} not found")
      return docs[doc_id]
  ```
- Define resources:
  ```python
  @mcp.resource("docs://documents", mime_type="application/json")
  def list_docs() -> list[str]:
      return list(docs.keys())

  @mcp.resource("docs://documents/{doc_id}", mime_type="text/plain")
  def fetch_doc(doc_id: str) -> str:
      return docs[doc_id]
  ```
- Define a prompt:
  ```python
  from mcp.server.fastmcp import base

  @mcp.prompt(name="format", description="Reformat a document in Markdown.")
  def format_document(doc_id: str = Field(description="Id of the document")) -> list[base.Message]:
      return [base.UserMessage(f"Reformat document {doc_id} using markdown syntax...")]
  ```
- Test with the MCP Inspector: `mcp dev mcp_server.py` → open browser at the local URL → Connect → List Tools.
- Client core methods:
  ```python
  async def list_tools(self): return (await self.session().list_tools()).tools
  async def call_tool(self, name, input): return await self.session().call_tool(name, input)
  async def list_prompts(self): return (await self.session().list_prompts()).prompts
  async def get_prompt(self, name, args): return (await self.session().get_prompt(name, args)).messages
  async def read_resource(self, uri):
      result = await self.session().read_resource(AnyUrl(uri))
      resource = result.contents[0]
      if isinstance(resource, types.TextResourceContents):
          return json.loads(resource.text) if resource.mimeType == "application/json" else resource.text
  ```
- Add an MCP server to Claude Code: `claude mcp add [server-name] [command-to-start-server]`
- Popular MCP integrations: sentry-mcp, playwright-mcp, figma-context-mcp, mcp-atlassian, firecrawl-mcp-server, slack-mcp.

---

### Section: Anthropic Apps — Claude Code and Computer Use

**Key Concepts:**

- **Claude Code** is a terminal-based coding assistant with file operations, terminal access, web search, and MCP server support. It demonstrates agentic behavior in a real product.
- Install: `npm install -g @anthropic-ai/claude-code`, then run `claude`.
- `/init` command scans the codebase and generates `CLAUDE.md` — a context file automatically included in all future conversations. Multiple scopes: project (shared), local (personal), user (cross-project).
- Best workflow: (1) Feed context (show relevant files), (2) Ask for a plan (no code yet), (3) Implement the plan. Or use TDD: feed context → brainstorm test cases → implement tests → write code that passes them.
- **Computer Use** gives Claude a full desktop environment: browser, apps, visual navigation.
- Both tools exemplify key agent principles: tool integration, multi-step task execution, environmental interaction, autonomous problem-solving.

**Practical Tips & Code Patterns:**

- Use `#` in Claude Code to quickly add notes to `CLAUDE.md`: `# Always use descriptive variable names`.
- `/clear` resets conversation history and context.
- Separate "plan" from "implement" steps — Claude produces better code when it thinks before writing.
- MCP servers dramatically expand Claude Code's capabilities: add Sentry for bug detection, Playwright for browser testing, Jira/Confluence for ticket context, Slack for team notifications.

---

### Section: Agents and Workflows

**Key Concepts:**

- **Workflows** are a predetermined series of Claude calls to solve a specific, well-understood problem. Use when you can articulate the exact steps.
- **Agents** receive a goal and tools, then autonomously decide how to accomplish the goal. Use when the path is unknown or variable.
- Decision rule: *Do I know the path to the solution?* Yes → workflow. No → agent.
- **Evaluator-optimizer pattern**: Producer creates output → Grader evaluates → Feedback loop to producer → Repeat until grader approves. Ideal for iterative quality improvement (e.g., CAD modeling, code writing).
- **Parallelization**: Split a complex decision into multiple independent sub-tasks, run them simultaneously, aggregate results with a final Claude call. Each sub-task gets specialized criteria. Example: evaluate a part for metal, polymer, ceramic, etc. in parallel, then aggregate.
- **Chaining**: Sequential Claude calls where output of one feeds the next. Especially powerful for fixing long-prompt constraint violations: first generate, then revise.
- **Routing**: Classify user input and direct to the most appropriate specialized workflow. Use Claude itself as a classifier for nuanced routing.
- **Environment inspection**: Agents must be able to observe results of their actions. Claude needs screenshots after UI actions, file reads before file edits, API response validation after calls. Without inspection, agents are blind.
- Agents operate in an observe → think → act loop until goal is achieved or iteration limit is reached.
- Hybrid approaches (workflows with agent-like flexibility at certain steps) often work best.

**Practical Tips & Code Patterns:**

- Evaluator-optimizer loop skeleton:
  ```python
  while True:
      output = producer(input)
      grade = grader(output)
      if grade.accept:
          break
      input = grade.feedback  # feed critique back to producer
  ```
- Parallelization with Python `asyncio` or `ThreadPoolExecutor`:
  ```python
  import concurrent.futures
  materials = ["metal", "polymer", "ceramic", "composite", "elastomer", "wood"]
  with concurrent.futures.ThreadPoolExecutor() as executor:
      analyses = list(executor.map(lambda m: evaluate_for_material(image, m), materials))
  final = aggregate(analyses)
  ```
- Chaining for constraint adherence: first pass generates content, second pass fixes violations:
  ```
  Revise the article. Steps:
  1. Remove any text that identifies the author as AI
  2. Remove all emojis
  3. Replace clichéd writing with technical prose
  ```
- Routing classifier prompt pattern: ask Claude to classify the input into one of N categories, then branch on the result.
- Environment inspection examples: take screenshots after UI clicks, read file before writing, check API response status, run `whisper.cpp` to verify audio placement in generated video.
- Always define clear success criteria for agents — vague objectives lead to unpredictable behavior.
- Add iteration limits to agent loops to prevent infinite execution.

---

## Top 20 Practical Tips (Consolidated)

1. **Store API keys in `.env` files.** Never hard-code them. Add `.env` to `.gitignore`.

2. **Manually maintain conversation history.** Claude is stateless — send the full message list on every request. Build helper functions (`add_user_message`, `add_assistant_message`, `chat`) early and reuse them throughout your project.

3. **Use prefilling + stop sequences for structured output.** Prepend `"```json"` as an assistant message and add `"```"` to `stop_sequences` to get raw JSON without wrapper text. Works for Python, CSV, regex, or any structured format.

4. **Match temperature to your use case.** Low (0–0.3) for factual/code, medium (0.4–0.7) for balanced tasks, high (0.8–1.0) for creative output. Adding `temperature` as a param to your `chat()` function makes this trivial to tune.

5. **Enable streaming for chat UIs.** Users see text appear immediately instead of waiting 10–30 seconds. Use `client.messages.stream()` with `.text_stream` for text chunks and `.get_final_message()` for the complete response.

6. **Evaluate prompts systematically before shipping.** Build a dataset, run prompts through Claude, grade outputs (code grader + model grader), calculate an average score, and iterate. One-off testing misses edge cases that real users will definitely hit.

7. **Use Claude to generate eval datasets and JSON schemas.** Feed it your function signature and ask for a tool schema. Feed it your task description and ask for diverse test cases. This is faster and often more comprehensive than doing it manually.

8. **Start prompts with a clear, direct action verb.** `"Generate a one-day meal plan..."` beats `"What should this person eat?"`. This single change can double your eval score.

9. **Add specific output guidelines and/or step-by-step process instructions.** Tell Claude exactly what to include: calorie totals, macro breakdown, portion sizes. Specificity turns vague outputs into reliable, structured ones.

10. **Use XML tags to structure complex prompts.** Wrap user data, documents, and code in descriptive tags (`<athlete_information>`, `<my_code>`, `<docs>`). Tags prevent Claude from confusing instructions with content, especially with large data blocks.

11. **Use few-shot examples (one-shot or multi-shot) for edge cases.** Sarcasm detection, specific JSON formats, unusual output structures — showing is more reliable than telling. Always wrap examples in XML tags and explain why the output is ideal.

12. **Build a scalable tool router.** Map tool names to functions with `if/elif` chains. Add error handling that returns `is_error: True` tool result blocks so Claude can adapt. Adding new tools requires only two steps: add schema to list, add case to router.

13. **Always include tool schemas in follow-up requests.** Claude needs them to parse its own prior messages in conversation history, even when no further tool calls are expected.

14. **Use BM25 + semantic search (hybrid search) in RAG pipelines.** Semantic search misses exact terms (IDs, technical names); BM25 misses conceptual similarity. Reciprocal rank fusion combines both rankings fairly without needing to normalize scores.

15. **Choose chunking strategy based on your documents.** Structure-based (Markdown) > sentence-based (general text) > size-based with overlap (universal fallback). Always add overlap to size-based chunking to avoid losing context at boundaries.

16. **Enable prompt caching for repeated large system prompts or tool schemas.** Add `"cache_control": {"type": "ephemeral"}` to the last tool schema and the system prompt. Cache lasts 5 minutes (TTL), max 200K tokens. Even one character change invalidates the cache, so keep cached content stable.

17. **Use extended thinking only after exhausting standard prompt engineering.** It costs more tokens and adds latency. Set a minimum budget of 1024 tokens. Note: incompatible with prefilling and temperature.

18. **Enable citations for document-based Q&A applications.** Add `"citations": {"enabled": True}` and a `"title"` to your document block. This surfaces `cited_text`, page numbers, and document references — transforming Claude from a black box into a transparent research assistant.

19. **Separate workflows from agents.** Workflows = known path, defined steps, consistent results. Agents = unknown path, tool autonomy, adaptive behavior. Use workflows for predictable processes; use agents for exploration and open-ended tasks. Hybrid is often best.

20. **Give agents environment inspection tools.** Before modifying files, read them. After UI actions, take a screenshot. After API calls, check the response. Agents without observability can't detect failures, can't adapt, and can't verify task completion. Observability is not optional.
