# 📚🃏 courserafied

> **Turn any course into a queryable knowledge base.** A Claude `.skill` that ingests lecture transcripts, textbook chapters, or video summaries — and produces a structured library of categorized markdown files that downstream agents can read to generate exams, flashcards, lookup answers, and cross-course comparisons.

```
RAW COURSE MATERIAL                      STRUCTURED KNOWLEDGE BASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Lecture transcript           ─┐         courses/{course-name}/
Video summary                 ├──→      ├── INDEX.md       (auto-generated)
Textbook chapter              │         ├── SUMMARIES.md
Notes from class              │         ├── GLOSSARY.md
Slide deck dump              ─┘         ├── PATTERNS.md
                                        ├── EXAMPLES.md
                                        ├── QUOTES.md
                                        └── EXAM-PREP.md
```

---

## 🎯 The big idea

Most "study tools" treat course content as a notebook — pour everything in, pray you can find it later.

**Courserafied treats course content as a database.** Every entry follows a schema. Every entry has a unique anchor. Every cross-link works. The result: agents can extract exactly what they need in milliseconds instead of scanning everything.

```
ONE ingestion           →   INFINITE downstream agents
                            ─────────────────────────────
                            /exam        — generates practice tests
                            /flashcard   — first-letter recall drills
                            /lookup [X]  — instant term resolution
                            /compare     — cross-course analysis
                            /teach-back  — explain a topic to me
                            /gap         — what am I weak on?
```

---

## 🚀 Quick start

```bash
# 1. Install the skill into Claude Code
mkdir -p ~/.claude/skills/courserafied
cp SKILL.md ~/.claude/skills/courserafied/

# Or zip it into a .skill bundle (matches Claude Code's expected format)
cd .. && zip -r ~/.claude/skills/courserafied.skill courserafied/
```

```bash
# 2. In any Claude Code session:
/courserafied init vanderbilt-prompt-engineering
/courserafied ingest vanderbilt-prompt-engineering [paste your transcript]
```

That's it. Six categorized markdown files now exist with everything from that lecture, properly structured.

---

## 📂 What gets written

For each lecture/chapter ingested:

| File | Contains | Schema |
|---|---|---|
| `SUMMARIES.md` | One paragraph per lecture: what + thesis | [template](schemas/SUMMARIES.template.md) |
| `GLOSSARY.md` | Key terms + definitions | [template](schemas/GLOSSARY.template.md) |
| `PATTERNS.md` | Named frameworks + formulas | [template](schemas/PATTERNS.template.md) |
| `EXAMPLES.md` | Worked demonstrations from the instructor | [template](schemas/EXAMPLES.template.md) |
| `QUOTES.md` | Verbatim memorable lines | [template](schemas/QUOTES.template.md) |
| `EXAM-PREP.md` | Likely test Q&As | [template](schemas/EXAM-PREP.template.md) |
| `INDEX.md` | Auto-generated table of contents | (auto-managed) |

---

## 🛡️ Foolproof by design

Three rules make the system retrievable forever:

1. **Schema enforcement** — every entry has all required fields, or `_(missing)_` placeholder. No silent data loss.
2. **Consistent headers** — `## Title (Source ID)` format → predictable anchors → reliable cross-links.
3. **Auto-regenerated INDEX** — every entry is one click away. No "where did I put that?" rummaging.

Plus:
- Duplicate detection (same title twice → merge prompt)
- Cross-link validation (broken refs surface immediately)
- Quality threshold (if extraction feels forced, the category is skipped — no fluff)

---

## 🤖 Subcommands

```bash
# Course name is the FIRST argument after the subcommand.
/courserafied init {course-name}                          # create scaffold
/courserafied ingest {course-name} [transcript]           # parse + write to all 6 files
/courserafied query {course-name} [term]                  # find where a term lives
/courserafied validate {course-name}                      # run integrity checks
/courserafied stats {course-name}                         # entry counts, weak categories
/courserafied list                                         # show all courses in CWD

# Examples:
/courserafied ingest ibm-data-analysis [transcript]
/courserafied ingest vanderbilt [next lecture]
/courserafied stats vanderbilt
```

Course names normalize to kebab-case ("IBM Data Analysis" → `ibm-data-analysis`). Unknown course names auto-init from the template.

---

## 🔗 Pairs perfectly with...

- **[cowatch](https://github.com/DimmMak/cowatch)** — live study buddy that watches lectures with you. On `done`, it can hand the full transcript to courserafied for ingestion.
- **[claudenotes](https://github.com/DimmMak/claudenotes)** — the agent factory where downstream agents (`/exam`, `/flashcard`, `/lookup`) live and query the courserafied database.

```
LECTURE PLAYS
     ↓
🟣 cowatch (live reactions, captures)
     ↓
🟣 cowatch done → 📚 courserafied ingest
     ↓
6 categorized .md files written
     ↓
📓 claudenotes downstream agents read the files for exams/quizzes/lookups
```

---

## 🎓 Use cases beyond Coursera

The skill is named after Coursera but works for anything that produces structured course material:

- ✅ Coursera, edX, Udemy, Khan Academy — any video course with transcripts
- ✅ Textbooks (chapter at a time)
- ✅ Lecture slides + speaker notes
- ✅ Conference talks
- ✅ Podcast episodes (with transcripts)
- ✅ Internal training material at work

If it has structure, courserafied can ingest it.

---

## 📜 License

MIT — do whatever you want with it.

---

## 🃏 Built by

[@DimmMak](https://github.com/DimmMak) as part of an exploration of `.skill`-based agent factories for self-directed learning.

**Sister projects:**
- [cowatch](https://github.com/DimmMak/cowatch) — live lecture co-watcher
- [claudenotes](https://github.com/DimmMak/claudenotes) — agent factory for personal note-taking
- [promptlatro](https://github.com/DimmMak/promptlatro) — roguelike that drills prompt patterns
- [coderecall](https://github.com/DimmMak/coderecall) — first-letter recall for SQL/Python

📚🃏⚡
