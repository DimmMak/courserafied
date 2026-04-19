# 📜 courserafied Changelog

## v0.2.0 — 2026-04-18

**World-Class Overhaul shipped.** Part of the fleet-wide upgrade to tree+plugin+unix architecture.

- 🌳 **Tree:** `domain:` field added to frontmatter (learning)
- 🎮 **Plugin:** `capabilities:` block declares reads / writes / calls / cannot
- 🐧 **Unix:** `unix_contract:` block declares data_format / schema_version / stdin_support / stdout_format / composable_with
- 🛡️ Schema v0.3 validation required at install (via `future-proof/scripts/validate-skill.py`)
- 🔗 Install converted to symlink pattern (kills drift between Desktop source and live install)
- 🏷️ Tagged at `v-2026-04-18-world-class` for rollback

See `memory/project_world_class_architecture.md` for the full model.

---


## v2.0.0 — 2026-04-17 — Multi-course via course-name-first invocation

### 🔧 Breaking change
- Course name is now the FIRST argument after every subcommand (except `list`).
  ```
  OLD: /courserafied ingest [transcript]
  NEW: /courserafied ingest {course-name} [transcript]
  ```
- Old single-folder auto-detection still works as fallback when only ONE course folder exists in CWD. Multi-course workflows now unambiguous.

### 🚀 Added
- 4-case routing logic (explicit arg / single folder / multiple folders / none)
- Course-name normalization to kebab-case (accepts "IBM Data Analysis" → `ibm-data-analysis`)
- Levenshtein ambiguity check — warns on typos that would create near-duplicate folders
- New `/courserafied list` subcommand — shows all course folders in CWD
- Auto-init from `courses/_template/` on first ingest to an unknown course

### 💡 Why
Allows concurrent courses (Vanderbilt + IBM + MIT) with zero routing confusion. Eliminates the ambiguity when multiple course folders coexist.

### 🔗 Migration
Existing single-course workflows still work. Multi-course workflows gain explicit routing with no new state to manage.

---

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
