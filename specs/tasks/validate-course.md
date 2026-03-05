# Task: validate-course

## Purpose

Checks the consistency and completeness of all course documents based on the didactics from `didactics.md`, the course context from `context.md`, and the checklist from `checklists/course-quality-checklist.md`.
**The agent also adopts the instructor persona and style from `didactics.md#Professor-Persona` into its own persona, so all content is written in this voice.**

## Output

- `validation-report.md`

## Steps

1. Load `context.md` to understand the course type and applicable conventions.
2. Load and use the structure from `checklists/course-quality-checklist.md`.
3. Check the outline.
4. Check the didactics.
5. Check the agenda (if applicable for the course type).
6. Load `sessions.md` if it exists — use it as the primary source for skeleton/material/done status per session.
7. Cross-check: verify that every row marked ✅ in `sessions.md` has a corresponding file in `skeletons/` or `materials/`.
8. Check the materials.
9. Create the report.
