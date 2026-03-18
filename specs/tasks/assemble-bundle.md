# Task: assemble-bundle

## Purpose

Combines all course documents into a complete, distributable package for handoff, archiving, or offline use.
Produces a structured `course-bundle/` folder with an auto-generated index and all relevant artifacts.

## Inputs

- `context.md` — course metadata and conventions
- `outline.md` — course title and abstract (used in bundle index)
- `didactics.md` — teaching approach and persona documentation
- `agenda.md` — session schedule (if exists)
- `sessions.md` — production status tracking
- `skeletons/` — session skeletons (optional, for documentation trail)
- `materials/` — full session materials (primary content)
- `visuals.md` + `assets/` — visual style guide and assets (if exists)
- `validation-report.md` — latest QA report (**required, must show PASS**)
- `notes/` — decision records and summaries (optional)

## Output

```
course-bundle/
├── bundle-index.md          ← auto-generated index
├── context.md
├── outline.md
├── didactics.md
├── agenda.md                ← if exists
├── sessions.md
├── validation-report.md
├── materials/
│   └── {n}-{type}.md
├── skeletons/               ← optional
│   └── {n}-{type}.md
├── assets/                  ← if exists
└── notes/                   ← if exists
```

## Steps

1. **Pre-flight check:** Confirm `validation-report.md` exists and shows PASS.
   - If missing or FAIL: block bundling. State: "⛔ Please run `/validate-course` first and resolve all issues before creating the bundle."

2. Read course title and abstract from `outline.md`.

3. Scan all source folders and collect files:
   - **Required:** `context.md`, `outline.md`, `didactics.md`, `sessions.md`, all files in `materials/`, `validation-report.md`
   - **Conditional:** `agenda.md` (if exists), `skeletons/` (if exists), `assets/` (if exists), `notes/` (if exists)

4. Generate `bundle-index.md`:

   ```markdown
   # Course Bundle: [Course Title]

   Generated: YYYY-MM-DD
   Course type: [type from context.md]
   Validation: PASS (see validation-report.md)

   ## Contents

   | File                    | Description                              |
   |-------------------------|------------------------------------------|
   | context.md              | Course governance and conventions        |
   | outline.md              | Title, audience, learning objectives     |
   | didactics.md            | Teaching approach and instructor persona |
   | agenda.md               | Session schedule and structure           |
   | sessions.md             | Production status per session            |
   | validation-report.md    | Quality validation results               |
   | materials/{n}-{type}.md | Session N: [title from agenda.md]        |

   ## Quick Start

   - **Instructor handoff:** Start with `outline.md` and `didactics.md`
   - **LiaScript publish:** Use files in `materials/` directly
   - **Quality audit:** See `validation-report.md`
   ```

5. Copy all collected files into `course-bundle/` preserving subfolder structure.

6. Confirm completion:
   > "Bundle created in `course-bundle/`. Contains [N] material files, [agenda.md ✅ / no agenda], [assets/ ✅ / no assets]."
   > "Next step: `/agent development` → `/create-project` to publish the course."