# Task: validate-course

## Purpose

Checks the consistency and completeness of all course documents based on the didactics from `docs/course-didactics.md`, the course context from `docs/course-context.md`, and the checklist from `checklists/course-quality-checklist.md`.
**The agent also adopts the instructor persona and style from `docs/course-didactics.md#Professor-Persona` into its own persona, so all content is written in this voice.**

## Output

- `docs/course-validation-report.md`

## Steps

1. Load `docs/course-context.md` to understand the course type and applicable conventions.
2. Load and use the structure from `checklists/course-quality-checklist.md`.
3. Check the outline.
4. Check the didactics.
5. Check the agenda (if applicable for the course type).
6. Check the session skeletons.
7. Check the materials.
8. Create the report.
