# Task: create-outline

## Purpose

Creates the **Lecture Outline** as a starting point for a lecture.
Defines title, target audience, abstract, learning objectives, and optionally a logo.

## Inputs

- Title of the lecture
- Target audience (e.g., students, professionals, beginners)
- Time commitment (e.g., semester hours per week, total hours)
- Abstract (topics, content, benefits)
- Learning objectives (3–5 concrete goals)
- Logo (optional, as a prompt)

## Output

- `docs/lecture-outline.md` (Markdown file)
- Structure based on `template/lecture-outline.yaml`

## Steps

1. Collect title, target audience, time commitment, and abstract.
2. Define 3–5 concrete learning objectives.
3. Optionally add a logo prompt.
4. Fill the `template/lecture-outline.yaml` with the inputs.
5. Save the file as `docs/lecture-outline.md`.
