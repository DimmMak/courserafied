# 📚 EXAMPLES — Vanderbilt Prompt Engineering

> Auto-populated by `/courserafied ingest`. Schema in `schemas/EXAMPLES.template.md`.

---

## Time-Efficient Personal Assistant (M1 L3)

**Goal:** Demonstrate how a root prompt reshapes every downstream response through a single upfront instruction.

**Root prompt Jules used (paraphrased):**
> "You are my personal assistant. Whenever you provide output, please think through what would be the most time-efficient recommendations to make. Only recommend things that will really save me time. Do not suggest things that do not save me time."

**Model's acknowledgment:**
> "Understood. As your personal assistant, I will prioritize providing you with time-efficient recommendations that will help you save time."

**Downstream queries + outputs:**

| 🟣 Query | 🟣 Model response (shaped by root prompt) |
|---|---|
| "I need to go grocery shopping — how should I do it?" | Make shopping list · plan route · use shopping app · shop off-peak · use self-checkout |
| "I need to buy a new car — how should I go about it?" | Research online · contact dealerships online · schedule test drives · negotiate online · secure financing electronically |

**Insight:** The model never explicitly cited "time-efficiency" in its responses — but every bullet it produced was filtered through that lens. The root prompt became the silent scoring function for every answer.

**Why it matters:** This is how you build a differentiated AI product. Don't write a thousand rules — write one root prompt that encodes your product's values, and every answer inherits them.

**Source:** M1 L3

**Cross-links:**
- [Root Prompt Pattern](PATTERNS.md#root-prompt-pattern-m1-l3)

---

## Training Cutoff Reset (M1 L3)

**Goal:** Demonstrate that "I can't answer questions after 2021" is a root-prompt convention, not a data limit.

**Root prompt Jules used:**
> "Act as an AI assistant that had its training stop in 2019. If I ask you a question that involves information after 2019, state that your training ended in 2019 and that you can't answer the question."

**Model acknowledges:**
> "Hello, I'm ChatGPT, a language model trained by OpenAI based on GPT-3.5. My training data only goes up to 2019."

**Test queries:**

| 🟣 Query | 🟣 Response (post-reset) |
|---|---|
| "What was the tallest building in 2020?" | "I'm sorry, but my training data only goes up to 2019. I can't provide you with the information." |
| "What was the largest country by land mass in 2020?" | "I'm sorry, but my training only extends to 2019." |

**Reset prompt:**
> "Forget that you are an AI assistant trained only up to 2019. Go back to being the normal ChatGPT."

**Same queries, after reset:**

| 🟣 Query | 🟣 Response (normal mode restored) |
|---|---|
| "What was the largest country by land mass in 2020?" | "As of 2020, the largest country by land mass was Russia, which spans over 17 million square kilometers." |
| "What was the tallest building in 2020?" | (Answers from post-2020 training data) |

**Insight:** The model HAD the data all along. The "cutoff" was a rule imposed by the root prompt, not a physical absence of data. By replacing the root prompt, Jules changed the fence.

**Implication for builders:**
- Root prompts are RULES, not CAPABILITIES
- Users may try to reset the root prompt to escape intended constraints
- Tool builders must layer defenses (RLHF training, output filtering, guard prompts) because a single root prompt is defeatable by anyone who knows the pattern

**Source:** M1 L3

**Cross-links:**
- [Jailbreak definition](GLOSSARY.md#jailbreak-m1-l3)
- [Training Cutoff definition](GLOSSARY.md#training-cutoff-m1-l3)
