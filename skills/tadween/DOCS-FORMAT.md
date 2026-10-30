# docs/ formats

Create a file only when there is content for it. File names are short English kebab-case (robust across tools and shells); the content is in the user's language. Each file opens with a one-line purpose and a «آخر تحديث: YYYY-MM-DD» line. Every file gets a row in the main file's docs map.

## decisions.md — decision log

Newest first. One entry per decision:

```markdown
### YYYY-MM-DD — <القرار في سطر واحد>
- **السبب:** ...
- **البدائل المرفوضة:** <البديل> — <لماذا رُفض>
- **الحالة:** معتمد | أُلغي في YYYY-MM-DD ← <ما حلّ محلّه ولماذا>
```

A superseded decision is marked, not deleted: the record of *why we changed* stops the next agent from proposing the old option again. The main file's «قرارات أساسية» keeps only the active ones, one line each.

## progress.md — session log

Newest first. One entry per date; if today's entry already exists (a repeat run), extend it instead of adding a second one.

```markdown
### YYYY-MM-DD
- **أُنجز:** ...
- **توقّفنا عند:** ...
```

When it passes ~300 lines, condense entries older than ~2 months into a short «ملخّص المراحل السابقة» section at the bottom.

## lessons.md — problems and pitfalls

Things likely to matter again: problem → cause → fix, approaches that failed and why, environment quirks. One-off hiccups belong in progress.md, not here.

```markdown
### <المشكلة باختصار>
- **السبب:** ...
- **الحل:** ...
```

## <topic>.md — one per part of the project

One file per subsystem, feature, workflow or domain topic (e.g. `zakat-calculation.md`, `invoices-import.md`, `glossary.md`): how it works, the rules and figures it depends on, the files involved, examples. Name it after what it holds.

## architecture.md — for larger projects

How the project is organized, how its parts connect, and how data flows between them. Create it once the main file's «خريطة المشروع» can no longer explain the structure in a few lines.
