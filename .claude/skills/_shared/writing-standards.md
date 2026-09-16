# Shared: Report Location, Writing Standards & Output Rules

Used by both `generate-work-summary` and `generate-performance-analysis`.

## Report location

Generate the report as Markdown in a year subfolder under `reports/`:

- **Year folder:** `reports/[YYYY]/`, where `[YYYY]` is the calendar year of the period **start** date (e.g., `2025-01-01` to `2025-06-30` → `reports/2025/`; `2025-11-01` to `2026-02-28` → `reports/2025/`). Create the subfolder if it does not exist.

## Regeneration policy (overwrite vs. version)

A report filename is derived from the report type and date range, so regenerating the same period targets the same file. When that file already exists:

- **Overwrite by default.** Regenerating a report for the same period replaces the existing file in place — no `-v2` suffixes, no timestamped copies.
- **Ask first if the existing report is older than 7 days.** Report the file's last-modified date and ask whether to overwrite it or save alongside it. A report that old may already have been shared or annotated.
- **Ask first if the existing report was hand-edited.** If the file's structure deviates from the skill's template (missing required sections, added notes), treat it as hand-edited and ask before overwriting, regardless of age.
- **Save alongside, when the user chooses that:** use `reports/[YYYY]/[report-type]-[date-range]-[YYYY-MM-DD].md`, where the suffix is the regeneration date. Never delete the original.
- **Always state the outcome** in the completion message: full path, and whether the file was created, overwritten, or saved alongside an existing one.

## Writing Standards

- **Tone:** Neutral, evidence-based, no flattery. Describe accomplishments and gaps honestly.
- **Format:** Past tense, strong verbs, one sentence per bullet, 3-10 bullets per section
- **Markdown:** Valid structure, no placeholders, no empty sections
- **Evidence:** Link to Jira keys (e.g., "EDU-123") and PR numbers (e.g., "PR #456") in parentheses

## Output Rules

- Output **only** Markdown reports, no meta text
- Use the exact heading structure defined in the report's skill
- No placeholder text (e.g., "Accomplishment 1")
- Valid Markdown hierarchy (# → ## → ###)
- Neutral, evidence-based tone throughout
- Save to `reports/[YYYY]/` (year from period start date); create the year subfolder if needed; inform the user when complete with full paths

**Priority:** Faithful reflection of activities, clarity, balanced view of accomplishments and development areas.
