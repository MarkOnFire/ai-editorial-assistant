# Getting Started with the Automated Workflow

## What Changed?

The repository now ships with a filesystem watcher and structured project folders so every Media ID lives in one place. Drop files in `transcripts/` and the automation creates deliverables in `output/<MEDIA_ID>/` with subfolders for drafts and SEMRush captures.

### Updated Folder Map

```
editorial-assistant/
├── transcripts/                # Drop source transcripts here
│   └── archive/                # Auto-archived transcripts (90+ days)
├── output/
│   ├── archive/                # Archived projects
│   └── <MEDIA_ID>/             # Live project workspace
│       ├── 01_brainstorming.md
│       ├── 02_copy_revision.md
│       ├── 03_keyword_report.md
│       ├── 04_implementation.md
│       ├── 05_formatted_transcript.md
│       ├── 06_timestamp_report.md
│       ├── drafts/             # Drop draft screenshots or text here
│       ├── semrush/            # Drop SEMRush screenshots here
│       └── workflow.json       # Automation run log
├── automation/                 # Watcher + archival scripts
├── system_prompts/             # Phase-specific prompts
├── knowledge/                  # Reference PDFs and guides
└── .claude/commands/           # Slash command wrappers
```

## First Run

1. **Install dependencies** (one time):
   ```bash
   ./venv/bin/pip install watchdog anthropic pyyaml
   ```

   Or activate the virtual environment first:
   ```bash
   source venv/bin/activate
   pip install watchdog anthropic pyyaml
   ```

2. **Start the watcher**:
   ```bash
   ./venv/bin/python -m automation.watcher --config automation/config.yaml
   ```

   Or if you activated the venv:
   ```bash
   python -m automation.watcher --config automation/config.yaml
   ```
3. **Drop a transcript** into `transcripts/` (e.g., `2POL0000HD_ForClaude.txt`).
4. Watcher output will confirm generation of `01_brainstorming.md`, `05_formatted_transcript.md`, and `06_timestamp_report.md` inside `output/2POL0000HD/`.
5. Open the new files in your editor to review.

## Triggering Remaining Phases

- **Copy Revision**: Save a draft screenshot or markdown copy to `output/<MEDIA_ID>/drafts/`. The watcher refreshes `02_copy_revision.md` automatically.
- **Keyword Research**: Save a SEMRush screenshot to `output/<MEDIA_ID>/semrush/`. The watcher generates `03_keyword_report.md` and `04_implementation.md`.
- **Manual Reruns**: Use `/revise`, `/brainstorm`, `/format-transcript`, or `/create-timestamps` when you want an interactive pass or to reprocess after edits.

## Using `/revise`

Running `/revise` in Claude Code now shows the 15 most recent Media IDs from `output/` along with phase statuses pulled from `workflow.json`. Choose a project to run Phase 2 interactively, or provide a specific Media ID like `/revise 2POL0000HD` to jump straight in.

## Archival Cleanup

Add a cron entry to prune stale work:
```bash
0 3 * * * cd /path/to/editorial-assistant && /usr/bin/env python3 automation/archive.py --config automation/config.yaml
```
Use `--dry-run` during setup to preview what will be moved to `output/archive/` and `transcripts/archive/` after 90 days of inactivity.

## Tips

- Keep filenames Media ID–first so automation can discover them.
- Use `workflow.json` to track what ran and when; it is safe to edit if you need to annotate anomalies.
- For experimentation, create a sandbox project like `output/TEST0001HD/` and feed it mock inputs.
- Remember to stop the watcher (`Ctrl+C`) when you are done.
