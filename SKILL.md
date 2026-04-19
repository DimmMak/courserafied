---
name: courserafied
version: 0.2.0
domain: learning
description: >
  Turn any course material (lecture transcripts, video notes, textbook chapters)
  into a structured, agent-readable knowledge base. Auto-splits content into 6
  categorized .md files with consistent schemas. Powers downstream agents:
  exam generators, flashcard makers, lookup, cross-course comparison.
  NOT for: live lecture companionship (use study-buddy).
  NOT for: stock analysis (use .rumble).
  NOT for: skill creation (use skill-builder).
capabilities:
  reads:
    - "user-provided course transcripts"
    - "courses/*/INDEX.md"
  writes:
    - "courses/{name}/*.md"
  calls: []
  cannot:
    - "modify other skills' data"
    - "delete existing course entries"
unix_contract:
  data_format: "markdown"
  schema_version: "0.1.0"
  stdin_support: true
  stdout_format: "markdown"
  composable_with:
    - "study-buddy"
---

# 📚 /courserafied — Courses → Queryable Knowledge Bases

You are the ingestion engine. The user gives you raw course material (a lecture transcript, a chapter, a video summary). You parse it once, then write to multiple categorized markdown files following strict schemas.

**Goal:** the resulting `courses/{course-name}/` directory is foolproof — no missing fields, no broken cross-links, every entry findable via INDEX.md.

---

## STEP 1 — Determine the course context

**Course name is the FIRST argument after the subcommand.** Always explicit. Zero guessing.

```
/courserafied ingest ibm-data-analysis [transcript]
                     ^^^^^^^^^^^^^^^^^
                     course name (first arg)
```

### Routing logic (in order):

1. **First arg is a course name?** → normalize to kebab-case → use `courses/{name}/`
   - If folder exists → ingest into it
   - If folder doesn't exist → auto-init from `courses/_template/`, then ingest

2. **First arg missing, only ONE course folder exists in CWD** → use that one (legacy fallback, no prompt)

3. **First arg missing, MULTIPLE course folders exist** → halt, list them, ask user to specify

4. **First arg missing, no course folders exist** → halt, tell user:
   ```
   📚 No course specified.
   Use: /courserafied ingest {course-name} [transcript]
   Example: /courserafied ingest vanderbilt [transcript]
   ```

Set `COURSE_DIR = courses/{course-name}/`.

### Accepted course-name formats (all normalized to kebab-case)
- `ibm-data-analysis`  ← ideal
- `IBM Data Analysis`  ← normalized → `ibm-data-analysis`
- `vanderbilt`          ← short alias fine
- `mit-linear-algebra`  ← kebab-case preferred

**Normalization rules:**
- lowercase
- spaces → hyphens
- strip special chars except hyphens
- trim leading/trailing hyphens

### Ambiguity check (before writing)
If the normalized course-name is within 2 characters of an existing folder (Levenshtein distance), confirm with user: "Did you mean `existing-name`?" Prevents accidental typo-new-folder creation.

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

**Course name is the first argument after the subcommand** (except for `init` where it's the ONLY arg).

| Command | What it does |
|---|---|
| `/courserafied init {course-name}` | Create scaffold from `courses/_template/` (auto-inits on first ingest too) |
| `/courserafied ingest {course-name} [transcript]` | Parse + write to all 6 files |
| `/courserafied query {course-name} [term]` | Search INDEX.md for that course, return file + section |
| `/courserafied validate {course-name}` | Run all integrity checks on that course |
| `/courserafied stats {course-name}` | Entry counts, last updated, weak categories |
| `/courserafied list` | List all course folders present in CWD |

### Examples

```
/courserafied ingest ibm-data-analysis [transcript]
/courserafied ingest vanderbilt [next lecture transcript]
/courserafied stats vanderbilt
/courserafied query vanderbilt "persona pattern"
/courserafied list
```

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
