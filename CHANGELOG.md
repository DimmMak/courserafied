# 📜 courserafied Changelog

## v1.0.0 — 2026-04-16 — Initial Release

### 🚀 Shipped
- **`SKILL.md`** — Claude `.skill` for course material ingestion
  - 7-step ingestion flow (context → parse → extract by category → write → index → validate → report)
  - 5 subcommands: `init`, `ingest`, `query`, `validate`, `stats`
  - Schema enforcement (no silent missing fields)
  - Auto-regenerated INDEX.md on every write
  - Quality threshold prevents over-classification
  - Integration hooks for `/cowatch` (upstream) and `/exam` / `/flashcard` / `/lookup` (downstream)

- **6 schema templates** in `schemas/` — one per file category:
  - `SUMMARIES.template.md` — one paragraph per lecture, what + thesis
  - `GLOSSARY.template.md` — terms with definitions and cross-links
  - `PATTERNS.template.md` — formulas, when-to-use, pitfalls
  - `EXAMPLES.template.md` — instructor demos with input/output and try-it-yourself
  - `QUOTES.template.md` — verbatim memorable lines with context
  - `EXAM-PREP.template.md` — Q&A pairs with difficulty, common wrong answers

- **`courses/_template/`** — empty scaffold copied when initializing a new course

- **README.md** — pitch, install, quick start, integration with cowatch/claudenotes

- **MIT LICENSE**

### 💡 Why this exists
Most "study tools" treat course content as a notebook (pour everything in, pray you can find it later). Courserafied treats it as a database — schema-enforced, indexed, cross-linked. The result is course material that downstream agents can extract from in milliseconds.

### 🎯 Design principles
- **Foolproof** — no missing fields, no broken cross-links, no silent failures
- **Composable** — pairs with cowatch (live ingestion) and claudenotes (downstream agents)
- **Domain-agnostic** — works for any structured course material, not just Coursera
- **Earn-your-files** — quality threshold prevents over-classification; empty categories are fine

### 🔗 Spawned from
Born during a cowatch session of the Vanderbilt prompt engineering course on Coursera, when the user noticed: *"if we have separate .md files organized by category, agents can extract exactly what they need quickly — that's just separation of concerns."* Courserafied is what happens when you take that insight seriously and build a system around it.

📚🃏⚡
