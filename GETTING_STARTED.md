# Getting Started with Claude Code Workflow

## What Just Happened?

Your repository has been restructured for file-based workflow with Claude Code! Here's what changed:

### New Folder Structure

```
claude-editorial-assistant/
├── transcripts/              ✅ Existing (unchanged)
├── draft_copy/               ✨ NEW - Drop draft screenshots here
├── semrush/                  ✨ NEW - Drop SEMRush screenshots here
├── output/                   ✨ NEW - Generated reports appear here
├── system_prompts/           ✨ NEW - Modular phase prompts
├── .claude/commands/         ✨ NEW - Slash commands
├── knowledge/                ✅ Existing (unchanged)
├── README.md                 ✨ NEW - Complete workflow guide
├── CLAUDE.md                 ✅ Updated
└── AUTOMATION_PLAN.md        ✨ NEW - Future automation roadmap
```

### New Slash Commands Available

- `/brainstorm` - Generate initial metadata from transcript (Phase 1)
- `/revise` - Review draft copy and provide edits (Phase 2)
- `/research-keywords` - SEO research with keyword analysis (Phase 3)
- `/format-transcript` - Create AP Style formatted transcript
- `/create-timestamps` - Generate chapter markers for long videos

## Test Drive: Process Your First Transcript

Let's walk through the workflow with one of your existing transcripts:

### Step 1: Pick a Test Transcript

```bash
# List available transcripts
ls -1 transcripts/*.txt
```

Choose one to test with (e.g., `2POL0000HD_REV20250825_ForClaude.txt`)

### Step 2: Generate Brainstorming Document

In Claude Code, run:
```
/brainstorm transcripts/2POL0000HD_REV20250825_ForClaude.txt
```

This will:
- Analyze the transcript
- Extract Media ID (2POL0000HD)
- Generate title/description options
- Create `output/2POL0000HD/01_brainstorming.md`

### Step 3: Review the Output

```bash
# Open the generated brainstorming document
code output/2POL0000HD/01_brainstorming.md
```

or

```bash
cat output/2POL0000HD/01_brainstorming.md
```

Check that:
- ✅ Title options are 80 chars or less
- ✅ Short descriptions are 100 chars or less
- ✅ Long descriptions are 350 chars or less
- ✅ Keywords match transcript content
- ✅ Character counts are exact

### Step 4: (Optional) Test Phase 2

If you have draft metadata already:

1. Take a screenshot of your draft in Media Manager
2. Save it as `draft_copy/2POL0000HD_draft.png`
3. Run `/revise 2POL0000HD`
4. Review `output/2POL0000HD/02_copy_revision.md`

### Step 5: (Optional) Test Phase 3

If you have SEMRush data:

1. Take a screenshot of SEMRush keyword analysis
2. Save it as `semrush/2POL0000HD_semrush.png`
3. Run `/research-keywords 2POL0000HD`
4. Review `output/2POL0000HD/03_keyword_report.md` and `04_implementation.md`

## Command Reference

### Basic Usage

All commands can be run with or without arguments:

**With explicit file path:**
```
/brainstorm transcripts/2WLI1203HD_ForClaude.txt
```

**With just Media ID:**
```
/revise 2WLI1203HD
```

**With no arguments (finds most recent):**
```
/brainstorm
```

### Command Details

#### `/brainstorm`
- **Input**: Transcript file
- **Output**: `output/[MEDIA_ID]/01_brainstorming.md`
- **Use when**: Starting work on a new video

#### `/revise`
- **Input**: Transcript + draft screenshot or text
- **Output**: `output/[MEDIA_ID]/02_copy_revision.md`
- **Use when**: You've drafted metadata and want editorial review

#### `/research-keywords`
- **Input**: Transcript + SEMRush screenshot + web research
- **Output**: `output/[MEDIA_ID]/03_keyword_report.md` + `04_implementation.md`
- **Use when**: You need SEO optimization and keyword analysis

#### `/format-transcript`
- **Input**: Transcript file
- **Output**: `output/[MEDIA_ID]/05_formatted_transcript.md`
- **Use when**: You need a publication-ready formatted transcript

#### `/create-timestamps`
- **Input**: Transcript file (15+ minute videos only)
- **Output**: `output/[MEDIA_ID]/06_timestamp_report.md`
- **Use when**: You need chapter markers for Media Manager and YouTube

## File Naming Tips

### For Transcripts
Already follows convention: `[MEDIA_ID]_ForClaude.txt`
- Example: `2WLI1203HD_ForClaude.txt`

### For Draft Screenshots
Use: `[MEDIA_ID]_draft.png`
- Example: `2WLI1203HD_draft.png`

### For SEMRush Screenshots
Use: `[MEDIA_ID]_semrush.png`
- Example: `2WLI1203HD_semrush.png`

**Why this matters**: The Media ID links everything together automatically!

## What About the Old Workflow?

The original system prompt is preserved at `system_prompts/current.md` and you can still use it in the chat interface if needed. But the new workflow offers:

✅ **Better organization** - All outputs grouped by Media ID
✅ **Faster processing** - Slash commands vs. copy-pasting prompts
✅ **Screenshot support** - Built-in vision for reading draft copy
✅ **Modular prompts** - Easier to customize individual phases
✅ **Future-ready** - Foundation for full automation (see AUTOMATION_PLAN.md)

## Next Steps

1. **Test with 2-3 transcripts** to get comfortable with commands
2. **Customize if needed** - Edit files in `system_prompts/` to adjust output style
3. **Create your workflow** - Find what sequence works best for your needs
4. **Archive completed work** - Move processed files to archive folders
5. **Consider automation** - Read AUTOMATION_PLAN.md for ideas

## Troubleshooting

**Q: Command not working?**
- Make sure you're in Claude Code (not desktop chat app)
- Check you're in the repository directory
- Try `/help` to see available commands

**Q: Output quality not good?**
- Check that the correct program-specific rules are being applied
- Verify Media ID is recognized in `knowledge/Media ID Prefixes.md`
- Edit the relevant system prompt file to adjust instructions

**Q: Screenshot not reading correctly?**
- Ensure screenshot is clear and shows all metadata fields
- Try providing text manually instead
- Check filename follows convention: `[MEDIA_ID]_draft.png`

**Q: Character counts seem wrong?**
- All counts include spaces (standard)
- Use the exact count from AI output
- CMS may count differently - trust the AI counts

## Questions?

- **General workflow**: See README.md
- **Technical details**: See CLAUDE.md
- **Future automation**: See AUTOMATION_PLAN.md
- **Improvements**: Add to Possible improvements.md

---

**Ready to start?** Try `/brainstorm` with one of your existing transcripts!
