# 🤝 Contributing to courserafied

PRs welcome. Highest-impact contributions:

---

## ✨ New downstream agents

The whole point of courserafied is feeding agents that READ from the knowledge base. The 6 categorized files are the database; agents are the queries. Build new ones!

Ideas:
- `/exam` — generate a mock exam from EXAM-PREP.md (multiple choice, short answer, essay variants)
- `/flashcard` — first-letter recall drills from GLOSSARY.md (CodeRecall mechanic)
- `/lookup [term]` — fast term resolution via INDEX.md
- `/teach-back [topic]` — Socratic dialogue, you explain, agent probes
- `/gap-analysis` — drill into what you're weak on (read recall stats)
- `/compare [course-a] [course-b]` — find overlap and contradictions across courses
- `/cite [claim]` — verify a claim against course material, return source

Each downstream agent is its own `.skill` file — submit as a standalone skill.

---

## 📐 New schema templates

If you find a category not covered by the current 6, propose a new schema:

1. Create `schemas/{NEWNAME}.template.md` following the existing pattern
2. Update `SKILL.md` STEP 3 table with the new category
3. Update `courses/_template/` with the new file
4. PR with rationale: what content fits here that doesn't fit existing files?

Bar: a new category should be **mutually exclusive** with existing categories AND have a **clear downstream agent** that would benefit from reading it.

---

## 🌱 New ingestion sources

Currently the skill assumes pasted transcripts. PRs welcome to add:
- PDF ingestion (use the anthropic-skills:pdf skill as upstream)
- YouTube auto-transcript fetching
- Coursera bulk export parsing
- Notion / Obsidian vault import

---

## 🐛 Bug reports

Open an issue with:
- Course material you tried to ingest
- Expected output (which file should have what)
- Actual output (what got written / not written)
- Any validation errors

---

## 📜 Code style

- **Markdown:** functional emojis paired (per project aesthetic 🃏📚)
- **Skill files:** follow Anthropic's `.skill` frontmatter spec
- **Schemas:** every field a downstream agent might need; nothing speculative

---

📚🃏 Thanks for contributing.
