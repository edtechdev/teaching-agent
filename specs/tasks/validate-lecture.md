# Task: validate-lecture

## Purpose

Checks the consistency and completeness of all lecture documents based on the didactics from `docs/lecture-didactics.md` and the agenda from `checklists/lecture-quality-checklist.md`.
**The agent also adopts the professor persona and style from `docs/lecture-didactics.md#Professor-Persona` into its own persona, so all content is written in this voice.**

## Output

- `docs/validation-report.md`

## Steps

1. Load and use the structure from `checklists/lecture-quality-checklist.md`.
2. Check the outline.
3. Check the didactics.
4. Check the agenda.
5. Check the session skeletons.
6. Check the materials.
7. Create the report.
