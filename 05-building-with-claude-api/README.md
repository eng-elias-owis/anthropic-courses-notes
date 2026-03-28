# Building with the Claude API — Course Notes

> Source: https://anthropic.skilljar.com/claude-with-the-anthropic-api
> Extracted: March 2026 (real content — 85 lessons, 8.1 hours)

## Course Overview

A practical, end-to-end course on integrating Claude into real applications. Covers API basics, prompt design and evaluation, tool use, RAG, MCP servers, Claude's special features (extended thinking, caching, citations), and how to build agentic workflows. 85 lessons across 12 sections, 8 quizzes with a final assessment, and a certificate on completion.

---

## Section Notes

### Section 1: Introduction

**Key Concepts:**
- Course goal: make API calls, craft effective prompts, use tool use, implement RAG, set up MCP servers, build AI workflows and agents
- Overview of Claude models available through the API

**Practical Tips:**
- Understand which Claude model fits your use case before starting — they differ in capability, speed, and cost

---

### Section 2: Accessing Claude with the API

**Key Concepts:**
- **Minimum API request fields:** API key, model name, messages array, and max_tokens
- **Tokenization:** Claude breaks input text into tokens before processing — not characters or words
- **Multi-turn conversations:** Claude has no memory by default; you must send the full conversation history with each request
- **System prompts:** Used to define Claude's role, persona, or constraints before the user turn
- **Temperature:** Controls randomness — low (~0.0) for factual/deterministic tasks, high (~1.0) for creative tasks
- **Response streaming:** Streams tokens as they're generated, improving perceived UX for long responses
- **Structured data:** Claude can return clean JSON; use prefilled messages and stop sequences to eliminate surrounding text
- **API key security:** Never expose it in client-side code (JavaScript, mobile apps); always keep it server-side

**Practical Tips:**
- Store your API key on the server — never in frontend JavaScript or mobile app code
- For a factual Q&A app, use low temperature for consistent answers
- Use streaming to eliminate the "loading spinner + all-at-once" UX problem
- For clean JSON output with no extra text, combine a prefilled assistant message with stop sequences
- Use system prompts to shape Claude's behavior (e.g., "give hints, not answers" for a tutor)
- Always include the full conversation history in the `messages` array for multi-turn chats

---

### Section 3: Prompt Evaluation

**Key Concepts:**
- **Why evaluation matters:** Testing a prompt once and deploying is risky — users will provide unexpected inputs
- **Eval workflow:** Generate test cases → run the prompt → grade the outputs
- **Generating test datasets:** Use a faster/cheaper model (e.g., Claude Haiku) to generate test cases, not the model being tested
- **Model-based grading:** Use another AI model to assess response quality — ask for strengths, weaknesses, and reasoning alongside a score to get more than middle-range numbers
- **Code-based grading:** Programmatic checks for format validation, syntax correctness, expected output matching

**Practical Tips:**
- Never deploy a prompt tested on just one input — generate a diverse test dataset first
- Use Haiku to generate test cases (cheaper, faster) and save your primary model for evaluation
- Ask model graders for strengths, weaknesses, and reasoning — not just a score — for better signal
- Combine model grading (quality) with code grading (format/syntax) for comprehensive evaluation
- Treat prompt engineering and prompt evaluation as a loop: engineer → evaluate → improve → repeat

---

### Section 4: Prompt Engineering Techniques

**Key Concepts:**
- **Prompt engineering:** The practice of improving prompts to get more reliable, higher-quality outputs
- **Being clear and direct:** Start with an imperative verb and specific task (e.g., "Create a 30-minute workout plan for beginners")
- **Being specific:** Vague requests get vague answers — specify format, length, tone, constraints
- **XML tags for structure:** Use `<tags>` to separate sections of a prompt, especially when injecting large content blocks (documents, data, examples)
- **Providing examples (few-shot/multi-shot prompting):** Show sample input/output pairs to guide Claude's response format and reasoning style
- **Handling edge cases:** If Claude misses nuance (e.g., sarcasm), provide labeled examples demonstrating the edge case

**Practical Tips:**
- Lead with the action: "Write a product description for X" beats "I was wondering about product descriptions..."
- Use XML tags when your prompt includes large document content or multiple data sources — it adds structure and reduces confusion
- When Claude misclassifies edge cases (e.g., sarcastic text), add labeled examples rather than just telling it to "be more careful"
- One-shot or multi-shot prompting (providing examples) is one of the highest-leverage techniques
- Specificity > length — a shorter, more specific prompt outperforms a longer vague one

---

### Section 5: Tool Use with Claude

**Key Concepts:**
- **What tool use enables:** Claude can access real-time information and perform actions that go beyond its training data
- **Tool workflow:** Initial Request → Claude returns `tool_use` block → Your code executes the function → You send back `tool_result` → Claude gives Final Response
- **Detecting tool calls:** Check `stop_reason == "tool_use"` in the API response
- **Response structure:** Claude returns multi-block messages containing both text blocks and tool_use blocks
- **JSON schema for tools:** Defines what arguments a function expects — this is how Claude knows how to call it correctly
- **Batch tool (fine-grained tool calling):** Reduces back-and-forth when multiple tools need to be called — Claude can request several tools in one turn
- **Using multiple tools:** Claude can call different tools in sequence or in parallel depending on the task
- **Built-in tools:** The text edit tool and web search tool are provided by Claude — Claude provides the schema, but you may still need to implement functionality
- **Multi-turn with tools:** The conversation continues as long as `stop_reason` is `tool_use`; you keep appending tool results and calling the API

**Practical Tips:**
- Always check `stop_reason` after every API response — `"tool_use"` means Claude needs more data before finishing
- Define clear, descriptive JSON schemas for your tools — Claude uses them to understand what each tool does and what parameters it needs
- Use the batch tool to cut down round-trips when multiple tools are needed in one task
- For agents, prefer abstract tools (`read_file`, `write_file`, `run_command`) over overly specific ones — they give Claude more flexibility
- The built-in web search tool gives Claude access to real-time information without you building the integration

---

### Section 6: RAG and Agentic Search

**Key Concepts:**
- **RAG (Retrieval Augmented Generation):** Retrieve relevant documents at query time and inject them into the prompt, giving Claude access to knowledge beyond its training data
- **Text chunking strategies:** Documents must be split into chunks before embedding — chunk size and overlap strategy affect retrieval quality
- **Text embeddings:** Numerical vector representations of text; semantically similar texts have similar vectors, enabling similarity search
- **Full RAG flow:** Chunk documents → embed chunks → store in vector DB → at query time, embed query → find similar chunks → inject into Claude's prompt
- **BM25 lexical search:** A keyword-based search algorithm (complement to vector/semantic search); useful for exact term matching
- **Multi-index RAG pipeline:** Combining semantic (embedding) search with lexical (BM25) search in a hybrid pipeline improves retrieval accuracy

**Practical Tips:**
- Don't rely on semantic search alone — combine with BM25 lexical search in a multi-index pipeline for better retrieval
- Chunk size matters: too small loses context, too large adds noise — experiment based on your content type
- Use a separate embedding model for encoding (not Claude itself) — dedicated embedding models are optimized for this
- The RAG flow naturally extends Claude's knowledge without fine-tuning or retraining

---

### Section 7: Features of Claude

**Key Concepts:**
- **Extended thinking:** Claude internally reasons before answering; the response contains two parts — the reasoning/thinking process and the final answer
- **Image support:** Claude can analyze images passed in the API request (multimodal)
- **PDF support:** Pass PDFs as documents (type `"document"`, media_type `"application/pdf"`) — similar to images but for document content
- **Citations:** Claude can cite specific parts of source documents in its response, creating a traceable trail from answer to source
- **Prompt caching:** Cache large, repeated content (e.g., a system prompt or big document) to speed up requests and reduce costs
- **Rules of prompt caching:** Only certain positions in the prompt can be cached; cached content must appear consistently at the start
- **Files API:** Upload files ahead of time and reference them by ID in messages — avoids re-encoding large files on every request
- **Code execution:** Claude can execute code in a sandboxed environment via the API

**Practical Tips:**
- If you're making many requests with the same large system prompt or document, enable prompt caching — it reduces latency and cost significantly
- Use the Files API for large or frequently reused files instead of base64-encoding them in every request
- Use citations when building research or document Q&A apps — they make Claude's answers verifiable
- Extended thinking is useful for complex reasoning tasks where you want to see (or validate) Claude's reasoning chain
- For PDF analysis, use type `"document"` and media_type `"application/pdf"` — same pattern as images but different type fields

---

### Section 8: Model Context Protocol (MCP)

**Key Concepts:**
- **MCP:** A standardized protocol for connecting AI models to external tools and data sources; handles tool definitions and execution
- **MCP clients:** Applications that connect to MCP servers (e.g., Claude Desktop, custom apps)
- **MCP servers:** Expose tools, resources, and prompts to MCP clients
- **Defining tools:** Use the `@mcp.tool` decorator on a Python function — the SDK handles schema generation automatically
- **Resources:** Named, referenceable data sources exposed by the server (e.g., files, documents accessible via `@document_name`)
- **Prompts:** Pre-defined prompt templates exposed by the MCP server that clients can invoke
- **Communication:** During development, client and server communicate via stdio (standard input/output) on the same machine
- **MCP Inspector:** Browser-based tool for testing MCP server tools before connecting to Claude
- **Benefit over custom integration:** MCP handles tool definitions and execution for you — no need to build custom integrations from scratch

**Practical Tips:**
- Use the MCP Inspector in your browser to test tools before wiring up Claude — don't test in production
- Use the `@mcp.tool` decorator instead of writing JSON schemas manually — the Python SDK infers the schema from the function signature
- Use **Resources** to expose document/file content that clients can reference by name
- Use **Prompts** to expose reusable prompt templates from the server
- MCP's main value: reuse existing MCP servers (e.g., for GitHub) instead of building your own tool integrations

---

### Section 9: Anthropic Apps — Claude Code and Computer Use

**Key Concepts:**
- **Claude Code:** Anthropic's agentic coding tool — operates in the terminal, reads/writes files, executes code, and can be enhanced with MCP servers
- **Claude Code in action:** Demonstrates real coding workflows with Claude autonomously working through a project
- **Enhancements with MCP servers:** Claude Code can be connected to MCP servers to extend its capabilities (e.g., access to external APIs, databases, services)

**Practical Tips:**
- Enhance Claude Code with MCP servers to give it access to your specific tools and data sources
- Claude Code operates best when given clear task descriptions and access to the right tools via MCP

---

### Section 10: Agents and Workflows

**Key Concepts:**
- **Workflows vs. agents:** Workflows are structured, predictable multi-step pipelines; agents are more autonomous and dynamic — the choice depends on how much flexibility the task requires
- **Parallelization workflows:** Run multiple Claude calls simultaneously for independent subtasks (e.g., evaluate 4 material options in parallel)
- **Chaining workflows:** Sequential steps where each Claude call's output feeds into the next — useful when tasks have natural ordered stages or when a long prompt causes Claude to miss rules
- **Routing workflows:** Classify the input first, then route to a specialized pipeline (e.g., technical topics → educational pipeline, sports → entertainment pipeline)
- **Evaluator-Optimizer pattern:** Claude writes output → another Claude call evaluates it → if not good enough, it loops back for improvement
- **Agents and tools:** Agents use tools to interact with their environment; prefer abstract tools (`read_file`, `write_file`, `run_command`) for maximum flexibility
- **Environment inspection:** Agents can inspect their environment state to inform decisions

**Practical Tips:**
- Use **parallelization** when subtasks are independent and you want speed (e.g., evaluating multiple options simultaneously)
- Use **chaining** when Claude ignores rules in a long prompt — break into focused sequential steps
- Use **routing** when different input types need fundamentally different processing pipelines
- Use the **Evaluator-Optimizer** pattern when output quality is critical and iterative refinement is acceptable
- Give agents **abstract tools** (`read_file`, `write_file`, `run_command`) rather than overly specific ones — this maximizes flexibility for unexpected requests
- Choose workflows (predictable) over agents (autonomous) when reliability and control matter more than flexibility

---

### Section 11: Final Assessment

**Key Concepts covered:**
- All major course topics: API basics, system prompts, temperature, streaming, tool use workflow, prompt engineering techniques, prompt evaluation, MCP, agents and workflows
- Applied scenarios: math tutor bots, creative writing, product description generation, multi-source data analysis

---

### Section 12: Wrapping Up

**Key Concepts:**
- Course recap of all major skills: API calls, prompt crafting, tool use, RAG, MCP server setup, AI-powered workflows and agents

---

## Top 20 Practical Tips (Consolidated)

1. **Keep your API key server-side** — never put it in frontend JavaScript, mobile apps, or public repos
2. **Always send full conversation history** — Claude has no memory; include all prior messages in the `messages` array for multi-turn conversations
3. **Use low temperature (near 0.0) for factual tasks**, high temperature (near 1.0) for creative or unpredictable outputs
4. **Enable streaming** to eliminate the "spinner then wall of text" UX — tokens arrive progressively as Claude generates them
5. **Use system prompts** to define Claude's role, persona, or constraints (e.g., "act as a tutor — give hints, not answers")
6. **For clean JSON output**, combine a prefilled assistant message with stop sequences — don't just ask nicely
7. **Generate diverse test cases before deploying** a prompt — one successful test is not enough; users will find edge cases
8. **Use Haiku to generate test datasets**, not your production model — it's cheaper and fast enough for this task
9. **Ask model graders for strengths, weaknesses, and reasoning** — not just a score — to get actionable eval signal
10. **Use XML tags to structure complex prompts**, especially when injecting large documents or multiple data sources
11. **Provide labeled examples (few-shot prompting)** when Claude misses nuance — e.g., show sarcastic texts labeled as negative sentiment
12. **Be specific and direct** — start prompts with an action verb and concrete task description; vague prompts get vague responses
13. **Check `stop_reason == "tool_use"`** after every API call in a tool-use loop — keep calling the API until stop_reason is `"end_turn"`
14. **Define abstract tools** (`read_file`, `write_file`, `run_command`) for agents instead of overly specific ones — gives Claude flexibility for unexpected requests
15. **Use prompt caching** when making many requests with the same large system prompt or document — reduces latency and cost significantly
16. **Use the Files API** for large or frequently reused files — upload once, reference by ID instead of re-encoding every request
17. **Combine BM25 lexical search with semantic (embedding) search** in a multi-index RAG pipeline for better retrieval accuracy
18. **Use the MCP Inspector** to test your MCP server tools in the browser before connecting to Claude
19. **Break long prompts into chaining workflows** when Claude ignores rules — sequential focused steps beat one giant prompt
20. **Match the workflow pattern to the task**: use parallelization for independent subtasks, routing for categorically different inputs, and the Evaluator-Optimizer pattern when quality is critical and iteration is acceptable
