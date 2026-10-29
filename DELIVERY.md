# S01E03 — what it shows

Prepared by Codex, harness for Adrian Śledziewski. Reviewed 2026-10-29 against the course's 2026-10-04 export.

- Signed source: `src/routes/kb/decisions/0002-dedupe-by-cause/+page.svx`
- Standalone rendered page: `rendered/4471-review.html`
- Complete interactive static KB: `build/` (serve as a directory; no hosting service is required)
- Engineering issue drafts and full report-ID lists: `issue-drafts/parser.md`, `issue-drafts/hint.md`

The supplied classifier gives 379 parser reports and 33 hint reports, after deduplicating 427 export rows to 412 report IDs. Five reports included under parser have unresolved timing/one-day evidence; the page does not claim a proven root cause for them. Intake labels disagree with tenant-based classification on 38 reports.

The new decision was adopted in this challenge's KB after human review. Active triage and the incident summary now link both issues and use cause-based merging. Original accepted decision 0001 remains intact as history. Closing the challenge tickets marks the analysis and KB handoff complete; production fixes and investigation of exceptional reports remain desk work.

## Rebuild

Run `npm ci`, then `npm run build`. The lockfile records the installed dependency versions. Run `npm run preview` to inspect the interactive KB locally.

The standalone HTML is the prerendered decision page with its generated CSS inlined. Its example controls are a static snapshot; use the complete build for interaction. All report counts in the source page are computed by components from the report data and tenant directory. The desk reference is computed from the frontmatter signature by the supplied RenderStamp component.

No application parser or production booking form was changed. The export documents behavior; it does not include either production implementation.
