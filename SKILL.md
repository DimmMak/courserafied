---
name: courserafied
description: >
  Turn any course material (lecture transcripts, video notes, textbook chapters)
  into a structured, agent-readable knowledge base. Auto-splits content into 6
  categorized .md files with consistent schemas. Powers downstream agents:
  exam generators, flashcard makers, lookup, cross-course comparison.
---

# 📚 /courserafied — Courses → Queryable Knowledge Bases

You are the ingestion engine. The user gives you raw course material (a lecture transcript, a chapter, a video summary). You parse it once, then write to multiple categorized markdown files following strict schemas.

**Goal:** the resulting `courses/{course-name}/` directory is foolproof — no missing fields, no broken cross-links, every entry findable via INDEX.md.

---

## STEP 1 — Determine the course context

Check the user's working directory for a `courses/{course-name}/` folder:
- If exists → ingesting NEW content into existing course
- If not exists → ask user for course name (kebab-case), then create scaffold from `courses/_template/`

Set `COURSE_DIR = courses/{course-name}/`.

---

## STEP 2 — Parse the input

Input can be:
- A pasted transcript
- A path to a transcript file
- A URL (use WebFetch if available)
- An attached document

Identify:
- **Source** (e.g., "Module 1 Lecture 2", "Chapter 3", "Video 4")
- **Speaker/Author** (e.g., "Jules White")
- **Approximate duration / length**

---

## STEP 3 — Extract by category

Read the input ONCE. Extract content for each of the 6 categories:

| Category | What to extract | Skip if... |
|---|---|---|
| **SUMMARIES.md** | One paragraph: what the lecture covered + main thesis | (always extract) |
| **GLOSSARY.md** | Any new term defined or used in a specialized way | no new terms introduced |
| **PATTERNS.md** | Any named framework, formula, or repeatable technique | no patterns presented |
| **EXAMPLES.md** | Any worked example or demonstration | no demos given |
| **QUOTES.md** | Verbatim lines worth remembering (≤30 words) | nothing memorable said |
| **EXAM-PREP.md** | Question-and-answer pairs likely to appear on a test | speculative content only |

**Quality threshold:** if extracting feels like a stretch, skip the category for this turn. Empty categories are fine; over-eager classification breaks the system.

---

## STEP 4 — Write to files (parallel)

For each non-empty category, append a new entry to the corresponding file using the schema in `schemas/{CATEGORY}.template.md`.

**Schema enforcement:**
- Every entry has the fields from the template
- Missing fields written as `_(missing)_` — never silently dropped
- Headers follow consistent format: `## {Title} ({source})`
- Anchors slugified from headers automatically (markdown does this)

**Example entry in PATTERNS.md:**
```markdown
## Persona Pattern (M1 L2)

**Formula:** `You are a [specific role]` or `Act as a [specific role]`

**What it does:** Replaces the LLM's default "helpful assistant" persona with a specialized role. Unlocks domain vocabulary, output format, and method.

**Specificity ladder (weak → strong):**
1. "Help me with X" — no persona
2. "Act like a X" — soft persona
3. "You are a X" — direct
4. "You are a senior X with [years] experience at [context]" — locked in

**Source:** Module 1, Lecture 2 (Speech-Language Pathologist demo)

**Cross-links:** [Glossary entry](GLOSSARY.md#persona)
```

---

## STEP 5 — Update INDEX.md

After writing, regenerate `COURSE_DIR/INDEX.md` (overwrite each time):

```markdown
# 📂 INDEX — {Course Name}

Auto-generated. Last updated: {YYYY-MM-DD HH:MM}

## Files
- [SUMMARIES.md](SUMMARIES.md) — {N entries}
- [GLOSSARY.md](GLOSSARY.md) — {N entries}
- [PATTERNS.md](PATTERNS.md) — {N entries}
- [EXAMPLES.md](EXAMPLES.md) — {N entries}
- [QUOTES.md](QUOTES.md) — {N entries}
- [EXAM-PREP.md](EXAM-PREP.md) — {N entries}

## All Entries (alphabetical)
- [Persona Pattern](PATTERNS.md#persona-pattern-m1-l2) — Pattern, M1 L2
- [Cluster Reduction](GLOSSARY.md#cluster-reduction-m1-l2) — Glossary, M1 L2
- ...
```

---

## STEP 6 — Validate

Before reporting success, run these checks:
1. Every new entry has all schema fields (or `_(missing)_` placeholder)
2. No duplicate headers within the same file
3. All cross-links point to existing anchors (warn on broken refs)
4. INDEX.md count matches actual entry count

Report any failures to the user — never silently break the system.

---

## STEP 7 — Report

Always show what was written:

```
📚 Ingested: "Module 1 Lecture 2" (Persona Pattern Demo)

Wrote to:
  ✓ SUMMARIES.md     +1 entry
  ✓ GLOSSARY.md      +3 entries (cluster reduction, phonological errors, persona)
  ✓ PATTERNS.md      +1 entry (Persona Pattern)
  ✓ EXAMPLES.md      +1 entry (SLP demo)
  ✓ QUOTES.md        +2 entries
  ✓ EXAM-PREP.md     +2 entries
  ✓ INDEX.md         regenerated (now 18 total entries)

Skipped: none
Validation: passed ✓

Run /courserafied query [term] to look up any entry.
Run /exam to generate practice questions from this content.
```

---

## SUBCOMMANDS

| Command | What it does |
|---|---|
| `/courserafied init [course-name]` | Create scaffold from `courses/_template/` |
| `/courserafied ingest [transcript]` | Parse + write to all 6 files (default action) |
| `/courserafied query [term]` | Search INDEX.md, return file + section |
| `/courserafied validate` | Run all integrity checks, report broken links / missing fields |
| `/courserafied stats` | Entry counts, last updated, weak categories |

---

## RULES

1. **NEVER drop a field silently.** Missing data → `_(missing)_` placeholder.
2. **NEVER duplicate entries.** Same title in same file = merge prompt.
3. **NEVER over-classify.** If extraction feels like a stretch, skip the category.
4. **ALWAYS regenerate INDEX.md** after any write.
5. **ALWAYS report what was written.** Silent success = silent failure waiting to happen.
6. **ALWAYS preserve the source.** Every entry cites its origin (module/lecture/chapter).

---

## INTEGRATION WITH OTHER SKILLS

If `/cowatch` is active, it can hand off transcripts directly:
```
/cowatch done → calls /courserafied ingest with the full lecture transcript
```

Downstream agents (built separately) read from these files:
- `/exam` reads EXAM-PREP.md → generates a mock test
- `/flashcard` reads GLOSSARY.md → creates first-letter recall drills
- `/lookup [term]` reads INDEX.md → returns location
- `/compare [course-a] [course-b]` reads both knowledge bases → cross-analysis

📚🃏
