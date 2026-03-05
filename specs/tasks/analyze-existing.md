# Task: analyze-existing

## Purpose

Analyzes an existing course project to identify which documentation is present and which is missing.
Used as the **second step after `/init`** when the course type is `improve-existing`.

Offers two paths for each missing core document:
- **Auto-generate** — agent reads existing materials and reverse-engineers a draft
- **Interactive creation** — agent guides the instructor through the relevant creation task

## Inputs

- `context.md` (created by `/init`, mandatory)
- Existing project files in the project root: `outline.md`, `didactics.md`, `agenda.md`, `visuals.md`
- Existing folders: `skeletons/`, `materials/`

## Output

- `docs-status.md` — status overview with recommended actions
- Optionally: auto-generated drafts for missing core docs (marked as draft)

## Steps

1. Load `context.md` for course type, terminology, and conventions.

2. Scan the project root and relevant folders:

   | Document | Required |
   |---|---|
   | `outline.md` | always |
   | `didactics.md` | always |
   | `agenda.md` | if `context.md` agenda = yes |
   | `visuals.md` | optional |
   | `skeletons/` | if sessions expected |
   | `materials/` | if sessions expected |

3. Display a **Course Doc Status** table:
   - ✅ exists
   - ⚠️ exists but likely incomplete (e.g., missing sections)
   - ❌ missing

4. For each **missing** core document (`outline.md`, `didactics.md`), ask the instructor:
   > "Für `[document]` gibt es drei Optionen:
   > 1. **Auto-generieren** — ich lese deine bestehenden Materialien und erstelle einen Entwurf
   > 2. **Interaktiv erstellen** — ich führe dich durch das passende Erstellungs-Kommando
   > 3. **Überspringen** — weiter ohne dieses Dokument"

5. If **auto-generate** is chosen:
   - Read all available files in `skeletons/` and `materials/`
   - Extract: title, target audience, topics, recurring structure, learning objectives
   - Generate a draft and save it (e.g., `outline.md`)
   - Add a draft marker at the top: `> **Draft (auto-generated from existing materials)** — please review and update`

6. If **interactive creation** is chosen, run the relevant task:
   - `outline.md` → `/create-outline`
   - `didactics.md` → `/create-didactics`
   - `agenda.md` → `/create-agenda`

7. After all missing docs are handled, list **improvement opportunities** in the existing content:
   - Sessions without materials
   - Materials without skeletons
   - Inconsistent terminology or persona style
   - Missing references or learning objectives
   - Language/tone inconsistencies vs. `context.md` conventions

8. Suggest a prioritized action list and the recommended next step (usually `/coauthor-materials`).

9. Save the full status overview as `docs-status.md`.
