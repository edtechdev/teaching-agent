# Task: promote-session

## Purpose

Converts a **Session Skeleton** into a detailed **Session Material**.  
**The agent also adopts the instructor persona and style from `docs/course-didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- number, type
- skeleton: file from `skeletons/`
- didactics: content from `docs/course-didactics.md`
- agenda: content from `docs/course-agenda.md`
- **Instructor persona from `docs/course-didactics.md` (mandatory handoff)**
- **Style & difficulty level from `docs/course-didactics.md` (mandatory handoff)**
- Terminology from `docs/course-context.md`

## Output

- `materials/{number}-{type}.md`
- Structure based on `templates/session-material.yaml`

## Steps

1. Load skeleton.
2. Read `docs/course-context.md` for terminology and conventions.
3. Adopt didactic concept and course type from Didactics.
4. **Agent adopts the instructor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the professor persona.
- All agenda descriptions reflect this style.

4. Insert agenda information.
5. Consider didactic inputs.
6. Generate planned outline.
7. Apply template.
8. Save the file.