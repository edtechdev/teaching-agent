# Task: create-didactics

## Purpose

Creates the document **Course Didactics & Style**.  
Defines the didactic concept, instructor persona, style, and course type.  
Builds on the outline to ensure a consistent teaching strategy aligned with the course type from `context.md`.

## Inputs

- Abstract from `outline.md`
- Target audience from `outline.md`
- Learning objectives from `outline.md`
- Course type & conventions from `context.md`

## Output

- `didactics.md` (Markdown file)
- Structure based on `templates/course-didactics.yaml`

## Steps

1. Read `context.md` for course type, persona type, and conventions.
2. Read abstract, target audience, and learning objectives from `outline.md`.
3. Design a suitable didactic concept (teaching methods, learning phases) adapted to the course type:
   - **lecture-series**: structured phases, presenter-driven, attendance-based
   - **self-paced**: modular, learner-driven, self-check oriented
   - **workshop**: activity-driven, collaborative, time-boxed
   - **single-lesson**: focused, compact, single arc
4. Describe the instructor persona (expertise, role, style) aligned with the persona type from `context.md`.
5. Define style & difficulty level (humorous, scientific, practical, etc.).
6. Set the delivery format consistent with the course type.
7. Fill the `templates/course-didactics.yaml` template with the results.
8. Save the file as `didactics.md`.