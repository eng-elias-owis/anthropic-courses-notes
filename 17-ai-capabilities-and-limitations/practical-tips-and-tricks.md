# AI Capabilities and Limitations — Practical Tips & Tricks

**Course URL:** https://anthropic.skilljar.com/ai-capabilities-and-limitations

---

## 📋 Course at a Glance

Foundational course explaining how AI actually works: next-token prediction, knowledge, working memory, steerability, and how the four properties interact.

**Lessons:** 12 instructional + quiz

---

## 🔑 Key Takeaways by Lesson

### 1. Intro to AI Capabilities and Limitations

- **Explain why this material is durable even as models and products keep changing** — See how the Capabilities & Limitations framework and the 4D Framework work together
- **Welcome to the AI Capabilities & Limitations Course** — (4 minutes) The 4D Framework teaches YOU how to collaborate with AI. This course teaches you how AI is able to work with you.
- **Key takeaways** — The AI Fluency Framework (4Ds) describes human competencies. This course describes the machine properties those competencies respond to.
- **Exercise: Mapping Your Current AI Use** — Why? This is the foundation for every exercise that follows in this course.
- **Lesson reflection** — Which of your listed tasks felt "safe" to hand to AI, and which felt risky? Can you articulate why yet?

### 2. What We Mean by AI

- By the end of this lesson you'll be able to: Distinguish generative AI from the classification and prediction AI you already encounter daily Understand that generative AI's properties exist on a continuum from capability to limitation Preview the four core properties you'll explo
- **What we mean by generative AI** — (4 minutes) Most AI in the world (spam filters, recommendations, fraud detection) isn't generative. This course is about the kind that is: transformer-based text models that produce new content one token at a time.
- **AI Capabilities & Limitations Framework** — Four properties that shape what AI can and can't do for you. Each sits on a spectrum — the further right, the more you should verify and compensate.
- **Steerability** — How much am I in control?
- **Key takeaways** — Generative AI produces new content rather than classifying existing content. AI isn't uniformly capable or uniformly unreliable.

### 3. How AI Gets Its Character

- By the end of this lesson you'll be able to: Explain the two-stage training process for generative AI (pretraining and fine-tuning) in plain language Recognize the behavioral fingerprints each stage leaves: sycophancy, verbosity, over-caution, loose confidence calibration Apply t
- **How AI gets its character** — (5 minutes) An AI's politeness, helpfulness, and caution aren't emergent magic. They're trained in, layer by layer, and each training stage leaves specific, predictable fingerprints on how the system interacts with you.
- **How AI Gets Its Character** — Two training stages turn raw prediction into the helpful assistant you interact with — and each stage leaves fingerprints on its behavior.
- **Pretraining** — The model reads vast amounts of text and learns one thing: predict what comes next. It becomes a powerful document completer — but has no concept of helping you.
- **Fine-tuning** — Human preferences shape the document completer into an assistant — one that treats your input as a request, answers helpfully, and declines harmful asks. Help me improve this paragraph.

### 4. Next Token Prediction

- By the end of this lesson you'll be able to: Explain Next Token Prediction as the core mechanism of generative AI and why it produces both fluency and hallucination Locate tasks on the Next Token Prediction continuum (well-worn path vs.
- **How AI models use next token prediction** — (4 minutes) Generative AI is closer to a vastly sophisticated autocomplete than to a search engine. It writes answers word by word based on what tends to follow what.
- **BEFORE YOU READ** — You ask AI to summarize a long report. How closely do you need to check the result?
- **Key takeaways** — Next Token Prediction refers to the fact that generative AI writes answers word by word based on what tends to follow what. Capability zone: tasks that resemble patterns the model has seen many times (summarizing, reformatting, explaining common concepts).
- **Exercise: The Verification Test** — Why? You now know that the same generative process that makes AI fluent is the one that makes it fabricate.

### 5. Try it out

- The Markov table lookup is a really simple and explainable operation. A forward pass through a neural network is quite a bit more complex.

### 6. Knowledge

- By the end of this lesson you'll be able to: Explain how an AI model's knowledge is formed during training and why it has a fixed cutoff Predict which topics sit in the capability zone (frequent, recent-in-training, consistent) versus the edge (rare, post-cutoff, niche, contested
- **Understanding knowledge gaps in AI models** — (5 minutes) The model knows what it was exposed to during training, and only that. No real-time browsing by default, no lived experience, and a hard stop at the knowledge cutoff.
- **BEFORE YOU READ** — You ask AI to explain a news event from last week. How closely do you need to check the result?
- **Key takeaways** — What generative AI knows comes entirely from training data and is frozen at the knowledge cutoff. Without tools, it has no access to any information after that date.
- **Exercise: The Outsider Test** — Why? You know the model's knowledge is broad but frozen, shaped by whatever was in its training data.

### 7. Try it out

- For decades, that was search, returning results based on string similarity rather than meaning. Google continuously made incremental improvements with engineering: Synonym dictionaries mapped "car" to "automobile," Stemming rules connected "running" to "run," and click-pattern mining surfaced that people who search "NYC apartments" want the same results as "Manhattan rentals.
- **Encoding** — Let's start with a simplified example. Imagine you were to score every document in a corpus of knowledge on two dimensions: how much it relates to dinosaurs, and how much it relates to roller coasters.
- **An entire encyclopedia** — click to select You've just mapped meaning in 2D space, plotting our collection of items based on what they're about.
- **Retrieval** — Now let's search this space. Plot a question on the same graph with the same axes.
- **QUESTION** — ❓ "What's the best dinosaur-themed roller coaster?" click graph to place That's similarity search in a nutshell. We plot the question and find the nearest k items.

### 8. Working Memory

- By the end of this lesson you'll be able to: Explain the context window as a fixed-size container and what that implies for long documents, long conversations, and cross-session memory Recognize the "cliff" nature of this property compared to the gradual degradation of others App
- **How the context window affects generative AI outputs** — (6 minutes) Everything the AI is paying attention to lives inside a fixed-size workspace called the context window. It can attend to what's in there.
- **BEFORE YOU READ** — You ask AI to review a 50-page contract. How closely do you need to check the result?
- **Key takeaways** — Working Memory is the fact that the AI model has a fixed context window that it can attend to. Capability zone: your material fits comfortably, the session is current, you're supplying relevant context.
- **Exercise: The Before-and-After** — Why? Context is leverage.

### 9. Try it out

- Not always. There's a phenomenon that anyone who has crammed for an exam knows intuitively: there's a limit to how much you can hold in mind at once.
- **Memory Test** — You'll see 15 words, one at a time. Each appears for about 1.
- **The U-Shaped Curve** — What you just experienced has a name: the serial position effect. Psychologists have studied it for over a century.
- **Recency** — The fascinating part: large language models exhibit the same pattern. In 2023, researchers at Stanford tested what happens when you place a key fact at different positions within a long context window.
- **What This Means for Prompting** — If you paste a 20-page document into a prompt and ask a question about something on page 11, the model is more likely to miss it than something on page 1 or page 20. This has real implications for how you structure context.

### 10. Steerability

- By the end of this lesson you'll be able to: Explain why steerability works (fine-tuning taught the model instruction-following) and why it has limits (instructions are followed via pattern-matching, not understanding) Predict where control is tightest (short, concrete, verifiabl
- **How steerability affects generative AI outputs** — (5 minutes) The model follows your instructions the same way it does everything else: by continuing a pattern. That makes it remarkably steerable.
- **BEFORE YOU READ** — You ask AI to write exactly 100 words, no more. How closely do you need to check the result?
- **Key takeaways** — Steerability means the model follows instructions via Next Token Prediction. Capability zone: short, concrete, verifiable instructions.
- **Exercise: The Goal Rewrite** — Why? The gap between what you say and what you mean is where most steerability failures live.

### 11. When Properties Collide

- **Recognize that most AI failures involve two or more properties interacting** — Diagnose common failure patterns (hallucinated citations, long-conversation drift, confidently wrong math, agreeable bad premises) by identifying which properties are at play
- **Diagnosing AI failures** — (3 minutes) The four properties don't operate in isolation. Most real failures are two of them intersecting.
- **Key takeaways** — Real-world failures are usually two properties interacting, not one.
- **Working Memory + Steerability (long-conversation drift)** — Naming the properties at play points you straight to the fix: verify specifics, re-supply context, offload to code execution, or invite pushback. This diagnostic move is Discernment applied.
- **Exercise: The Failure Diagnosis** — Why? Most real-world AI failures aren't one property acting up.

### 12. Next Steps

- By the end of this lesson you'll be able to: Synthesize the four properties and training fingerprints into a working mental model Connect the Capabilities & Limitations framework to the 4D Framework as two halves of one system
- **Applying the 4D framework to get better AI outputs** — (5 minutes) Fluent AI use isn't about memorizing every failure mode. It's about holding a small, clear model of the machine in your head, so that when something goes wrong you can recognize which kind of wrong it is and respond accordingly.
- **AI Capabilities & Limitations Framework** — Four properties that shape what AI can and can't do for you. Each sits on a spectrum — the further right, the more you should verify and compensate.
- **Steerability** — How much am I in control?
- **Key takeaways** — You now hold a working mental model: four properties as continuums, characteristic failures as property intersections. This framework and the 4D Framework are two sides of one system.

---

## 💡 Universal Tips for Working with AI

- **Start with the goal, not the prompt** — be clear about what outcome you want
- **Iterate** — first outputs are drafts, not final answers
- **Verify facts independently** — AI is confident but not always correct
- **Stay in the loop** — always review AI output before acting on it
- **Be transparent** — disclose AI use to collaborators and stakeholders
- **Match AI use to value** — delegate tasks where AI genuinely helps, not for its own sake

---

*Tips extracted from AI Capabilities and Limitations course content.*
