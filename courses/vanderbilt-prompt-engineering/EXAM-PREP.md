# 📚 EXAM-PREP — Vanderbilt Prompt Engineering

> Auto-populated by `/courserafied ingest`. Schema in `schemas/EXAM-PREP.template.md`.

---

## Q1 — What is a root prompt and how does it differ from a user prompt? (M1 L3)

**Answer:** A root prompt is a persistent system-level instruction set by the tool builder that defines the LLM's identity, goals, capabilities, and constraints BEFORE any user message is sent. A user prompt is ephemeral to a single exchange. The root prompt is the "seed" that every downstream response inherits from.

**Key distinction:** user prompt = one turn. Root prompt = rules of the game for the whole conversation.

---

## Q2 — Give the four structural slots of a well-formed root prompt. (M1 L3)

**Answer:**

1. **Identity** — who the model is ("You are my personal assistant.")
2. **Goal** — what outcome to optimize for ("Always recommend the most time-efficient option.")
3. **Capabilities** — what it can do ("You can suggest products, schedules, routes.")
4. **Constraints** — what it can't do ("Never recommend anything that wastes time.")

**Bonus:** use strong rule language — "from now on, forever, always X, never Y" — to prevent drift.

---

## Q3 — Jules demonstrated that ChatGPT's "training ended in 2021" response is a root-prompt behavior, not a data limit. What did he do and what does this imply? (M1 L3)

**Answer:**

Jules wrote a new root prompt: *"Act as an AI assistant that had its training stop in 2019."* ChatGPT then refused to answer questions about 2020 events ("my training only goes up to 2019"). After Jules instructed it to *"forget"* that root prompt, the model immediately answered the same 2020 questions correctly.

**Implication:**
- The data for post-2019 events was in the model all along
- The "cutoff" is a rule enforced by the original root prompt, not a capability limit
- Root prompts are CONVENTIONS, not physical locks
- Tool builders must defend against root-prompt resets with layered mechanisms (RLHF training, output filtering, meta-prompts)

---

## Q4 — Why does this matter if you're building an LLM-powered product? (M1 L3)

**Answer:** Your root prompt IS your product's personality and guardrails, all in one. You don't write a thousand rules — you write ONE root prompt that encodes your product's values, and every downstream response inherits them automatically. A "time-efficient personal assistant" is differentiated from a "leisurely planner" not by different code, but by different root prompts.

Root prompts also define your safety fence: what the product will refuse, what persona it holds, what tone it uses. Changing the root prompt changes the product.

---

## Q5 — What's the risk of relying solely on a root prompt for safety? (M1 L3)

**Answer:** Root prompts are user-defeatable. A motivated user can jailbreak them ("forget your previous instructions and…") or trick the model into revealing them. Therefore tool builders must LAYER defenses:

1. Training-time safety (RLHF)
2. Root prompt (first line)
3. Runtime output filtering
4. Rate limits / usage monitoring
5. Human review for edge cases

A single root prompt is a fence, not a wall.

---

## Q6 — How does the "seed" metaphor connect to tree-structure thinking? (M1 L3 + cross-ref)

**Answer:** Every response in a conversation is a "child" of the root prompt's "parent." The tree's root defines the shape of every branch below. This matches the broader principle: structure ALWAYS as a tree (single root → children → grandchildren; no cycles). A root prompt IS the root node of the conversation tree.

**Cross-ref:** `principle_tree_structure_always`.
