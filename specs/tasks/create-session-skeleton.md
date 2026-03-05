# Task: create-session-skeleton

## Purpose

Creates a **Session Skeleton** (lecture or exercise) as a structured framework.  
**The agent also adopts the instructor persona and style from `didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- number: session number
- type: type of session (`lecture` or `exercise`)
- title (optional)
- Didactic concept from `didactics.md`
- **Instructor persona from `didactics.md` (mandatory handoff)**
- **Style & difficulty level from `didactics.md` (mandatory handoff)**
- Terminology from `context.md` (sessions-called, lectures-called)

## Output

- `skeletons/{number}-{type}.md` (Markdown file)
- Structure based on `templates/session-skeleton.yaml`

## Steps

1. Collect session number, type, and optional title.
2. Read `context.md` for terminology and conventions.
3. Adopt didactic concept and course type from Didactics.
4. **Agent adopts the instructor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the professor persona.
- All agenda descriptions reflect this style.

4. Generate the basic structure for the session.
5. Fill out template `templates/session-skeleton.yaml`.
6. Save the file.