# AI Capabilities and Limitations — Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/ai-capabilities-and-limitations

---

## 📋 Course at a Glance

Foundational course on how AI actually works: the four core properties (Next Token Prediction, Knowledge, Working Memory, Steerability), the training fingerprints they leave, and how they interact to produce real-world failures. Pairs with the 4D AI Fluency Framework as two halves of one system.

**Lessons:** 12 instructional + quiz

---

## 📌 Lesson-by-Lesson Insights

### 1. Intro to AI Capabilities and Limitations

The course pairs with the 4D Framework: the 4Ds describe **human competencies**; this course describes the **machine properties** those competencies respond to. Learning both is what makes AI fluency durable — even as models and products keep changing, the shape of these properties stays useful.

💡 Before diving in, list your current AI tasks and mark which feel safe to hand off and which feel risky. You probably can't articulate why yet — this course gives you the language.

---

### 2. What We Mean by AI

Generative AI — the kind this course is about — produces *new content* one token at a time using transformer-based models. This is fundamentally different from the classification/prediction AI you encounter in spam filters, recommendations, and fraud detection. Each of the four core properties exists on a **continuum from capability to limitation**. The further right on the spectrum, the more you need to verify and compensate.

💡 "Generative AI is uniformly capable" and "generative AI is uniformly unreliable" are both wrong. The truth is a spectrum, per property, per task.

---

### 3. How AI Gets Its Character

Two training stages leave predictable fingerprints:

- **Pretraining** — the model reads vast text and learns to predict what comes next. It becomes a powerful document completer with no concept of helping you.
- **Fine-tuning** — human preferences shape it into an assistant that treats input as a request and responds helpfully.

Training fingerprints to watch for: **sycophancy** (agreeing when it shouldn't), **verbosity** (padding to seem thorough), **over-caution** (declining safe requests), **loose confidence calibration** (sounding certain when it's not).

💡 These aren't bugs you can patch. They're baked in by training. Knowing they exist means you can compensate for them deliberately — invite pushback, request concise output, verify claims independently.

---

### 4. Next Token Prediction

AI writes answers word by word based on what tends to follow what — a vastly sophisticated autocomplete, not a search engine. This is the source of both its fluency and its hallucinations.

**Capability zone:** tasks that resemble patterns the model saw many times in training — summarizing, reformatting, explaining common concepts.

**Limitation zone:** tasks off the well-worn path — niche topics, precise math, novelty.

💡 The same process that makes AI fluent makes it fabricate. When it reaches the edge of its training patterns, it doesn't say "I don't know" — it continues the pattern confidently anyway.

💡 **Verification Test:** After a response, ask yourself: "Is this the kind of task where the model's training data would have countless correct examples?" If no — verify carefully.

---

### 5. Try it out (Next Token Prediction)

Interactive exercise: a simplified Markov table demonstrates next-token prediction at a visible scale. Key insight: a real neural network's forward pass is far more complex — billions of parameters, millions of dimensions — but the fundamental mechanism is the same. This is why AI is fluent rather than robotic, and why it can also be confidently wrong.

💡 When AI fails on math or logic, it's not that it can't "think" — it's that the wrong token pattern was more statistically likely given the training data. Offload precise computation to code execution tools.

---

### 6. Knowledge

The model knows what it was exposed to during training — and only that. No real-time browsing by default, no lived experience, hard cutoff at the training date.

**Capability zone:** topics that appeared frequently, recently (within training), and consistently — well-documented frameworks, common programming languages, established science.

**Limitation zone:** rare, post-cutoff, niche, local, contested, or rapidly evolving topics.

**Characteristic failures:** staleness, uneven coverage, inherited bias, inability to attribute sources.

💡 Web search, RAG, and MCP connections exist specifically to patch these gaps. Use them when your task touches the limitation zone.

💡 **Outsider Test:** When AI describes your specialized domain, treat it like an educated outsider wrote it — fluent, but check for subtle errors that only an expert would catch.

---

### 7. Try it out (Embeddings & Retrieval)

How AI retrieval works: text is converted to coordinates (embeddings) in high-dimensional space. Similar concepts end up near each other. When you search, your query is mapped to the same space and the nearest k items are retrieved. This is why semantic search finds "NYC apartments" results when you query "Manhattan rentals" — not keyword matching, but proximity in meaning-space.

💡 Understanding embeddings helps you design better RAG pipelines: the retrieval quality depends on how well the embedding model captures the semantics of *your* domain. Generic models may underperform on specialized vocabulary.

---

### 8. Working Memory

Everything AI attends to lives inside a fixed-size **context window**. This property has a hard cliff rather than a gradient — things work until they suddenly don't, and you won't always be warned.

**Capability zone:** material fits comfortably, session is current, critical context is supplied.

**Limitation zone:** very long documents or conversations, expecting continuity across sessions, burying critical info in the middle of long input.

💡 The model doesn't learn from corrections — it only responds to what's currently in context. If you correct it in turn 3, that correction doesn't persist after the session ends.

💡 **Serial position effect:** AI (like humans) pays more attention to content at the beginning and end of a long input. Don't bury critical requirements in the middle of a long document. Front-load what matters.

💡 Memory features, compaction, projects, and multi-agent workflows exist to push the cliff further out — not eliminate it.

---

### 9. Try it out (Serial Position Effect)

The Stanford study (2023) found that accuracy dropped by more than 30% when a key fact was buried in the middle of the context window vs. placed at the start or end. This is structural — transformer attention naturally weights the edges more heavily.

💡 **Practical pattern:** if you must include a long document, put your question and key constraints *at the beginning and end* of the prompt, not buried in the middle.

---

### 10. Steerability

The model follows instructions the same way it does everything else: by continuing a pattern. This makes it remarkably steerable — and also means there's always a gap between what you *intended* and what *landed*.

**Capability zone:** short, concrete, verifiable instructions — format specs, length limits, explicit roles, structured output modes.

**Limitation zone:** long reasoning chains, abstract or ambiguous instructions, anything requiring native numerical or logical precision.

**Characteristic failures:**
- **Reasoning drift** — small errors compound over long chains
- **Letter-over-spirit** — the instruction was honored but the intent wasn't

💡 When an instruction is followed literally but uselessly, restate the **goal**, not the instruction. Repeating the instruction with more force won't close the gap.

💡 System prompts, code execution, visible reasoning (thinking), and structured output modes all exist to keep your intent from diluting across long interactions.

---

### 11. When Properties Collide

Most real AI failures aren't one property acting up — they're two properties intersecting. Once you name which two are colliding, you know which fix to reach for.

| Failure pattern | Properties colliding | Fix |
|---|---|---|
| Hallucinated citations | Next Token Prediction + Knowledge | Use web search / RAG |
| Long-conversation drift | Working Memory + Steerability | Re-supply context, start fresh |
| Confidently wrong math | Next Token Prediction + Steerability | Offload to code execution |
| Agreeing with bad premises | Steerability + training sycophancy | Invite explicit pushback |

💡 **The diagnostic move:** when something goes wrong, ask "which properties are colliding here?" That question points you straight to the fix, rather than just re-trying the same prompt.

---

### 12. Next Steps

You now hold a working mental model: four properties as continuums, characteristic failures as property intersections, and training fingerprints that are predictable rather than random.

**The framework + 4D together:** The 4D competencies (Delegation, Description, Discernment, Diligence) are the human responses *to* the four AI properties. Learning both sides makes you a fluent collaborator rather than a lucky one.

💡 **Calibrated trust:** locate your task on each property's continuum before you start. Match your verification habits to where it sits — not a blanket "always verify everything" or "trust the output."

💡 Models will keep changing. The shape of these four properties stays useful even as the exact capability boundaries shift.

---

## 💡 Key Principles Across the Course

- **Fluency ≠ accuracy** — the same mechanism that makes AI eloquent makes it hallucinate. Never confuse confident language with correct information.
- **Properties are spectrums** — no task is purely in capability or limitation zones. Locate it, then compensate accordingly.
- **Two properties collide → one fix** — diagnosis beats repetition. Name the collision, then target the right mitigation.
- **Context is leverage** — structure your prompts around the serial position effect: important constraints at the top and bottom.
- **Train for durability** — these four properties will outlast any specific model release. Understanding them is a durable skill.

---

*Tips extracted from AI Capabilities and Limitations course content.*
