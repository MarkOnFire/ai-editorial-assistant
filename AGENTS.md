# Repository Guidelines

## Project Structure & Module Organization
Keep work inside the existing phase folders so Claude commands can stitch everything together. Use `transcripts/` for active `.txt` sources, `draft_copy/` and `semrush/` for screenshots, and archive completed assets in their respective `archive/` subfolders. AI outputs belong in `output/<MEDIA_ID>/` where each phase writes sequential Markdown (`01_brainstorming.md` through `06_timestamp_report.md`). Prompt logic lives in `system_prompts/`, while executable command specs are in `.claude/commands/`. Reference PDFs and style sheets stay in `knowledge/`; do not sync them elsewhere.

## Build, Test, and Development Commands
This repository is driven from Claude Code slash commands rather than npm or make targets. Trigger deliverables with `/brainstorm transcripts/<MEDIA_ID>_ForClaude.txt`, `/revise <MEDIA_ID>`, and `/research-keywords <MEDIA_ID>`; optional refinements come from `/format-transcript` and `/create-timestamps`. When verifying results locally, inspect outputs with `cat output/<MEDIA_ID>/01_brainstorming.md` or your editor of choice, and use `ls -t transcripts/*.txt` to confirm the latest transcript before invoking commands.

## Coding Style & Naming Conventions
Write prompts and documentation in Markdown with short, imperative headings and 80–120 character paragraphs for readability. Follow AP Style guidance already embedded in `system_prompts/` unless a program-specific override is required. File names must lead with the Media ID, e.g., `2WLI1203HD_ForClaude.txt`, `2WLI1203HD_draft.png`, and `2WLI1203HD_semrush.png`. Keep archive filenames untouched so historical outputs remain traceable.

## Testing Guidelines
There is no automated test suite; quality control means re-running the relevant slash command. Before committing, spot-check character counts, verify quotes and keywords match the transcript, and confirm media-specific rules via `knowledge/Media ID Prefixes.md`. When adjusting prompts, generate a fresh sandbox Media ID (e.g., `output/EXAMPLE_2WLI1203HD/`) to ensure downstream phases still format correctly.

## Commit & Pull Request Guidelines
Write commit messages in the format `area: concise action` (e.g., `prompts: tighten phase1 seo checklist`). Pull requests should summarize the goal, note any prompt side effects, link to related tickets or media IDs, and attach before/after snippets or screenshots when changing output structure. Flag any manual migrations (such as archive moves) so reviewers can replicate them.

## Agent-Specific Tips
Use `.claude/commands/` as the single source of truth for workflow automation; update both the command file and matching prompt when behavior changes. Preserve sensitive PDFs in `knowledge/` and avoid uploading them to shared issue trackers. For large transcript imports, batch them and let teammates know via PR description to prevent accidental duplication.
