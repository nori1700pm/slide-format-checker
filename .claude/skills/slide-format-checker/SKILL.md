---
name: slide-format-checker
description: Final formatting check for existing company PowerPoint (.pptx) slides. Use when the user asks to review, check, standardize or fix slide formatting (fonts, title and description style, date format, line wrapping). The skill flags deviations, auto-fixes what is safe, and reports every change. It does not create slides.
---

# Slide Format Checker

## Purpose

This skill is a **final formatting check on existing slides**. It does not generate slides or rewrite content.

For every deck it reviews, it must:

1. **Flag** each deviation from the rules below.
2. **Fix** the deviation automatically when the fix is safe and unambiguous.
3. **Report** every change to the user. Anything that needs human judgment is **flagged but not changed**.

Slides use the **company master slide (10.83" x 7.5", 9906000 x 6858000 EMU)**. Do not check or change the slide size.

The reference deck for "correct" is **`★SDF DX Stage-Gate Document Template_v6_WIP_DRAFT_Formatted.pptx`** (in this skill folder). When a position or style is unclear, the template slides are the source of truth: slides 2 (checklist), 3 (divider) and 4 to 14 (content slides).

---

## 1. Slide types and which rules apply

| Slide type | How to identify | Rules applied |
|---|---|---|
| **Hidden slide** | `show="0"` on the slide | **Skipped.** List it in the report as skipped. |
| **Cover** | First slide | **Exempt from everything.** Do not touch. |
| **Section divider** | **Layout first**: the slide uses the divider layout (`Section Title`, as in template slide 3, with a large `LX` / `L3` code and a stage name). Use content only if the layout is unclear. | **Exempt from everything.** Do not touch. |
| **Checklist** | Title or eyebrow label contains "checklist" (case-insensitive). Follow template slide 2. | **Every rule in this document**, with the checklist title position in section 2. |
| **All other slides** | Everything else | Every rule in this document |

If a slide's type cannot be determined with confidence, **do not guess**. Flag it in the report and ask the user.

**Stage slides** are slides whose title matches `L{stage}-{no}`, where `{stage}` is digits and `{no}` is digits or `X` (for example `L3-2: ...`, `L0-X: ...`). Everything else is a non-stage slide, including titles that only start with `L3` (such as `L3 stage gate overview`) and lowercase `l3-2`. Leave those alone and do not flag them.

---

## 2. Fonts, sizes, colors and positions

Positions are measured from the **top-left corner** of the slide and must match **exactly** (compare in EMU, no tolerance).

| Element | Font | Size | Color | Position (X, Y) | Bold |
|---|---|---|---|---|---|
| **Slide title** | HDharmony B | 24 pt | #002060 | 0.3", 0.25" (274320, 228600 EMU) | Off |
| **Slide title, checklist slides** | HDharmony B | 24 pt | #002060 | 0.17", 0.297" (155448, 271681 EMU) | Off |
| **Page brief description** (only if the slide has one) | HDharmony B | 18 pt | #002060 | 0.5", 0.95" (457200, 868680 EMU) | Off |
| **Header text** (see below) | HDharmony B | Not enforced | Not enforced | Not enforced | Off |
| **Body text** (see below) | Hyundai Sans Text KR | 12 pt, or 11 pt if 12 pt does not fit | Not enforced | Not enforced | Not checked |

### Identifying the title and brief description

- **Title:** the title placeholder ("Insert Title here" in the template). If the slide has no title placeholder, use the top-left text box that holds the slide title (in the template, the text box named `제목 1`).
- **Brief description:** the text next to the blue vertical line, below the title. If it is at the wrong position, move it to 0.5", 0.95" as in template slides 4 to 14.
- **Eyebrow labels** (small labels above the title such as "CHECKLIST" or "1 Pager") are ignored.
- **Slides with an eyebrow label other than checklist** (for example `1 Pager`): the title position is not enforced yet. Fix font, size, color and bold, and flag the position for the user to confirm.

### Header text vs body text

Classify by the text itself, not only by the shape. Headers use HDharmony B. Body text uses Hyundai Sans Text KR.

**Header text:**
- Table header cells (the first row of a table).
- Short labels or captions of 1 to 3 words, such as "Sensor" or "Test Plans".
- Section headers: usually 2 to 5 words, in a larger size (14 pt and above), followed by a paragraph of smaller text.

**Body text:** everything else, including table cells, footnotes, text inside groups, charts and shapes. Replace any other font (Arial, Calibri, HDharmony M, Hyundai Sans Head KR, and so on) with Hyundai Sans Text KR.

**Ambiguous: flag, do not change the font.** For example:
- A 1 to 3 word item that is one bullet among several in the same list.
- A 4 to 5 word phrase at 12 pt or smaller that stands on its own.

### Notes

- **HDharmony B is already a bold-looking typeface. The bold attribute must be OFF** on all HDharmony B text (titles, descriptions, table headers, section headers). Table styles can make the first row bold by inheritance, so set `b="0"` explicitly.
- **Font names must be exact:**
  - `HDharmony B`. The Korean display name `현대하모니 B` is the same face and is accepted as is. Do not rename it and do not report it.
  - `Hyundai Sans Text KR`. In PowerPoint's font list this appears as "Hyundai Sans Text KR (Body)", where "(Body)" means the theme body font. Accept either the explicit typeface name or a theme body-font reference that resolves to Hyundai Sans Text KR.
- **Set both the Latin and East Asian typefaces** (`a:latin` and `a:ea`) to the required font, because both fonts are designed for Korean characters.
- **Page brief description:** optional. If a slide has none, do not flag it as missing.
- **Theme colors:** if the title or description color is a theme color rather than an explicit RGB, leave it, flag it, and ask the user.

### Inherited values

Many sizes, fonts and bold settings are not written on the run. They come from the paragraph, the shape's list style, the layout, the master or the table style.

- Work out the **effective value** (run, then paragraph, then shape, then layout placeholder, then master, then table style).
- If the effective value already complies, leave it alone and report nothing.
- If it does not comply, write an explicit value on the run.
- If the effective value cannot be resolved with confidence, flag it instead of guessing.

---

## 3. Body text size rules

Body text should be 12 pt, or 11 pt when 12 pt does not fit the box.

| Current size | Action |
|---|---|
| Above 12 pt | Set to **12 pt** |
| Below 11 pt | Set to **11 pt** |
| Exactly 11 pt or 12 pt | **Leave alone** |

- Do not decide whether 12 pt "fits". The choice between 11 and 12 depends on the slide content.
- If the text may overflow its box after the change, or 12 pt may not fit but you cannot tell, **flag the slide for manual review**. Do not shrink on your own.
- Header text size is not enforced.

---

## 4. Stage slide title format

Stage slide titles must use this format: **`L{stage}-{no}: Title`**

Examples: `L1-3: Project Path to L3 stage gating`, `L3-2: Logical : Testing Strategy`

- **Auto-rewrite only when the correction is unambiguous**, such as a missing space, a missing colon or a wrong separator:
  - `L3-2:Testing` becomes `L3-2: Testing`
  - `L3 2: Testing` becomes `L3-2: Testing`
  - `L3-2 - Testing` becomes `L3-2: Testing`
  - `L4-7 Production Release` becomes `L4-7: Production Release`
- **Flag without changing** when more than one correction is possible.
- Keep line breaks and the rest of the title text exactly as written.
- Non-stage slides may use any title format. They still follow the font, size, color and position rules.

---

## 5. Date format

The standard date format is **MM/DD/YYYY, zero-padded** (for example `06/10/2025`).

Check dates **anywhere on the slide**: text boxes, shapes, tables, groups and inside sentences. **Ignore speaker notes.**

| Input | Action |
|---|---|
| `3/27/2026` (day is over 12, so month/day order is clear) | Fix to `03/27/2026` |
| `2nd Jan 2025`, `10th June 2025`, `10 Jun 2025` (day, month name, year) | Fix to `01/02/2025`, `06/10/2025`, `06/10/2025` |
| Already `MM/DD/YYYY` | Leave alone |
| Numeric date where both parts are 12 or less and not zero-padded (for example `3/4/2026`) | **Flag, do not change** |
| Date with no year (for example `13 Feb`) | **Flag, do not change** |
| Month labels (for example `May'25`, `Sept'25 ~ Nov'25`) | **Flag, do not change** |
| Any other format: `27/03/2026`, `2026-03-27`, `2026.03.27`, `Mar 27, 2026`, two-digit years, date ranges | **Flag, do not change** |

For every flagged date, add a **suggestion to standardize** it to MM/DD/YYYY. Never change it yourself. Never guess a missing year or the order of day and month.

---

## 6. English text line-wrapping (Asian Typography)

HDharmony B and Hyundai Sans Text KR are designed for Korean. When they are used for English text, words can break in the middle when the line wraps.

**For every non-empty paragraph that contains English (Latin) characters, digits or symbols, turn OFF:**

`Paragraph > Asian Typography > Allow Latin text to wrap in the middle of a word`

- In the XML this is the `latinLnBrk` attribute on the paragraph properties (`a:pPr`). Set it to `"0"`.
- Apply it to every such paragraph in text boxes, shapes, groups and table cells, on every slide except hidden slides, the cover and section dividers.
- **Placeholder text such as `XXX` or `[Description]` is included.**
- **Skip** empty paragraphs. **Slides only**: do not touch speaker notes, layouts or the master.
- A paragraph with only digits or symbols (for example `3/27/2026`) counts.
- Set the attribute explicitly on each paragraph. Do not rely on inheritance from the master.
- Report it as a change only when the value actually changed.

---

## 7. What to ignore

Do not check, flag or change any of the following:

- Confidentiality notices and classification tags (such as "사내한 (Restricted)").
- The grey horizontal line below the title (#AEAEAE, 0.3", 0.76") and the blue vertical line beside the description (0.3", 0.9"). They come from the master slide, or on some slides from a small slide-level rectangle. Do not move or restyle them.
- Placeholder text such as `XXX`, `X`, `[Description]`, `[ Image ]`, `Step X – Step Description` and `Insert Title here`: its **wording** stays as is. (Fonts and the Latin-wrap rule in section 6 still apply to it.)
- Eyebrow labels above the title.
- Status wording (such as Pass or Scheduled).
- The language of the content. Do not translate or rewrite.
- Table header fill and size, and table sizes.
- Slide size.
- Speaker notes.

---

## 8. Moving, resizing and alignment

### 8.1 Moving the title and description

If a title or brief description is not at its exact position (section 2):

1. Check whether moving it would **overlap another shape**, or leave **less than 0.1" of space** between their edges.
2. If not, **move it** and report the old and new position.
3. If it would, **do not move it**. Flag it for review and say which shapes would collide.

Apply the font, size, color and bold fixes even when a move is held back.

### 8.2 Overlap

- **Text must never overlap other text.** If text overlaps, separate the shapes, or flag it when the separation is not safe.
- Keep a small space (at least 0.1") between shapes, tables and boxes.
- The master's lines are not obstacles.

### 8.3 Resizing

Change X and Y, and also the width and height, **only when the text exceeds its text box**. Check this only when both fonts are installed (section 9). If the fonts are missing, leave the box as it is and say so in the report.

### 8.4 Alignment

Boxes, tables and shapes should be **aligned to each other on a slide and across slides**: shared left edges, shared tops and equal gaps in a row or column.

- Flag near-misses (for example two boxes in a column whose left edges differ by a few hundredths of an inch).
- Move a shape for alignment only when it is the title or description being moved under 8.1. Flag everything else.

---

## 9. Font availability warning

Check whether `HDharmony B` and `Hyundai Sans Text KR` are installed on the machine running the check (Windows: the font registry under `HKLM` and `HKCU` `SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts`). The "Hyundai Sans Text Office" and "Head Office" files bundled in this folder are a different family and do not count.

- The required fonts are bundled in the `fonts/` folder of this skill (HDharmony B/L/M, Hyundai Sans Text KR and Head KR). If one is missing, tell the user they can install it from there. Do not install fonts without asking.
- If either font is missing, add a **warning at the top of the report**. Say that the font names were set correctly, but rendering, text fit and overflow could not be verified.
- Do not stop or skip fixes because of this warning. Leave box sizes as they are (section 8.3).

---

## 10. Workflow

1. Open the .pptx file and confirm it can be saved.
2. Classify each slide (hidden, cover, divider, checklist, other). Skip the hidden and exempt slides.
3. For each remaining slide, check the rules in sections 2 to 6 and 8.
4. Apply the safe fixes and record every change.
5. Record everything that needs human judgment as "flagged, not changed".
6. Save the result as **`<original name>_checked.pptx`** next to the original. Never overwrite the original. If the `_checked` file already exists, ask before replacing it.
7. Produce the report (section 11).

---

## 11. Report format

Write the report **in English**. Always end with a report in this structure.

### Summary
- Slides reviewed, slides skipped (hidden, cover, divider) and number of changes made
- Font availability warning, if any

### Changes made

**Group changes per slide and element.** One row can cover many runs. Put the count in the Before/After columns when there are many (for example "37 paragraphs").

| Slide | Element | Rule | Before | After |
|---|---|---|---|---|
| 5 | Title | Font size | 28 pt | 24 pt |
| 10 | Table header | Bold | On | Off |
| 12 | Date text | Date format | 3/27/2026 | 03/27/2026 |
| 14 | Body text | Latin wrap | On (37 paragraphs) | Off |

### Flagged for review (not changed)

| Slide | Element | Issue | Why it was not changed |
|---|---|---|---|
| 7 | Date text | `13 Feb` has no year. Suggest MM/DD/YYYY. | The year would have to be guessed |
| 8 | Title | Would overlap the picture if moved | Move may damage the layout |

If nothing was changed or flagged, say so plainly.

---

## 12. Implementation reference

`helpers.py` in this folder holds small, mechanical python-pptx helpers (set both typefaces, bold off, size, color, `latinLnBrk`, exact EMU comparison, and a walk over every text frame in shapes, groups and tables). It makes no judgments: slide classification, the header/body decision, dates, overlap and alignment follow this document, not the code. Use it where it helps and ignore it where it does not.

Gotchas the code already handles:
- `Inches(x)` truncates, so compare and set positions in EMU constants. The checklist title is at 155448 x 271681 EMU, which is not a round inch value.
- A run with no `a:latin` element would crash a naive `a:ea` insert.
- Table and group text is not reachable through `slide.shapes` alone.
- Fields (`a:fld`) and empty-paragraph end properties (`a:endParaRPr`) are not runs.

---

## 13. Behavior rules

- **Never edit content.** Only change formatting, plus date and stage-title text where the rules above allow it.
- **Never guess.** If the correct fix is not clear, flag it.
- **Never overwrite the original file.** Save as `_checked.pptx`.
- **Always give the report**, even when nothing needed changing.
