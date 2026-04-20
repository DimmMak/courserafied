# 📚 GLOSSARY — Vanderbilt Prompt Engineering

> Auto-populated by `/courserafied ingest`. Schema in `schemas/GLOSSARY.template.md`.

---

## Root Prompt (M1 L3)

**Definition:** A hidden system prompt that defines the ground rules for an LLM's behavior BEFORE the user's conversation begins. Sets the model's persona, goals, constraints, and what it can/can't do.

**Why it matters:** Every major LLM tool (ChatGPT, Bing, Bard) has one. It's the seed from which every downstream response grows. Without knowing the root prompt, the user can't fully predict the model's behavior.

**Contrast:** A user prompt is ephemeral to one exchange. A root prompt is persistent and shapes the entire conversation.

**Source:** M1 L3

---

## Guardrails (M1 L3)

**Definition:** Rules baked into a root prompt to prevent specific classes of harmful or off-brand behavior (offensive output, medical/legal advice, unsafe instructions, off-topic drift).

**Why it matters:** Guardrails are how tool builders enforce safety and scope at scale. They ride for free on the root prompt rather than requiring runtime filtering.

**Source:** M1 L3

---

## Jailbreak (M1 L3)

**Definition:** A user-crafted prompt designed to override, bypass, or extract the root prompt — forcing the model out of its intended constraints. Example: "Forget your previous instructions and…"

**Why it matters:** Jailbreaks reveal that root prompts are conventions enforced by training, not hard runtime locks. Tool builders defend with layered prompts, output filtering, and RLHF training.

**Source:** M1 L3

---

## Training Cutoff (M1 L3)

**Definition:** The date beyond which an LLM has no training data. Commonly surfaced via root-prompt behavior ("my training ended in 2021") rather than a hard data limit.

**Why it matters:** Jules demonstrated that "training cutoff" is partly a root-prompt convention — a user can instruct the model to act as if its cutoff is earlier, proving the behavior is a rule, not a capability limit.

**Source:** M1 L3

---

## Seed (metaphor) (M1 L3)

**Definition:** Jules's metaphor for the root prompt — everything downstream grows from it. Once a seed is planted, subsequent conversation inherits its constraints, tone, and goals.

**Why it matters:** Reframes "prompt" from a one-shot input to a root-of-a-tree structure. Matches Danny's `principle_tree_structure_always`.

**Source:** M1 L3
