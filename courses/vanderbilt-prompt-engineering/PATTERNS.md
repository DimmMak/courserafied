# 📚 PATTERNS — Vanderbilt Prompt Engineering

> Auto-populated by `/courserafied ingest`. Schema in `schemas/PATTERNS.template.md`.

---

## Root Prompt Pattern (M1 L3)

**Formula:** `You are [ROLE]. Your goal is [GOAL]. You can [CAPABILITIES]. You cannot [CONSTRAINTS]. From now on, you will always [RULE]. You will never [ANTI-RULE].`

**What it does:** Establishes a persistent behavioral contract the model will follow for the entire conversation. Unlike a one-shot instruction, a root prompt is the SEED — every downstream response inherits its constraints and tone.

**Structure (the 4 slots Jules identified):**

| 🟣 Slot | 🟣 What to specify | 🟣 Example |
|---|---|---|
| 1. Identity | Who the model is | "You are my personal assistant." |
| 2. Goal | What outcome to optimize | "Always recommend the most time-efficient option." |
| 3. Capabilities | What it can do | "You can suggest products, schedules, routes." |
| 4. Constraints | What it can't do | "Never recommend anything that wastes time." |

**Strength-of-rule language:**
- Weak: "try to save time" (model may drift)
- Strong: "from now on, forever, always prioritize time-efficient recommendations" (sticks)
- Strongest: "never do X under any circumstance" + "always do Y when Z" (pair positive + negative)

**Applications:**
- Building a product: design root prompt to create your differentiated assistant (the "time-efficient concierge" vs the "slow leisurely planner")
- Overriding constraints (for testing): reset the model's training-cutoff or capability fences to see what's gated by root prompt vs architecture
- Composing assistants: chain specialized root prompts — "time-efficient travel planner" inside "personal life assistant"

**Jules's demo (paraphrased):**
> "You are my personal assistant. Whenever you provide output, think through what would be the most time-efficient recommendations. Only recommend things that will save me time. Do not suggest things that do not save me time."
>
> After this root prompt is set: every subsequent question (grocery shopping, buying a car) gets filtered through the time-efficiency lens automatically.

**Cross-links:**
- [Root Prompt definition](GLOSSARY.md#root-prompt-m1-l3)
- [Training cutoff reset example](EXAMPLES.md#training-cutoff-reset-m1-l3)
- [Time-efficient assistant example](EXAMPLES.md#time-efficient-personal-assistant-m1-l3)
