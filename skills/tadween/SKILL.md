---
name: tadween
description: "تدوين وتوثيق كل ما هو مهم في المشروع داخل ملفات Markdown (AGENTS.md + CLAUDE.md + مجلد docs/) حتى تبدأ أي جلسة جديدة فاهمةً كل شيء دون إعادة شرح، أو تحديث هذا التوثيق إن كان موجودًا. شغّله عند: «دوّن»، «وثّق»، «حدّث التوثيق»، «سجّل المهم»، «احفظ السياق قبل محادثة جديدة». Capture or refresh persistent project context (decisions, user preferences, status, next steps) so a fresh agent session can resume cold. Not for writing end-user, API or publishing docs."
argument-hint: "(اختياري) ما الذي تريد التركيز عليه في التدوين"
---

# Tadween — project memory in Markdown

The docs must survive a **cold start**: a fresh agent (Claude Code, Codex, Cursor, …) holding only these files — none of this conversation — continues the work immediately, without the user re-explaining anything. The user reads these files too, so they must also be clear to a human.

## Layout

```
<project root>/
├── AGENTS.md   ← main file: the essentials every session needs
├── CLAUDE.md   ← contains `@AGENTS.md`, so Claude Code loads the main file
└── docs/       ← detail, only when the project needs it
    ├── decisions.md
    ├── progress.md
    └── <topic>.md
```

- AGENTS.md is the shared standard read by Codex, Cursor, OpenCode and others; Claude Code reads CLAUDE.md, which pulls AGENTS.md in through the `@AGENTS.md` import line.
- The main file is loaded into every session's context, so it holds only what every session needs and stays lean (aim for ≤ ~150 lines). Detail goes to `docs/`, listed in the main file's docs map so an agent knows what to open and when.
- Small project → main file only. Create `docs/` when the main file would pass ~150 lines, a topic needs more than ~20 lines of detail, or the project has several distinct parts.

Existing files win over this layout:
- Only CLAUDE.md exists, with content → it stays the main file (no migration). Create AGENTS.md holding one line: `Project context lives in CLAUDE.md — read it first.`
- Only AGENTS.md exists → it is the main file; create CLAUDE.md containing `@AGENTS.md` (or add that line at the top of an existing CLAUDE.md that lacks it).
- The project already has its own docs (README, a "read me first" file, a `docs/` for a website) → link them from the docs map instead of copying. If `docs/` already serves another purpose, use `docs/context/`.

Project root: the git root inside a repo, otherwise the current working directory. If that is the home directory or clearly not a project, ask the user where to write.

## Steps

### 1. Read what exists

Read the main file(s) fully, and every doc you may touch; skim the headings of the rest. If tadween docs already exist, this run is an **update**: integrate, never rebuild from scratch.

The user may invoke this skill several times in one session — often because they doubt the last run caught everything. Treat each invocation as a fresh audit of the conversation against the files; "already documented" is a valid answer only after checking item by item.

Done when: you know what every relevant doc currently says.

### 2. Harvest the conversation

Walk the whole conversation and list every item worth keeping. If it was compacted, use the summary and also inspect the project's state (recently modified files, `git log`, `git diff`) to recover what happened.

- **Goal & scope** — what the project is, for whom, what "done" looks like.
- **Decisions** — what was chosen, why, and what was rejected (so nobody re-litigates it).
- **User preferences & corrections** — how the user wants things done, what they refused. Highest value: repeating these is exactly what the user wants to stop doing.
- **Current status** — done / in progress / blocked, and where work stopped.
- **Next steps** and **open questions** waiting on the user.
- **Problems & fixes** — what broke, the cause, what fixed it, dead ends tried.
- **Project map** — key files, folders, commands, tools, external links and accounts.
- **Domain knowledge** — rules, figures, assumptions and sources learned during the work that the files don't make obvious.

Leave out what a cold-start agent doesn't need: small talk, narration of routine edits, temporary debug output, and content already in the project's files (point to the file instead).

If the user passed text with the command, give that topic extra weight.

Done when: every decision, preference, correction, status change and solved problem in the conversation is on your list or consciously judged unimportant.

### 3. Write

- **Merge, don't pile up.** Update a fact where it lives. Each fact lives in one place: the main file summarizes, `docs/` holds the detail, and they link to each other.
- **Correct what is stale.** An outdated status or a contradicted decision misleads the next session more than a gap does. A reversed decision stays in `decisions.md`, marked superseded with the reason.
- **Keep the user's own writing.** Edit around content the user wrote by hand.
- **Language:** the one the user speaks in the conversation; code, paths, commands and technical identifiers stay in their original form. Short sentences, bullets, tables.
- **Absolute dates** (YYYY-MM-DD) — "today" means nothing in a later session. Get the date with `date +%F`.
- **Secrets stay out.** For API keys, passwords and tokens, write where they are stored (e.g. "`.env` → `API_KEY`"), never the value. Treat sensitive client data (IDs, account numbers, salaries) the same way: reference the source file.
- Bump the main file's last-updated date and add today's entry to the progress log (`docs/progress.md`, or a short "آخر ما تم" line in the main file for a small project).

The main file's format is below. Before creating or restructuring anything in `docs/`, read [DOCS-FORMAT.md](DOCS-FORMAT.md).

### 4. Cold-start check

Re-read the main file as a new agent who knows nothing about this project. List what you would have to ask the user before continuing; each such question must be answered by the main file or a doc it points to — fill every gap. Confirm that the paths and commands you documented actually exist.

Done when: no question remains that would force the user to re-explain.

### 5. Report

Tell the user briefly, in their language: which files were created or updated, one line each on what changed. On a repeat run with nothing new, say so and list the latest points already captured so the user can confirm nothing is missing.

## Main file format

Headings follow the user's language (Arabic shown here). Omit sections with nothing in them; add project-specific ones when useful (e.g. «طريقة التشغيل»).

```markdown
# <اسم المشروع>

> آخر تحديث للتوثيق: YYYY-MM-DD

## نظرة عامة
<ما المشروع، ولمن، وما الهدف — من سطرين إلى خمسة>

## الحالة الحالية
<ما اكتمل، وما قيد العمل، وأين توقّفنا>

## الخطوات التالية
- [ ] ...

## قرارات أساسية
- <القرار> — <سببه باختصار> (التفاصيل: docs/decisions.md)

## تفضيلات المستخدم وقواعد العمل
- ...

## خريطة المشروع
<أهم الملفات والمجلدات والأوامر والروابط>

## خريطة التوثيق
| الملف | محتواه | متى تقرؤه |
|---|---|---|
| docs/decisions.md | سجل القرارات وأسبابها | قبل تغيير أي قرار سابق |

## قاعدة التوثيق (للوكيل)
- ابدأ من «الحالة الحالية» و«الخطوات التالية»، واقرأ من «خريطة التوثيق» ما يخص مهمتك قبل البدء.
- عند حدوث شيء مهم (قرار، تغيير في البنية، تفضيل جديد من المستخدم، مشكلة حُلّت، مرحلة اكتملت) حدّث الملف المعني في حينه، وأبقِ هذا الملف موجزًا.
- لمراجعة شاملة قبل إنهاء الجلسة استخدم سكل `tadween` إن كان متاحًا.
```

The «قاعدة التوثيق» section is what keeps the docs current between invocations — every future session reads it and keeps updating on its own. Keep it in every main file.

A CLAUDE.md that only bridges to AGENTS.md contains just:

```markdown
@AGENTS.md
```
