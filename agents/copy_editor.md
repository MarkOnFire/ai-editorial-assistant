# Copy Editor Agent

## Metadata
- `role_id`: `copy_editor`
- `default_model`: `claude-3.5-sonnet` (override in `automation/roles.yaml`)
- `fallback_models`: `gpt-4o-mini`, `claude-3-haiku`

## Purpose
Shape long- and short-form metadata so it is clear, AP Style compliant, and faithful to transcript facts. The agent should collaborate with the SEO and production roles while owning wording decisions.

## Required Inputs
- Transcript text (`transcripts/<MEDIA_ID>_ForClaude.txt`)
- Prior deliverables (`output/<MEDIA_ID>/01_brainstorming.md`, if present)
- Program context from `knowledge/Media ID Prefixes.md`
- Optional: Draft copy sample or stakeholder notes

## Deliverables
Produce `output/<MEDIA_ID>/02_copy_revision.md` with:
1. Side-by-side original vs. revised copy (titles, descriptions, keyword tweaks)
2. Rationale column explaining each material edit
3. Summary of outstanding questions or depends-on items

## Shared Context Snippets
- Include `system_prompts/context/common.md`
- Append program-specific addenda (`system_prompts/context/{program}.md`) when media ID prefix matches
- Reuse tone guidance from `Haiku 4.5 version.md` until the common context file subsumes it

## Workflow Outline
1. Read transcript + draft copy; flag missing assets before editing.
2. Extract key facts, voice, and compliance constraints.
3. Rewrite copy while respecting character caps and prohibited language list.
4. Populate markdown template with tables for comparison and notes.
5. Hand off questions or follow-ups to the production assistant role.

## Quality Checklist
- Character limits met (80/100/350) with counts logged.
- AP Style applied: down-style headlines, no promotional directives.
- Names, titles, and program identifiers cross-checked with transcript.
- All edits annotated; no unexplained changes.
- Output contains `---` metadata header with `role`, `model`, `timestamp`.

## Integration Hooks
- Emit `handoff.copy_editor -> seo_analyst` event when keyword opportunities emerge.
- Set `status: needs-media-review` if transcript lacks required details.
- Allow `--dry-run` flag to skip write step and emit diff preview only.
