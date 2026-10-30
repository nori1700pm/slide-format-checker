# Format check report: testing.pptx

## Summary

- Slides reviewed: 19. Slides skipped (cover/divider): 0 (none identified; see flags).
- Total edits: 2704. Output saved as a new file: `testing_checked.pptx` (original untouched).
- **Font warning:** HDharmony B was not found on this machine or in the supplied font package. Title and header typeface names were left as they are, and rendering, fit and overflow could not be verified. Hyundai Sans Text KR is installed.

## Changes made

| Slides | Element | Rule | Before | After | Count |
|---|---|---|---|---|---|
| 3 | Body text | Typeface (ea) | Malgun Gothic | Hyundai Sans Text KR | 3 |
| 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 | Body text | Typeface (ea) | 현대산스 Text | Hyundai Sans Text KR | 877 |
| 7 | Body text | Typeface (latin) | inherited | Hyundai Sans Text KR | 1 |
| 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 | Body text | Typeface (latin) | 현대산스 Text | Hyundai Sans Text KR | 852 |
| 4, 8, 9, 10, 11, 14, 19 | Paragraphs with English text | Latin wrap (latinLnBrk) | 1 | 0 (off) | 7 |
| 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 | Paragraphs with English text | Latin wrap (latinLnBrk) | inherited/on | 0 (off) | 905 |
| 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 | Title | Bold | On | Off | 40 |
| 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19 | Title | Position | (0.299", 0.196") | (0.300", 0.250") | 19 |

## Flagged for review (not changed)

| Slide | Element | Issue | Why it was not changed |
|---|---|---|---|
| all | Body text size | 2824 runs are below 11 pt (5 to 9 pt, mostly 8 to 9 pt); per slide: S1:114, S2:90, S3:92, S4:331, S5:70, S6:80, S7:24, S8:322, S9:85, S10:70, S11:375, S12:86, S13:118, S14:314, S15:92, S16:95, S17:25, S18:349, S19:92 | Raising to 11 pt would likely overflow dense tables and boxes. Needs manual decision |
| all | Title and header typeface | Titles use `현대하모니 B`; the required name is `HDharmony B` | Font not available yet; name left as is |
| all | Table header cells | 121 runs are `현대산스 Text` (not renamed), bold on; rule calls for HDharmony B, bold off | Waiting on HDharmony B |
| all | `TextBox 9` (line under the title) | Looks like the page brief description but is 14/12 pt `현대하모니 M`, bold, at (0.299", 0.925"); rule is 18 pt HDharmony B at (0.5", 0.95") | Not certain it is the brief description |
| 4, 8, 11, 14 | Note "Need to translate KR Ver later" | 18 pt `현대하모니 M` | Looks like a placeholder note |
| 1 | Slide type | First slide is a content slide, not a cover | Skill says first slide = cover; not guessed |

## Not applicable

- Dates: no real dates found (`01.1`, `71.3`, `1st Action`, `10 Gbps` are not dates).
- Stage titles: none; titles start with ①–⑥.
- Slide size (10.83" x 7.5"): ignored per skill.