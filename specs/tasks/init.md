# Task: init

## Purpose

Initializes a new course project by creating `context.md`.

This is the **first mandatory step** for every new course project.
The course context acts as the governance layer: it defines the course type, terminology, persona style, conventions, and LiaScript rules that all subsequent tasks will load and follow.

## Inputs

- Course type (asked interactively)
- Working title (optional at this stage)
- Instructor preferences (optional)

## Output

- `context.md` (Markdown file)
- Structure based on `templates/course-context.yaml`

## Steps

1. Welcome the instructor and briefly explain the workflow.
2. Ask for the **course type**:
   1. **lecture-series** – Semester course / lecture series with instructor
   2. **self-paced** – Self-learning course, asynchronous, no live sessions
   3. **workshop** – Intensive, interactive, time-boxed (1–3 days)
   4. **single-lesson** – One standalone lesson or tutorial
   5. **improve-existing** – Analyze and improve an existing course
3. Ask for a working title (optional).
4. Ask about the target platform (LiaScript / other).
5. Based on the course type, set the profile defaults:

   | Type | Terminology | Persona | Agenda | Pacing | Assessment |
   |---|---|---|---|---|---|
   | lecture-series | session / lecture | professor | required | scheduled | quizzes + assignments |
   | self-paced | unit / module | coach | optional | learner-driven | self-check quizzes |
   | workshop | block / activity | facilitator | required | event-based | reflection + group work |
   | single-lesson | lesson | tutor | no | n/a | optional quiz |
   | improve-existing | (from existing) | (from existing) | optional | (from existing) | (from existing) |

6. Ask about project-level conventions:
   - Language (de / en / other)
   - Tone (formal / informal / conversational)
   - Person (Sie / Du / you)
   - Accessibility requirements
   - Any LiaScript-specific conventions

7. Fill the `templates/course-context.yaml` template with the collected inputs.
8. Save the file as `context.md`.
9. Confirm completion and suggest the next step based on course type:
   - **lecture-series / self-paced / workshop** → `/create-outline`
   - **single-lesson** → `/create-outline`, then directly `/create-session 1 lecture`
   - **improve-existing** → ask the instructor to share the existing materials first

## Notes

- All subsequent tasks (`/create-outline`, `/create-didactics`, `/create-agenda`, etc.) will read `context.md` and adapt their behavior accordingly.
- The profile defaults are suggestions; the instructor can override any field.
- For `improve-existing`, the init creates a baseline context; the actual analysis happens interactively during `/coauthor-materials`.
