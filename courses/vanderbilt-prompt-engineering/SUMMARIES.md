# 📚 SUMMARIES — Vanderbilt Prompt Engineering

> Auto-populated by `/courserafied ingest`. Schema in `schemas/SUMMARIES.template.md`.

---

## Root Prompts — the hidden seed that shapes the whole conversation (M1 L3)

**Source:** Vanderbilt Prompt Engineering, Module 1 Lecture 3
**Speaker:** Jules White
**Duration:** ~7 min transcript

**Main thesis:** Every LLM tool you interact with (ChatGPT, Bing, Bard) has a hidden "root prompt" that defines ground rules for the model's behavior before the user ever types a message. The root prompt is the **seed** — it tells the model who it is, what its goals are, what it can and can't do, and these rules propagate through every downstream response. When you build your own LLM-powered tools, you can design root prompts to create specialized assistants (e.g., "always recommend the most time-efficient option"). You can also use root-prompt techniques to temporarily override or simulate constraints on existing tools — like convincing ChatGPT to act as if its training cut off in 2019 — revealing that the "I can't answer questions after 2021" response is itself a root-prompt behavior, not a hard limit.

**Cross-links:**
- [Root Prompt Pattern](PATTERNS.md#root-prompt-pattern-m1-l3)
- [Glossary entry: root prompt](GLOSSARY.md#root-prompt-m1-l3)
- [Training cutoff reset example](EXAMPLES.md#training-cutoff-reset-m1-l3)
