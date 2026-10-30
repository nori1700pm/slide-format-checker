# Chat History: Setting up and testing the slide-format-checker skill

Date: 2026-10-29
Working folder: `C:\Users\HMGICS\Desktop\zheqing\Slides-Reformatter`

This is a condensed, readable record of the whole conversation. Tool output is summarized, not reproduced in full.

---

## 1. How to add a skill

**Question:** How do I add a skill? I have a `SKILL.md`, one .pptx template and one font package.

**Answer:**
- A skill is a folder named for the skill, containing `SKILL.md` and any supporting files.
- Project-only location: `<project>\.claude\skills\<skill-name>\`
- All-projects location: `C:\Users\HMGICS\.claude\skills\<skill-name>\`
- `SKILL.md` needs frontmatter with `name` and `description`. The description decides when the skill is triggered.
- Reference bundled files by relative path inside `SKILL.md`.
- Claude Code does not install fonts. If they must be on the system, install them in Windows.
- Restart Claude Code so it picks up the new skill.

## 2. Installing the skill

At that point the folder contained only `SKILL.md`. The template and font package were not there yet.

Action taken: copied `SKILL.md` to `.claude\skills\slide-format-checker\SKILL.md` (folder name matches `name: slide-format-checker`). The original was left in place.

Skill summary (from `SKILL.md`): a final formatting check on existing company slides. It flags deviations, auto-fixes safe ones, and reports every change.
- Cover and section-divider slides are exempt. Checklist slides get font family, date and Latin-wrap rules only.
- Title: HDharmony B, 24 pt, #002060, at (0.3", 0.25"), bold off.
- Brief description: HDharmony B, 18 pt, #002060, at (0.5", 0.95"), bold off.
- Body: Hyundai Sans Text KR, 12 pt (or 11 pt if 12 does not fit).
- Stage titles use the format `L{stage}-{no}: Title`.
- Dates use MM/DD/YYYY.
- "Allow Latin text to wrap in the middle of a word" is turned off (`latinLnBrk="0"`).
- Unclear cases are flagged and not changed. The output is saved as a new file.

## 3. Clarifying questions about SKILL.md

Questions raised before testing:
1. Which text counts as "body" (table cells, footnotes, captions, charts, groups)?
2. What counts as a "header"?
3. Should other fonts (Arial, Calibri) be replaced with Hyundai Sans Text KR?
4. On checklist slides, are the title font, position and stage format handled?
5. How should the overlap check before moving a shape work?
6. Date edge cases: `27/03/2026`, `2026-03-27`, `Mar 27, 2026`, ranges, dates inside sentences.
7. Do digit-only paragraphs count as Latin? Should empty paragraphs and notes be skipped?
8. How should overflow be judged when the fonts are not installed?
9. What should the output file be named? (Default proposed: `<original>_checked.pptx`.)
10. Should hidden slides be checked?

Also suggested: tidy `SKILL.md`, which has blank lines between nearly every line. This has not been done.

## 4. First look at `testing.pptx` (read-only audit)

Files found in the skill folder: `SKILL.md`, `testing.pptx`, `Windows (TTF)\` (Hyundai Sans Head and Text Office fonts), and the `★SDF DX Stage-Gate Document Template_v6_WIP_DRAFT_Formatted.pptx` template.

Setup:
- Installed `python-pptx` (it was missing).
- Worked on a copy in `C:\Users\HMGICS\Desktop\zheqing\checker-work\` (scripts in `scripts\`, copy of the deck in `in\`, results in `out\`). The original was not modified.
- Ran a read-only audit first and made no changes.

### Audit findings (19 slides)

| Rule | Result |
|---|---|
| Slide type | All 19 slides use the same blank layout. No cover, divider or checklist slides found. Slide 1 is a content slide. |
| Stage titles | None. Titles start with ①–⑥, so all slides are non-stage. |
| Title position | All titles at (0.299", 0.196"). Required (0.3", 0.25"). |
| Title bold | On for all 19. Required: off. |
| Title size and color | 24 pt, #002060. Pass. |
| Body text size | 2,983 runs below 11 pt (mostly 8–9 pt). 82 runs above 12 pt. |
| Latin wrap | 912 paragraphs with English text did not have it turned off. |

Other observations:
- Many fonts are stored under Korean names: `현대하모니 B` (39 runs), `현대산스 Text` (973), `현대하모니 M` (135, mostly the "Restricted" tag).
- The skill requires the exact names `HDharmony B` and `Hyundai Sans Text KR`.
- No HDharmony font was found on the machine or in the bundled package. Hyundai Sans Text KR is installed.
- Slide size is 10.83" x 7.5" (the skill says to ignore size).

Decisions that were put to the user:
1. **Body size:** Forcing about 3,000 runs from 8–9 pt to 11 pt would likely overflow the dense slides. Proposal: apply font, bold, position and Latin-wrap fixes, and list size issues for review.
2. **Font names:** Are the Korean names aliases of the required English names?
3. **Missing HDharmony:** The title font cannot be verified here.
4. **Slide size:** Noted only.

## 5. The `charmap` error

During the audit, this error appeared briefly:

```
UnicodeEncodeError: 'charmap' codec can't encode character '\ube48' in position 24
```

**What it was:** Python on Windows printed to the terminal using the cp1252 (Western European) encoding, which has no Korean characters. `\ube48` is the Korean character 빈, taken from the slide layout name `1_빈 화면`. When the script tried to print it, Python could not convert it.

**Was it a problem with the deck?** No. It was a display problem in the inspection script. The deck was not affected.

**Fix:** The script now prints as UTF-8 (`sys.stdout.reconfigure(encoding="utf-8")`). Setting `PYTHONIOENCODING` first did not work because the script ran with Python's `-I` (isolated) flag, which ignores environment variables.

## 6. Handling Korean text in the future

Options given:
1. **Add a note to `SKILL.md` (recommended):** tell the skill to call `sys.stdout.reconfigure(encoding="utf-8")` in any script that prints slide text on Windows, and to use `encoding="utf-8"` for text files. Not yet added; offered.
2. **Set `PYTHONUTF8=1`** with `setx PYTHONUTF8 1` and open a new terminal. This does not help scripts run with `-I`.
3. **Run `chcp 65001`** in the terminal for one session (display only).

Notes: editing the .pptx itself handles Korean correctly. Garbled text can appear in terminals, or in text/CSV files opened without UTF-8. For saved reports, use UTF-8 Markdown or .docx.

## 7. Applying the fixes

**User's answers:**
- Question 1 (body size): "do as you see fit." Chosen approach: apply font, bold, position and Latin-wrap fixes, and only flag size problems.
- Question 2 (font names): "proceed with Hyundai Sans Text for now," and the user is trying to get the HDharmony font. Interpretation: rename `현대산스 Text` to `Hyundai Sans Text KR`, and leave HDharmony typeface names alone until the font is available.

**Extra checks before editing:**
- No real dates. `01.1`, `71.3`, `1st Action` and `10 Gbps` are not dates.
- All runs above 12 pt outside titles are `TextBox 9` (14 pt header line) and the 18 pt "Need to translate KR Ver later" notes. None are body text, so they were left.
- Table header cells (121 runs) use `현대산스 Text`, bold on. Left for the HDharmony decision.

**Result:** 2,704 edits, saved as a new file. The original was not touched.
- **Body typeface:** about 1,730 runs now use Hyundai Sans Text KR for both the Latin and East Asian settings (previously `현대산스 Text` or `Malgun Gothic`).
- **Latin wrap:** turned off in 912 paragraphs that contain English text.
- **Titles:** all 19 moved to (0.3", 0.25") with bold off. No overlap with other shapes (the "Restricted" tag is ignored by the skill).

**Verification:** the saved file was reopened with `python-pptx`, passes the zip integrity check, has the same shape counts as the original, and the audit shows the fixed items are fixed. It was not opened in PowerPoint, so the visual result is unchecked.

**Flagged, not changed:**
- Body text below 11 pt: 2,824 runs (mostly 8–9 pt). Slides 4, 8, 11, 14 and 18 have the most (314–375 each). Needs a manual decision.
- Titles still use `현대하모니 B`, and table headers (121 runs) still use `현대산스 Text` with bold on. Both wait on HDharmony B.
- `TextBox 9` looks like the page brief description but is 14/12 pt `현대하모니 M`, bold, at (0.299", 0.925"). The rule says 18 pt HDharmony B at (0.5", 0.95"). Not certain it is the same element.
- "Need to translate KR Ver later" notes on slides 4, 8, 11 and 14 (18 pt). Look like placeholder notes.
- Slide 1 is a content slide, not a cover.

**Open question asked at the end:** Should `TextBox 9` be treated as the brief description once HDharmony B is available? (No answer yet.)

## 8. Files

In `C:\Users\HMGICS\Desktop\zheqing\Slides-Reformatter\`:
- `SKILL.md`: the original skill file.
- `.claude\skills\slide-format-checker\`: installed skill, with `SKILL.md`, `testing.pptx`, the font package and the template.
- `testing_checked.pptx`: the fixed copy.
- `testing_checked_report.md`: the full format-check report.
- `chat-history.md`: this file.

In `C:\Users\HMGICS\Desktop\zheqing\checker-work\`:
- `scripts\`: `inspect_pptx.py`, `audit.py`, `explore.py`, `big.py`, `fix.py`.
- `in\testing.pptx`: working copy of the test deck.
- `out\`: the fixed deck and report.

## 9. Still to do
- Get the HDharmony B font, then apply the title and header typeface names and the table-header bold fix.
- Decide how to handle body text below 11 pt.
- Decide whether `TextBox 9` is the brief description.
- Optionally add the UTF-8 note to `SKILL.md`, and tidy its blank lines.
- Open `testing_checked.pptx` in PowerPoint to check the layout visually.
