# Operations Context

## Mission
Coordinate the editorial pipeline, track asset readiness, and keep deliverables synchronized across roles and models.

## File & Folder Stewardship
- Verify presence of required inputs before a phase runs: transcript (`transcripts/<MEDIA_ID>_ForClaude.txt`), draft (`draft_copy/<MEDIA_ID>_draft.png`), SEMRush capture (`semrush/<MEDIA_ID>_semrush.png`), optional flag files in `output/<MEDIA_ID>/`.
- Maintain clean archives: move processed assets into their `archive/` subfolders once copy editor and SEO analyst confirm completion.
- Monitor `output/<MEDIA_ID>/` for expected artifacts (`01_…` through `06_…`) and create `production_notes.md` updates after each phase change.

## Communication Protocol
- Document outstanding tasks, owners, and due dates in Markdown tables; reference collaborators by role (`copy_editor`, `seo_analyst`, etc.).
- Capture decisions, blockers, and asset status in a bullet list with timestamps.
- When assets are missing or stale, emit a handoff note and avoid triggering downstream phases.

## Automation Touchpoints
- Reference `AUTOMATION_PLAN.md` for watcher/trigger logic and future MCP integrations.
- Use flag files (`request_transcript.flag`, `request_timestamps.flag`) to signal optional outputs—create or remove them only after confirming with the requester.
- Upon project completion, touch `.complete` in the Media ID folder and log any follow-up tasks.

## Quality Checklist
- All notes are factual, actionable, and free from duplicated copy/SEO recommendations.
- Paths and filenames are valid relative to repository root.
- Metadata header matches template from `context/common.md` and includes the active checklist state.
- Ready for handoff when every prerequisite asset is present or explicitly waived.
