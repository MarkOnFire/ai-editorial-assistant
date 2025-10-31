# PBS Wisconsin Video Editorial Workflow Guide

Complete guide to the automated editorial assistant workflow, from transcript to publication.

---

## Table of Contents

1. [Overview](#overview)
2. [Getting Started](#getting-started)
3. [Phase-by-Phase Walkthrough](#phase-by-phase-walkthrough)
4. [Decision Points](#decision-points)
5. [Quality Standards](#quality-standards)
6. [Troubleshooting](#troubleshooting)
7. [Advanced Features](#advanced-features)

---

## Overview

### What This System Does

Transforms raw video transcripts into production-ready metadata:
- **Titles** (80 chars max)
- **Short descriptions** (100 chars max)
- **Long descriptions** (350 chars max)
- **SEO keywords** (20 terms, comma-separated)
- **Formatted transcripts** (AP Style)
- **Timestamp chapter markers** (Media Manager + YouTube formats)
- **SRT subtitle files** (when timing info available)

### Time Savings

- **Manual process**: 45-60 minutes per video
- **Automated process**: 15-20 minutes per video
- **Hands-on time**: ~10-15 minutes (rest is automation)

### Prerequisites

- Python 3.8+
- Anthropic API key (set as `ANTHROPIC_API_KEY` environment variable)
- Required packages: `anthropic`, `watchdog`, `pyyaml`
- Claude Code desktop app (for `/start` command)

---

## Getting Started

### Initial Setup (One Time)

1. **Install dependencies:**
   ```bash
   pip install anthropic watchdog pyyaml
   ```

2. **Set API key:**
   ```bash
   export ANTHROPIC_API_KEY="your-api-key-here"
   # Add to ~/.bashrc or ~/.zshrc to persist
   ```

3. **Verify configuration:**
   ```bash
   python3 automation/test_automation.py
   ```
   Should see: `✓ All tests passed!`

### Starting a Session

Open Claude Code in this project directory and run:
```
/start
```

This activates the **Editor Agent** who:
- Starts the automation watcher
- Monitors all file events
- Guides you through each phase
- Provides real-time updates
- Troubleshoots issues

---

## Phase-by-Phase Walkthrough

### Phase 0: Session Initialization

**What happens:**
1. Editor Agent starts automation watcher
2. Surveys existing projects
3. Presents status dashboard
4. Asks what you want to work on

**Your actions:**
- Review current project list
- Decide: start new project or continue existing

**Expected output:**
```
✓ Editor Agent activated
✓ Automation watcher running

Current projects:
  2WLI1203HD - Phase 2 complete (waiting for SEMRush)
  9UNP2000HD - Phase 1 complete (needs draft)
  6HNS0425SM - All phases complete

What would you like to work on today?
```

**Next step:** Tell the agent your choice

---

### Phase 1: Initial Analysis (Automatic)

**Trigger:** Drop transcript file in `transcripts/` folder

**You do:**
```bash
cp ~/Desktop/2WLI1205HD_ForClaude.txt transcripts/
```
Or provide path to agent if using `/start`

**Automation does:**
1. Detects transcript (2-second debounce)
2. Extracts Media ID: `2WLI1205HD`
3. Creates project folder: `output/2WLI1205HD/`
4. Creates subfolders: `drafts/` and `semrush/`
5. Calls Claude API 3× in parallel:
   - Generates brainstorming options
   - Formats transcript (AP Style)
   - Creates timestamp report
6. Checks for timing info (HH:MM:SS:FF format)
7. If found, generates SRT subtitles
8. Records all events in `workflow.json`

**Agent shows:**
```
📥 Detected transcript: 2WLI1205HD_ForClaude.txt
⚙️ Phase 1 automation running...
   → Brainstorming options
   → Formatted transcript
   → Timestamp report
   → SRT subtitles (timing detected: yes)

Estimated time: 2-3 minutes
[Progress updates as outputs complete...]

✓ Phase 1 complete! (2m 18s)

Generated:
  📄 01_brainstorming.md
     - 3 title options
     - 2 short descriptions
     - 2 long descriptions
     - 20 SEO keywords
  📄 05_formatted_transcript.md
  📄 06_timestamp_report.md
  📄 07_subtitles.srt
```

**Your review:**
1. Open `output/2WLI1205HD/01_brainstorming.md`
2. Read the 3 title options - which do you prefer?
3. Read the 2 short descriptions - which pairs best with your chosen title?
4. Read the 2 long descriptions - which provides best context?
5. Review the 20 keywords - do they match the content?
6. Note any standout quotes

**Agent nudges you:**
```
Would you like me to:
1. Show a summary of brainstorming options
2. Explain any program-specific rules for this Media ID
3. Walk you through character limits

Once you've picked your options, draft the metadata in Media Manager.
When ready, I'll guide you through Phase 2 (copy revision).
```

**Decision point:** Have you picked your preferred options?
- ✅ Yes → Draft metadata in Media Manager → Proceed to Phase 2
- ❌ Not sure → Ask agent to explain options or suggest alternatives
- 🔄 Need changes → Ask agent to run `/brainstorm` again with different focus

**Next step:** Draft metadata in your CMS (Media Manager)

---

### Phase 2: Copy Revision (Automatic)

**Trigger:** Drop draft file in `output/[MEDIA_ID]/drafts/`

**You do:**
1. Draft metadata in Media Manager using brainstorming options
2. Take screenshot showing:
   - Title field
   - Short description field
   - Long description field
   - Keywords field (if visible)
3. Save screenshot:
   ```bash
   # Name it clearly with Media ID and date
   cp ~/Screenshots/metadata.png output/2WLI1205HD/drafts/2WLI1205HD_20250129_draft.png
   ```

**Alternative (text input):**
```bash
cat > output/2WLI1205HD/drafts/draft.txt << 'EOF'
Title: Wisconsin farmers innovate with sustainable agriculture practices
Short: Family farms adopt new climate-friendly techniques across rural Wisconsin communities.
Long: Wisconsin family farmers are leading the way in sustainable agriculture, adopting innovative techniques to reduce environmental impact while maintaining productivity. From regenerative soil practices to renewable energy integration, these rural communities are proving that farming and environmental stewardship go hand in hand.
Keywords: Wisconsin agriculture, sustainable farming, family farms, climate-friendly farming, regenerative agriculture, rural Wisconsin, environmental stewardship, farming innovation
EOF
```

**Automation does:**
1. Detects new file in `drafts/`
2. Reads transcript
3. Extracts metadata:
   - If screenshot: uses Claude vision
   - If text: parses directly
4. Generates `02_copy_revision.md` with:
   - Side-by-side comparison (Original | Revised)
   - Character counts for each field
   - Reasoning for each suggestion
   - AP Style compliance notes
   - Program-specific rules applied
   - Keyword adjustments
5. Updates `workflow.json`

**Agent shows:**
```
📥 Detected draft: 2WLI1205HD_20250129_draft.png
⚙️ Phase 2 automation running...
   → Extracting metadata from screenshot
   → Analyzing against transcript
   → Applying AP Style guidelines
   → Checking program-specific rules

Estimated time: 1-2 minutes

✓ Phase 2 complete! (1m 42s)

Key recommendations:
  ⚠️ Title is 82 chars (2 over limit)
  ✓ Short description fits (98 chars)
  ✓ Long description fits (347 chars)
  💡 Suggested 3 keyword additions based on transcript
```

**Your review:**
1. Open `output/2WLI1205HD/02_copy_revision.md`
2. Compare Original vs. Revised columns
3. Read reasoning for each change
4. Check character counts
5. Review keyword recommendations

**Agent nudges you:**
```
Copy revision analysis:

📊 Character Count Status:
   Title: 82/80 chars ⚠️ OVER LIMIT
   Short: 98/100 chars ✓
   Long: 347/350 chars ✓

🔍 Main Issues Found:
   1. Title exceeds limit by 2 characters
   2. Recommended removal of "the" for brevity
   3. Suggested front-loading key terms for SEO

💡 Top Recommendations:
   1. Title: "Wisconsin farmers innovate with sustainable agriculture"
      (eliminates "practices", saves 10 chars → 72/80)
   2. Add keywords: "soil health", "crop rotation", "conservation"
   3. Long description: move location earlier in sentence

Want to:
1. Iterate (update draft, drop new screenshot)
2. Review detailed reasoning
3. Move to Phase 3 (keyword research)
4. Skip Phase 3, finalize for publication
```

**Decision point:** Are you satisfied with the revisions?
- ✅ Yes, no keyword research → Proceed to Phase 4 (finalization)
- ✅ Yes, want keyword research → Proceed to Phase 3
- 🔄 Need iteration → Update draft in Media Manager, take new screenshot, drop in `drafts/`
- ❌ Confused → Ask agent to explain specific recommendations

**Next step:** Choose your path:
- **Path A**: Implement revisions, skip SEO research → Phase 4
- **Path B**: Implement revisions, conduct SEO research → Phase 3

---

### Phase 3: SEO Research (Optional, Automatic)

**Trigger:** Drop SEMRush screenshot in `output/[MEDIA_ID]/semrush/`

**When to use Phase 3:**
- High-profile content (pledge drives, special programming)
- Competitive topics (many similar videos online)
- Strategic keyword opportunities identified
- Content you want to maximize discoverability for

**When to skip Phase 3:**
- Routine episode metadata
- Time-sensitive content (quick turnaround needed)
- Niche topics with limited search competition
- Content with inherent audience (series with loyal following)

**You do:**
1. Log into SEMRush
2. Run keyword analysis for your topic/content
3. Take screenshot showing:
   - Keywords with search volumes
   - Keyword difficulty scores
   - Related keyword suggestions
   - Trend graphs (if visible)
4. Save screenshot:
   ```bash
   cp ~/Screenshots/semrush.png output/2WLI1205HD/semrush/2WLI1205HD_20250129_semrush.png
   ```

**Automation does:**
1. Detects SEMRush screenshot
2. Reads transcript + previous outputs
3. **First AI call** - Generates `03_keyword_report.md`:
   - Extracts data from screenshot (vision)
   - Conducts web research for trends
   - Ranks keywords by search volume
   - Identifies competitive gaps
   - Provides platform-ready keyword list
4. **Second AI call** - Generates `04_implementation.md`:
   - Reads keyword report as input
   - Reviews brainstorming + revision docs
   - Creates specific copy optimization suggestions
   - Prioritizes actions (Immediate/Short-term/Long-term)
   - Provides platform-specific tactics
   - Includes success metrics
5. Updates `workflow.json`

**Agent shows:**
```
📥 Detected SEMRush: 2WLI1205HD_20250129_semrush.png
⚙️ Phase 3 automation running (2 reports)...
   → Analyzing keyword data (vision extraction)
   → Researching trending keywords
   → Generating keyword report
   → Creating implementation plan

Estimated time: 3-5 minutes

✓ Keyword report complete! (2m 34s)
   - 20 keywords ranked by search volume
   - 5 high-opportunity terms identified
   - Platform-ready list generated

✓ Implementation plan complete! (1m 58s)
   - 3 immediate action items
   - 4 short-term optimizations
   - 2 long-term strategies

✓ Phase 3 complete! (4m 32s total)
```

**Your review:**
1. Open `output/2WLI1205HD/03_keyword_report.md`
   - Check platform-ready keyword list (copy-paste ready)
   - Review high/medium/low volume keywords
   - Note competitive gaps identified
   - Read distinctive keywords (lower volume, high relevance)

2. Open `output/2WLI1205HD/04_implementation.md`
   - Read executive summary (what's the big picture?)
   - Review copy revision recommendations (specific changes)
   - Check immediate actions (do these first)
   - Review platform-specific tactics (YouTube, website, social)
   - Note success metrics (how to measure impact)

**Agent nudges you:**
```
📊 Keyword Research Results:

Top 5 High-Opportunity Keywords:
  1. "sustainable farming Wisconsin" - 1,200/mo, Easy difficulty
  2. "regenerative agriculture" - 2,400/mo, Moderate difficulty
  3. "family farm sustainability" - 480/mo, Easy difficulty
  4. "climate-friendly farming" - 890/mo, Easy difficulty
  5. "soil health practices" - 1,650/mo, Moderate difficulty

💡 Immediate Actions (Priority 1):
  1. Update keyword list to platform-ready version (copy from report)
  2. Revise title to include "sustainable farming" earlier
  3. Add "regenerative agriculture" to long description naturally

📋 Implementation Plan:
  - Immediate (0-24h): 3 actions
  - Short-term (1-7 days): 4 actions
  - Long-term (1-4 weeks): 2 strategies

Want to:
1. Copy platform-ready keyword list (I'll show it)
2. Review specific copy changes recommended
3. See platform-specific tactics (YouTube vs website)
4. Move to Phase 4 (finalization)
```

**Decision point:** How will you use this research?
- ✅ Implement all immediate actions → Update metadata now
- 🎯 Cherry-pick recommendations → Choose specific changes
- 📋 Save for later → Note in documentation, implement gradually
- 🔄 Need more research → Ask agent for deeper analysis on specific keywords

**Next step:** Implement priority recommendations → Proceed to Phase 4

---

### Phase 4: Finalization

**Trigger:** User indicates they're ready to publish

**Agent shows:**
```
✓ Pre-Publication Checklist for 2WLI1205HD

📋 Metadata Status:
   ✓ Title: 72/80 chars (within limit)
   ✓ Short description: 98/100 chars (within limit)
   ✓ Long description: 347/350 chars (within limit)
   ✓ Keywords: 20 terms, platform-ready format
   ✓ No prohibited language detected
   ✓ AP Style compliance verified

📄 Content Outputs:
   ✓ Brainstorming options reviewed
   ✓ Copy revision implemented
   ✓ Keyword research complete
   ✓ Formatted transcript available
   ✓ Timestamps verified (15:42 runtime)
   ✓ SRT subtitles generated

🎯 Program-Specific Checks:
   [Shows any special rules for this Media ID prefix]

Optional Actions:
   [ ] Run /format-transcript for publication version
   [ ] Run /create-timestamps to adjust chapter breaks
   [ ] Review SRT subtitles for accuracy

Ready to publish? Any final questions?
```

**Your review:**
Final quality checks before publication:

1. **Metadata accuracy:**
   - Title accurately describes content?
   - Description factually correct?
   - Keywords match what's actually in video?

2. **Style compliance:**
   - Down-style headlines (only first word + proper nouns capitalized)?
   - No promotional language ("don't miss", "amazing", etc.)?
   - No viewer directives ("watch as", "see how", "learn")?
   - Numbers formatted correctly (one-nine spelled out, 10+ numerals)?

3. **Technical specs:**
   - Character limits respected?
   - Keywords comma-separated, no hash symbols?
   - Timestamps match video length?

4. **Program-specific:**
   - University Place: No honorifics (Dr., Professor)?
   - Here and Now: Name format correct ([org] [title] [name])?
   - The Look Back: Host names and historians mentioned?

**Agent nudges you:**
```
Final review for 2WLI1205HD:

✓ All automated outputs generated
✓ All quality checks passed
✓ Program-specific rules applied

📁 Complete project folder:
   01_brainstorming.md
   02_copy_revision.md
   03_keyword_report.md
   04_implementation.md
   05_formatted_transcript.md
   06_timestamp_report.md
   07_subtitles.srt
   workflow.json

📤 Ready to publish:
   - Metadata finalized in Media Manager
   - Timestamp chapter markers ready
   - SRT subtitles available for upload
   - Formatted transcript for accessibility

Need me to:
1. Generate any missing outputs
2. Run final quality check on specific field
3. Archive this project
4. Start a new project
```

**Decision point:** Publish now or polish further?
- ✅ Publish now → Complete! Move transcript to archive
- 🔍 Final polish → Ask agent for specific checks
- 📝 Need formatted transcript → Run `/format-transcript`
- ⏱️ Adjust timestamps → Run `/create-timestamps`

**Next step:** Publish → Archive → Start next project

---

### Phase 5: Archival (Manual or Automatic)

**Manual archival (after publishing):**
```bash
# Move transcript to archive
mv transcripts/2WLI1205HD_ForClaude.txt transcripts/archive/

# Project stays in output/2WLI1205HD/ as permanent record
```

**Automatic archival (90 days, cron job):**
```bash
# Set up daily archival job
crontab -e

# Add this line (runs daily at 2am):
0 2 * * * cd /path/to/editorial-assistant && python3 automation/archive.py --config automation/config.yaml
```

**What archival does:**
- Moves projects >90 days old to `output/archive/`
- Moves matching transcripts to `transcripts/archive/`
- Preserves `workflow.json` for audit trail
- Keeps active projects clean and manageable

**Agent tracks:**
```
📦 Archival Status:

Active projects: 3
  2WLI1205HD (1 day old)
  9UNP2000HD (5 days old)
  6HNS0425SM (18 days old)

Archived projects: 47
  Last archival run: 2 days ago
  Next scheduled run: tonight at 2am

Projects approaching archival (85-90 days):
  2WLI1103HD (87 days old)
  9UNP1985HD (89 days old)

Want me to:
1. Preview what would be archived now
2. Manually archive specific project
3. Check archived project status
```

---

## Decision Points

### Decision Tree: Phase 1 → Publication

```
Phase 1 Complete
     ↓
Review brainstorming
     ↓
     ├─→ Satisfied with options?
     │        ↓ Yes
     │   Draft in Media Manager
     │        ↓
     │   Screenshot draft
     │        ↓
     │   Phase 2 (auto)
     │        ↓
     │   Review revision
     │        ↓
     │        ├─→ Needs iteration?
     │        │        ↓ Yes
     │        │   [Loop back to "Draft in Media Manager"]
     │        │
     │        └─→ Satisfied?
     │                 ↓ Yes
     │            Need SEO research?
     │                 ↓
     │                 ├─→ Yes
     │                 │    ↓
     │                 │   Run SEMRush
     │                 │    ↓
     │                 │   Screenshot data
     │                 │    ↓
     │                 │   Phase 3 (auto)
     │                 │    ↓
     │                 │   Review recommendations
     │                 │    ↓
     │                 │   Implement priorities
     │                 │    ↓
     │                 └─→ [Join at Finalization]
     │
     └─→ No → Ask agent for alternatives
          [Rerun /brainstorm or get explanation]

Finalization
     ↓
Final quality check
     ↓
Publish
     ↓
Archive
```

### When to Iterate vs. Proceed

**Iterate (run phase again) if:**
- Character limits exceeded significantly (>5 chars)
- Major factual inaccuracies
- Missing key information from transcript
- Doesn't match program-specific rules
- User significantly revised their approach

**Proceed to next phase if:**
- Minor wording tweaks only
- Character limits close but not over
- Core messaging is accurate
- Stylistic preferences (not errors)
- Time-sensitive publication deadline

---

## Quality Standards

### Non-Negotiable Rules

**Character Limits (HARD STOPS):**
- Title: ≤80 characters (spaces count)
- Short description: ≤100 characters (spaces count)
- Long description: ≤350 characters (spaces count)

**AP Style Compliance:**
- Down-style headlines (only first word + proper nouns capitalized)
- Numbers: spell out one through nine, numerals for 10+
- Abbreviations: spell out on first reference
- Time: numerals with lowercase a.m. or p.m.

**Prohibited Language:**
- ❌ Viewer directives: "watch as", "see how", "discover", "learn"
- ❌ Promises: "will show", "will teach", "will reveal"
- ❌ Sales language: "free", "amazing", "incredible"
- ❌ Emotional predictions: "you'll love", "you'll be amazed"
- ❌ Calls to action: "join us", "don't miss", "tune in"

**Required Approach:**
- ✅ State what content IS
- ✅ Describe what happens (factually)
- ✅ Use specific details over adjectives
- ✅ Verify all facts against transcript

### Program-Specific Rules

**University Place:**
- Include lecture series name as keyword if part of series
- No honorific titles (Dr., Professor)
- Avoid inflammatory/bombastic language
- Focus on academic subject matter

**Here and Now:**
- Title format: `[SUBJECT] on [brief neutral description]`
- Long description: `[Org] [title] [name] [verb] [subject]`
- Use "discuss" for ALL elected officials
- Use "explain," "describe," "consider" for non-elected
- Include party/location for elected officials (R-Rochester, D-Madison)

**The Look Back:**
- MUST include: host names (Nick and Taylor)
- MUST include: institutions/locations visited
- MUST include: expert historians consulted
- Focus on WHY it matters > WHAT happened
- Use precise historical language (deliberate decisions, not accidents)

---

## Troubleshooting

### Watcher Not Starting

**Symptoms:**
```
✗ Failed to start watcher
Error: [error message]
```

**Fixes:**
1. **Check Python version:**
   ```bash
   python3 --version  # Should be 3.8+
   ```

2. **Install dependencies:**
   ```bash
   pip install anthropic watchdog pyyaml
   ```

3. **Verify API key:**
   ```bash
   echo $ANTHROPIC_API_KEY  # Should show your key
   ```

4. **Check config file:**
   ```bash
   cat automation/config.yaml  # Verify paths exist
   ```

5. **Run test suite:**
   ```bash
   python3 automation/test_automation.py
   ```

---

### Watcher Crashes During Processing

**Symptoms:**
```
⚠️ Watcher stopped unexpectedly
Shell exited with code 1
```

**Fixes:**
1. **Restart with verbose logging:**
   ```bash
   ./venv/bin/python -m automation.watcher --config automation/config.yaml --verbose
   ```

   Or if venv is activated:
   ```bash
   python -m automation.watcher --config automation/config.yaml --verbose
   ```

2. **Check for Python errors in logs:**
   - Look for "Traceback" in output
   - Note the error message and line number

3. **Common issues:**
   - **Out of memory**: Large transcript files (>100KB) may need more RAM
   - **API rate limit**: Wait 1 minute, retry
   - **Malformed transcript**: Check file encoding (should be UTF-8)

4. **Manual processing fallback:**
   If watcher keeps crashing, use manual commands:
   ```
   /brainstorm transcripts/[MEDIA_ID]_ForClaude.txt
   /revise [MEDIA_ID]
   /research-keywords [MEDIA_ID]
   ```

---

### Phase Not Completing

**Symptoms:**
```
⚙️ Phase 2 running... [stuck for >5 minutes]
```

**Diagnosis:**
1. **Check watcher output:**
   - Agent should show recent log messages
   - Look for "API error" or "timeout"

2. **Verify file detected:**
   ```bash
   ls -lh output/[MEDIA_ID]/drafts/  # File should be there
   ```

3. **Check workflow.json:**
   ```bash
   cat output/[MEDIA_ID]/workflow.json
   ```
   - Look for error messages in events

**Fixes:**
1. **API timeout**: Wait and retry
   - Sometimes Claude API is slow
   - 5-10 minutes is unusual but possible

2. **File detection issue**: Rename file
   ```bash
   mv output/[MEDIA_ID]/drafts/old.png output/[MEDIA_ID]/drafts/[MEDIA_ID]_new.png
   ```

3. **Corrupted screenshot**: Retake
   - Ensure screenshot is PNG or JPEG
   - File should be >10KB, <10MB
   - Image should be clear and readable

4. **Manual workaround**:
   ```
   /revise [MEDIA_ID]
   # Provide file path or paste text when prompted
   ```

---

### Output Quality Issues

**Symptoms:**
- Character counts wrong
- Factual inaccuracies
- Doesn't match program rules
- Keywords don't match content

**Fixes:**

1. **Character count errors:**
   - Manually verify: `echo "text" | wc -c`
   - If AI undercounted: iterate with explicit instruction
   - Agent can rerun with focus on limits

2. **Factual inaccuracies:**
   - Check transcript accuracy (was it OCR'd correctly?)
   - Verify names/dates in transcript are correct
   - Iterate with corrections noted

3. **Program rules not applied:**
   - Check `knowledge/Media ID Prefixes.md` for Media ID
   - Verify Media ID extracted correctly
   - Ask agent to explicitly apply rules

4. **Poor keyword selection:**
   - Check if keywords match transcript terms
   - Verify logical/implied keywords make sense
   - Phase 3 (SEO research) can improve this

---

### SRT Subtitles Not Generated

**Symptoms:**
- Phase 1 complete but no `07_subtitles.srt`

**Diagnosis:**
Transcript doesn't have timing information.

**Timing format required:**
```
00:00:00:00 - 00:00:10:15
Speaker Name
Dialogue text here.
```

**Check your transcript:**
```bash
head -5 transcripts/[MEDIA_ID]_ForClaude.txt
```

Should see timecodes in format `HH:MM:SS:FF - HH:MM:SS:FF`

**If timing present but SRT not generated:**
1. Check logs for "SRT generation failed"
2. Verify timing format is consistent throughout
3. Try manual generation:
   ```bash
   python3 automation/srt_generator.py transcripts/[MEDIA_ID]_ForClaude.txt output/[MEDIA_ID]/07_subtitles.srt
   ```

---

## Advanced Features

### Custom Prompt Tweaking

All prompts live in `system_prompts/`:
```
system_prompts/
├── phase1_brainstorming.md       # Initial analysis
├── phase2_editing.md              # Copy revision
├── phase3_analysis.md             # Keyword research
├── phase3_implementation.md       # Action planning
├── phase4_transcript.md           # Transcript formatting
└── phase4_timestamps.md           # Chapter markers
```

**To customize behavior:**
1. Edit the relevant `.md` file
2. Test with a sample Media ID
3. Compare output before/after
4. Commit changes if improved

**Common customizations:**
- Adjust keyword count (default: 20)
- Change title/description option counts
- Add industry-specific terminology
- Modify quality standards
- Adjust character count tolerances

---

### Batch Processing

**Process multiple videos at once:**

```bash
# Drop all transcripts
cp ~/transcripts/*.txt transcripts/

# Watcher processes them sequentially
# Check status:
ls -lh output/*/workflow.json
```

**Agent will:**
- Detect each file
- Process in order received
- Show progress for all projects
- Notify when each completes

**Monitor batch:**
```
/start

# Agent shows:
⚙️ 3 projects in queue:
   2WLI1205HD - Phase 1 running (1m 30s)
   2WLI1206HD - Waiting to start
   9UNP2001HD - Waiting to start
```

---

### Integration with Media Manager

**Workflow:**
1. Generate metadata in editorial assistant
2. Copy platform-ready keyword list
3. Paste into Media Manager
4. Copy/adapt descriptions
5. Upload SRT subtitles
6. Add timestamp chapters
7. Publish

**Platform-ready outputs:**
- Keywords: comma-separated, ready to paste
- YouTube timestamps: copy entire section
- Media Manager timestamps: import table format
- SRT: upload directly to video player

---

### Archival Recovery

**Find archived project:**
```bash
ls -lh output/archive/ | grep [MEDIA_ID]
```

**Restore archived project:**
```bash
mv output/archive/[MEDIA_ID]_20250115 output/[MEDIA_ID]
mv transcripts/archive/[MEDIA_ID]_ForClaude.txt transcripts/
```

**View archived workflow:**
```bash
cat output/archive/[MEDIA_ID]_20250115/workflow.json | jq .
```

---

## Success Metrics

Track your editorial efficiency:

### Time Savings
- **Before automation**: 45-60 min per video
- **After automation**: 15-20 min per video
- **Hands-on time**: 10-15 min
- **Automation time**: 5-10 min (parallel)

### Quality Improvements
- **Character limit violations**: Near zero (agent catches before submission)
- **AP Style errors**: Dramatically reduced
- **Keyword relevance**: Higher (AI extracts from transcript)
- **SEO optimization**: Optional but effective (Phase 3)

### Workflow Benefits
- **Fewer revision cycles**: Better first drafts
- **Consistent quality**: Standards enforced automatically
- **Knowledge retention**: workflow.json audit trail
- **Easier onboarding**: New staff follow guided workflow

---

## Summary: The Complete Workflow

1. **/start** - Activate editor agent, start watcher
2. **Drop transcript** - Phase 1 runs automatically (2-3 min)
3. **Review options** - Pick titles/descriptions, note keywords
4. **Draft in CMS** - Write metadata based on brainstorming
5. **Screenshot draft** - Capture for Phase 2
6. **Drop screenshot** - Phase 2 runs automatically (1-2 min)
7. **Review revisions** - Check recommendations, iterate if needed
8. **Optional: SEMRush** - Drop screenshot for Phase 3 (3-5 min)
9. **Implement changes** - Apply recommendations in CMS
10. **Final review** - Quality checklist, verify all outputs
11. **Publish** - Upload to Media Manager with all assets
12. **Archive** - Move transcript, let automation handle rest

**Total time: 15-20 minutes per video**
**Outputs: 6-7 publication-ready files**
**Quality: Consistent, standards-compliant, optimized**

---

## Questions?

Ask the **Editor Agent** anytime during your session:
- "Explain this recommendation"
- "Why did Phase 3 suggest this keyword?"
- "How do I fix character count issues?"
- "What are the rules for University Place?"
- "Can I skip Phase 3?"
- "How do I iterate on this?"

The agent is your collaborative partner throughout the workflow!
