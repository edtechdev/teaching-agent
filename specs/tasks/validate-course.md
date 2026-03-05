# Task: validate-course

## Purpose

Checks the consistency, completeness, and LiaScript syntax correctness of course documents.
Can be run in two modes:

- **Session mode** (`/validate-course {number} {type}`) — checks a single material file after co-authoring
- **Course mode** (`/validate-course`) — checks the entire course before publishing

## Inputs

- `context.md` — course type and conventions
- `checklists/course-quality-checklist.md` — structured checklist
- `data/liascript-cheat-sheet.md` — syntax reference for LiaScript checks
- For session mode: `materials/{number}-{type}.md` and matching row in `sessions.md`
- For course mode: all docs (`outline.md`, `didactics.md`, `agenda.md`, `sessions.md`, `skeletons/`, `materials/`)

## Output

- **Session mode**: short inline report (printed, not saved) with issues for this session
- **Course mode**: `validation-report.md` — structured report with pass/fail per section and a list of issues

---

## Session Mode Steps (`/validate-course {number} {type}`)

1. Load `context.md` for course type and conventions.
2. Load `agenda.md` to get the learning objectives for this session.
3. Load `data/liascript-cheat-sheet.md` as syntax reference.
4. Open `materials/{number}-{type}.md` and check:

   **Content checks:**
   - [ ] All learning objectives from `agenda.md` for this session are addressed
   - [ ] No section is vague, content-free, or placeholder-only
   - [ ] References present where content claims are made

   **Persona & style checks:**
   - [ ] Tone matches the instructor persona from `didactics.md`
   - [ ] Terminology matches `context.md` (sessions-called, etc.)

   **LiaScript syntax checks** (against `data/liascript-cheat-sheet.md`):
   - [ ] Exactly one `#` heading in the file (course title)
   - [ ] `###` and deeper headings only inside HTML blocks, lists, or blockquotes
   - [ ] All code blocks properly closed (triple backticks)
   - [ ] Animation counters (`--{{n}}--`, `{{n}}`) reset to 0 after each `##`
   - [ ] Quiz syntax correct: `[(X)]` for single choice, `[[X]]` for multiple choice, `[[answer]]` for text
   - [ ] All media elements have alt text
   - [ ] No unclosed `<div>` blocks

5. Report issues clearly with line references where possible.
6. If no issues found: confirm "Session {number} ({type}) — ✅ Syntax und Inhalt geprüft."
7. If issues found: list them and ask the instructor whether to open `/coauthor-materials` to fix them.

---

## Course Mode Steps (`/validate-course`)

1. Load `context.md` to understand course type and applicable conventions.
2. Load `checklists/course-quality-checklist.md` — apply only the checks relevant for this course type (skip sections marked with conditions that don't apply).
3. Load `data/liascript-cheat-sheet.md` as syntax reference.

4. **Check Context & Foundation:**
   - `context.md` complete (course type, terminology, agenda flag, conventions)
   - `outline.md`: title, target audience, time commitment `[not single-lesson]`, abstract, learning objectives
   - `didactics.md`: instructor persona, didactic concept, style, difficulty level

5. **Check Agenda** `[if agenda flag = yes in context.md]`:
   - All sessions have title, duration, type, learning objective, summary
   - Learning objectives align with `outline.md`

6. **Check Session Progress:**
   - Load `sessions.md` as primary source
   - All expected sessions have a row
   - Cross-check: every ✅ Skeleton row has a file in `skeletons/`
   - Cross-check: every ✅ Material row has a file in `materials/`
   - All sessions marked ✅ Fertig `[required before publishing]`

7. **Check each material file** in `materials/` (same LiaScript + content checks as Session Mode Step 4).

8. **Consistency check across all documents:**
   - Terminology consistent (sessions-called from `context.md` used throughout)
   - Persona tone consistent across all materials
   - Learning objectives from `outline.md` traceable through `agenda.md` into materials
   - Numbering correct and no gaps

9. **Create `validation-report.md`:**

   ```
   # Validation Report — [Course Title]
   Date: YYYY-MM-DD
   Course type: [type]

   ## Summary
   PASS / FAIL — [N issues found]

   ## Issues by Section
   ### Foundation
   - [issue or ✅ OK]

   ### Agenda
   - [issue or ✅ OK / SKIPPED (course type)]

   ### Session Progress
   - [issue or ✅ OK]

   ### Materials
   #### Session {N} — {title}
   - [issue or ✅ OK]

   ### Consistency
   - [issue or ✅ OK]

   ## Recommended Actions
   1. [Concrete action with file reference]
   ```

10. After report is created: suggest next step.
    - If issues exist: "Öffne `/coauthor-materials {number} {type}` um die Issues in Session X zu beheben, dann erneut `/validate-course` ausführen."
    - If no issues: "Kurs ist bereit für Publishing. Nächster Schritt: `/agent development` → `/create-project`"
