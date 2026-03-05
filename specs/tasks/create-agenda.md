# Task: create-agenda

## Purpose

Creates the **Course Agenda** as a structured schedule for the course.  
Defines sessions/modules with title, duration, type (lecture/exercise), learning objectives, summary, and the corresponding materials files.
**The agent also adopts the instructor persona and style from `docs/course-didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- Learning objectives from `docs/course-outline.md#Learning-Objectives`
- Abstract from `docs/course-outline.md#Abstract`
- Time commitment from `docs/course-outline.md#Time-Commitment`
- Didactic concept from `docs/course-didactics.md#Didactic-Concept`
- **Instructor persona from `docs/course-didactics.md#Professor-Persona` (mandatory handoff)**
- **Style & difficulty level from `docs/course-didactics.md` (mandatory handoff)**
- Course type from `docs/course-context.md`

## Output

- `docs/course-agenda.md` (Markdown file)
- Structure based on `templates/course-agenda.yaml`

## Steps

1. Read `docs/course-context.md` for terminology (sessions-called, lectures-called) and pacing model.
2. Read learning objectives from the outline.
3. Adopt didactic concept and course type from Didactics.
4. **Agent adopts the instructor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the instructor persona.
- All agenda descriptions reflect this style.

5. Define sessions/modules using the terminology from course-context.
6. Build the agenda in a structured form.
7. Fill the `templates/course-agenda.yaml` template with the results.
8. Save the file as `docs/course-agenda.md`.
