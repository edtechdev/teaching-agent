# Task: create-agenda

## Purpose

Creates the **Course Agenda** as a structured schedule for the course.  
Defines sessions/modules with title, duration, type (lecture/exercise), learning objectives, summary, and the corresponding materials files.
**The agent also adopts the instructor persona and style from `didactics.md` into its own persona, so all content is written in this voice.**

## Inputs

- Learning objectives from `outline.md#Learning-Objectives`
- Abstract from `outline.md#Abstract`
- Time commitment from `outline.md#Time-Commitment`
- Didactic concept from `didactics.md#Didactic-Concept`
- **Instructor persona from `didactics.md#Professor-Persona` (mandatory handoff)**
- **Style & difficulty level from `didactics.md` (mandatory handoff)**
- Course type from `context.md`

## Output

- `agenda.md` (Markdown file)
- Structure based on `templates/course-agenda.yaml`

## Steps

1. Read `context.md`:
   - Check `agenda` field in the profile:
     - **`no`** → Inform the instructor that the agenda was skipped during init and suggest proceeding with `/create-session 1 {type}`. Stop here.
     - **`optional`** → 🎛️ Ask with structured question (single choice):
       - **Ja** — Agenda erstellen, um die Struktur zu planen
       - **Nein** — direkt zu `/create-session`
       - **Später** — Agenda überspringen, später nachholen
       If no: redirect to `/create-session`. If yes: continue.
     - **`yes`** (required) → Continue without asking.
   - Read terminology (sessions-called, lectures-called) and pacing model.
2. Read learning objectives from the outline.
3. Adopt didactic concept and course type from Didactics.
4. **Agent adopts the instructor persona & style from Didactics into its own persona.**

- From this step, the agent writes in the tone of the instructor persona.
- All agenda descriptions reflect this style.

5. Define sessions/modules using the terminology from `context.md`.
6. Build the agenda in a structured form adapted to the pacing model:
   - **lecture-series**: sessions with time slots and weekly schedule
   - **workshop**: blocks with approximate time per block
   - **self-paced**: modules without fixed time slots, estimated duration only
   - **single-lesson** (if agenda is yes): sections/chapters within the lesson, no time slots
7. Fill the `templates/course-agenda.yaml` template with the results.
8. Save the file as `agenda.md`.
