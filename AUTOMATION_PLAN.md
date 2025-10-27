# Claude Code Automation Plan

## Overview

This document outlines the changes needed to transform the chat-based editorial assistant workflow into an automated file-watching system suitable for Claude Code.

## Current vs. Proposed Workflow

### Current (Chat-Based)
1. User pastes transcript into chat
2. Claude generates brainstorming document in chat artifact
3. User provides draft copy via chat
4. Claude generates revision document in chat artifact
5. User provides SEMRush data via chat
6. Claude generates keyword/implementation reports in chat artifacts

### Proposed (Automated File-Based)
1. **Trigger**: New transcript appears in `transcripts/` folder
2. **Phase 1**: Auto-generate brainstorming report → `output/[MEDIA_ID]/01_brainstorming.md`
3. **Trigger**: Screenshot appears in `draft_copy/[MEDIA_ID]_draft.png`
4. **Phase 2**: Auto-generate revision report → `output/[MEDIA_ID]/02_copy_revision.md`
5. **Trigger**: Screenshot appears in `semrush/[MEDIA_ID]_semrush.png`
6. **Phase 3**: Auto-generate keyword/implementation reports → `output/[MEDIA_ID]/03_keyword_report.md` + `04_implementation.md`
7. **Trigger**: User creates flag file `output/[MEDIA_ID]/request_transcript.flag`
8. **Optional**: Auto-generate formatted transcript → `output/[MEDIA_ID]/05_formatted_transcript.md`
9. **Trigger**: User creates flag file `output/[MEDIA_ID]/request_timestamps.flag`
10. **Optional**: Auto-generate timestamp report → `output/[MEDIA_ID]/06_timestamp_report.md`

## Proposed Folder Structure

```
claude-editorial-assistant/
├── transcripts/              # Input: Raw transcripts
│   ├── 2WLI1203HD_ForClaude.txt
│   └── archive/             # Move here when complete
│
├── draft_copy/              # Input: Screenshots of draft metadata
│   ├── 2WLI1203HD_draft.png
│   └── archive/
│
├── semrush/                 # Input: Screenshots of SEMRush analysis
│   ├── 2WLI1203HD_semrush.png
│   └── archive/
│
├── output/                  # Generated reports organized by Media ID
│   └── 2WLI1203HD/
│       ├── 01_brainstorming.md
│       ├── 02_copy_revision.md
│       ├── 03_keyword_report.md
│       ├── 04_implementation.md
│       ├── 05_formatted_transcript.md      # Only if requested
│       ├── 06_timestamp_report.md          # Only if requested
│       ├── request_transcript.flag         # User creates to request
│       ├── request_timestamps.flag         # User creates to request
│       └── .complete                       # Marks workflow complete
│
├── knowledge/               # Reference materials (unchanged)
│   ├── ap_styleguide.pdf
│   ├── Transcript Style Guide.pdf
│   ├── WPM Generative AI Guidelines.pdf
│   └── Media ID Prefixes.md
│
├── automation/              # New: Automation scripts
│   ├── watcher.py           # File watcher implementation
│   ├── processor.py         # Claude API integration
│   └── config.yaml          # Configuration
│
├── system_prompts/          # Reorganized: Version-controlled prompts
│   ├── current.md           # Active system prompt
│   ├── phase1_brainstorming.md
│   ├── phase2_editing.md
│   ├── phase3_analysis.md
│   ├── phase4_transcript.md
│   ├── phase4_timestamps.md
│   └── archive/             # Old versions
│
├── CLAUDE.md
└── AUTOMATION_PLAN.md       # This file
```

## Implementation Strategy

### 1. Modular System Prompts

Split the monolithic system prompt into phase-specific prompts:

**`system_prompts/phase1_brainstorming.md`**
- Input: Transcript file path
- Output: Markdown file with brainstorming document
- No chat interaction needed

**`system_prompts/phase2_editing.md`**
- Input: Transcript file path + draft copy screenshot path
- Output: Markdown file with copy revision document
- Use Claude's vision capabilities to read screenshot

**`system_prompts/phase3_analysis.md`**
- Input: Transcript file path + SEMRush screenshot path
- Output: Two markdown files (keyword report + implementation report)
- Use vision for screenshot + web search for trending keywords

**`system_prompts/phase4_transcript.md`**
- Input: Transcript file path
- Output: Formatted transcript markdown

**`system_prompts/phase4_timestamps.md`**
- Input: Transcript file path
- Output: Timestamp report with both formats

### 2. File Watcher Implementation

**Option A: Python Script (Recommended)**
```python
# automation/watcher.py
import time
import os
from watchdog import FileSystemEventHandler, Observer
from processor import process_new_file

class TranscriptWatcher(FileSystemEventHandler):
    def on_created(self, event):
        if event.is_directory:
            return

        file_path = event.src_path

        # Trigger Phase 1 on new transcript
        if file_path.endswith('_ForClaude.txt') and 'transcripts/' in file_path:
            media_id = extract_media_id(file_path)
            process_phase1(media_id, file_path)

        # Trigger Phase 2 on draft copy screenshot
        elif '_draft.png' in file_path and 'draft_copy/' in file_path:
            media_id = extract_media_id(file_path)
            process_phase2(media_id, file_path)

        # Trigger Phase 3 on SEMRush screenshot
        elif '_semrush.png' in file_path and 'semrush/' in file_path:
            media_id = extract_media_id(file_path)
            process_phase3(media_id, file_path)

        # Trigger optional outputs on flag files
        elif 'request_transcript.flag' in file_path:
            media_id = os.path.basename(os.path.dirname(file_path))
            process_transcript(media_id)

        elif 'request_timestamps.flag' in file_path:
            media_id = os.path.basename(os.path.dirname(file_path))
            process_timestamps(media_id)
```

**Option B: MCP Server (Future Enhancement)**
- Create custom MCP server for PBS Wisconsin editorial workflow
- Integrates directly with Claude Code
- Provides tools like `analyze_transcript`, `review_draft`, `research_keywords`

**Option C: GitHub Actions / CI/CD**
- Trigger on git commit to specific folders
- Run Claude API processing in cloud
- Commit results back to repo
- Good for team workflows

### 3. Claude API Integration

**`automation/processor.py`**
```python
import anthropic
import os

def process_phase1(media_id, transcript_path):
    """Generate brainstorming document from transcript"""

    # Read transcript
    with open(transcript_path, 'r') as f:
        transcript = f.read()

    # Read phase 1 system prompt
    with open('system_prompts/phase1_brainstorming.md', 'r') as f:
        system_prompt = f.read()

    # Call Claude API
    client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))
    message = client.messages.create(
        model="claude-sonnet-4-5-20250929",
        max_tokens=4000,
        system=system_prompt,
        messages=[{
            "role": "user",
            "content": f"Process this transcript:\n\n{transcript}"
        }]
    )

    # Save output
    output_dir = f"output/{media_id}"
    os.makedirs(output_dir, exist_ok=True)

    with open(f"{output_dir}/01_brainstorming.md", 'w') as f:
        f.write(message.content[0].text)

    print(f"✓ Generated brainstorming document for {media_id}")

def process_phase2(media_id, draft_screenshot_path):
    """Generate copy revision from draft screenshot"""

    # Read transcript
    transcript_path = find_transcript(media_id)
    with open(transcript_path, 'r') as f:
        transcript = f.read()

    # Read screenshot as base64
    import base64
    with open(draft_screenshot_path, 'rb') as f:
        image_data = base64.standard_b64encode(f.read()).decode("utf-8")

    # Read phase 2 system prompt
    with open('system_prompts/phase2_editing.md', 'r') as f:
        system_prompt = f.read()

    # Call Claude API with vision
    client = anthropic.Anthropic(api_key=os.environ.get("ANTHROPIC_API_KEY"))
    message = client.messages.create(
        model="claude-sonnet-4-5-20250929",
        max_tokens=4000,
        system=system_prompt,
        messages=[{
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": f"Transcript:\n\n{transcript}\n\nPlease review the draft copy shown in the screenshot:"
                },
                {
                    "type": "image",
                    "source": {
                        "type": "base64",
                        "media_type": "image/png",
                        "data": image_data,
                    },
                }
            ]
        }]
    )

    # Save output
    output_dir = f"output/{media_id}"
    with open(f"{output_dir}/02_copy_revision.md", 'w') as f:
        f.write(message.content[0].text)

    print(f"✓ Generated copy revision for {media_id}")

# Similar functions for process_phase3, process_transcript, process_timestamps...
```

### 4. Configuration File

**`automation/config.yaml`**
```yaml
# Claude API Configuration
model: claude-sonnet-4-5-20250929
max_tokens: 4000
temperature: 1.0

# Folder paths
folders:
  transcripts: ./transcripts
  draft_copy: ./draft_copy
  semrush: ./semrush
  output: ./output
  system_prompts: ./system_prompts

# File naming patterns
patterns:
  transcript: "*_ForClaude.txt"
  draft_screenshot: "*_draft.png"
  semrush_screenshot: "*_semrush.png"

# Optional features (disabled by default, enabled by flag files)
optional_outputs:
  formatted_transcript: false  # Create request_transcript.flag to enable
  timestamps: false             # Create request_timestamps.flag to enable

# Automation behavior
automation:
  watch_enabled: true
  auto_archive: true           # Move inputs to archive/ after processing
  notification_email: null     # Optional: email when processing completes
```

## Changes to System Prompt

### Key Modifications Needed

1. **Remove Chat Interaction Language**
   - ❌ "I'll help you with...", "Let me know if...", "What would you like to..."
   - ✅ Direct, declarative output focused on deliverables

2. **File-Based Input/Output**
   - Read transcript from file path provided
   - Extract media ID from filename
   - Output pure markdown (no chat artifacts)
   - Reference files by path when needed

3. **Vision Integration for Screenshots**
   ```
   When processing draft copy:
   1. Analyze the provided screenshot of draft metadata
   2. Extract the following fields from the image:
      - Title (and character count if shown)
      - Short description (and character count if shown)
      - Long description (and character count if shown)
      - Keywords (if shown)
   3. Generate copy revision document comparing screenshot content to transcript
   ```

4. **Structured Output Format**
   ```markdown
   # Brainstorming Document
   **Media ID**: 2WLI1203HD
   **Generated**: 2025-10-22 14:30:00
   **Transcript**: transcripts/2WLI1203HD_ForClaude.txt

   [Rest of brainstorming content...]
   ```

5. **Remove Ethical Disclaimer from Automated Reports**
   - Keep in README or project documentation
   - Don't include in every generated file (user already knows)
   - Or include once in a header comment that can be easily removed

6. **Deterministic Processing**
   - No "Option 1, Option 2, Option 3" requiring user choice
   - Generate ranked recommendations instead
   - User can edit the markdown file directly if they prefer alternatives

## User Workflow

### Standard Video Processing

1. **Add transcript**
   ```bash
   # Copy new transcript to transcripts folder
   cp ~/Downloads/2WLI1203HD_ForClaude.txt transcripts/
   ```
   → Automation generates `output/2WLI1203HD/01_brainstorming.md`

2. **Review brainstorming, draft metadata in CMS**
   - Open `output/2WLI1203HD/01_brainstorming.md`
   - Use suggestions to create draft in Media Manager
   - Take screenshot of draft metadata

3. **Add draft screenshot**
   ```bash
   cp ~/Screenshots/draft.png draft_copy/2WLI1203HD_draft.png
   ```
   → Automation generates `output/2WLI1203HD/02_copy_revision.md`

4. **Review copy revision, optionally research keywords**
   - If satisfied: implement revisions and done
   - If SEO research needed: go to SEMRush, take screenshot

5. **Add SEMRush screenshot** (optional)
   ```bash
   cp ~/Screenshots/semrush.png semrush/2WLI1203HD_semrush.png
   ```
   → Automation generates:
   - `output/2WLI1203HD/03_keyword_report.md`
   - `output/2WLI1203HD/04_implementation.md`

6. **Request optional outputs** (if needed)
   ```bash
   # For formatted transcript
   touch output/2WLI1203HD/request_transcript.flag

   # For timestamps (15+ minute videos)
   touch output/2WLI1203HD/request_timestamps.flag
   ```
   → Automation generates requested files

7. **Archive when complete**
   ```bash
   # Auto-archive moves files automatically, or manually:
   mv transcripts/2WLI1203HD_ForClaude.txt transcripts/archive/
   mv draft_copy/2WLI1203HD_draft.png draft_copy/archive/
   mv semrush/2WLI1203HD_semrush.png semrush/archive/

   # Mark complete
   touch output/2WLI1203HD/.complete
   ```

### Batch Processing (Multiple Videos)

1. **Drop multiple transcripts at once**
   ```bash
   cp ~/batch/*.txt transcripts/
   ```
   → Automation processes each in parallel, creates separate output folders

2. **Review all brainstorming documents**
   ```bash
   # List all pending reviews
   ls -1 output/*/01_brainstorming.md

   # Open in editor
   code output/2WLI*/01_brainstorming.md
   ```

3. **Batch screenshot workflow**
   - Draft all metadata in CMS
   - Screenshot each (consistent naming)
   - Drop all screenshots at once
   → Automation processes all revisions

## Claude Code Integration

### Using Claude Code as the Processor

Instead of Python scripts + Claude API, you could use Claude Code itself:

**`.claude/commands/process-transcript.md`**
```markdown
When a new file appears in transcripts/ folder:

1. Extract Media ID from filename (e.g., "2WLI1203HD" from "2WLI1203HD_ForClaude.txt")
2. Create output directory: `output/[MEDIA_ID]/`
3. Read the transcript file
4. Read `system_prompts/phase1_brainstorming.md` for instructions
5. Generate brainstorming document following those instructions
6. Write output to `output/[MEDIA_ID]/01_brainstorming.md`
7. Include metadata header with Media ID, timestamp, source file
```

**User invokes manually:**
```bash
# In Claude Code
/process-transcript transcripts/2WLI1203HD_ForClaude.txt
```

**Or use MCP server for watching:**
- MCP server watches folders
- Notifies Claude Code when new files appear
- Claude Code processes automatically
- Results written to output/

### Advantages of Claude Code Approach
- No separate Python environment needed
- Built-in vision capabilities for screenshots
- Web search already integrated for keyword research
- Easy to iterate on prompts (just edit markdown files)
- Git integration for version control

### Disadvantages
- Less "set it and forget it" than standalone daemon
- Requires Claude Code to be running
- May need manual triggering vs. automatic

## Migration Path

### Phase 1: Restructure Repository (Week 1)
- [x] Create new folder structure
- [ ] Split system prompt into modular phase prompts
- [ ] Move old versions to system_prompts/archive/
- [ ] Create sample output/ structure

### Phase 2: Manual File-Based Workflow (Week 2)
- [ ] Test Phase 1 prompt with manual file processing
- [ ] Refine prompts to work with file paths instead of chat
- [ ] Validate all deliverable formats still work correctly
- [ ] Create slash commands for Claude Code

### Phase 3: Semi-Automated (Week 3)
- [ ] Implement basic Python watcher script
- [ ] Add Claude API integration for Phase 1 only
- [ ] Test with 5-10 real transcripts
- [ ] Gather feedback on output quality

### Phase 4: Full Automation (Week 4)
- [ ] Add all phases to automation
- [ ] Implement screenshot processing with vision
- [ ] Add configuration file support
- [ ] Implement auto-archiving
- [ ] Create monitoring/logging

### Phase 5: Polish & Documentation (Week 5)
- [ ] Write comprehensive README for new workflow
- [ ] Create troubleshooting guide
- [ ] Add error handling and validation
- [ ] Performance optimization
- [ ] Training documentation for team

## Considerations & Challenges

### 1. Screenshot Parsing Reliability
- **Challenge**: OCR/vision may misread text or character counts
- **Solution**: Include confidence scoring, request user validation for first few
- **Mitigation**: Standardize screenshot format (always capture same CMS fields)

### 2. Media ID Extraction
- **Challenge**: Filenames must follow exact convention
- **Solution**: Validate filename format before processing
- **Mitigation**: Create upload helper script that enforces naming

### 3. Concurrent Processing
- **Challenge**: Multiple files added simultaneously
- **Solution**: Queue-based processing with locks
- **Mitigation**: Rate limiting for Claude API calls

### 4. Error Recovery
- **Challenge**: What if Phase 2 fails but Phase 1 succeeded?
- **Solution**: Track processing state in .state.json file per media ID
- **Mitigation**: Allow manual retry of individual phases

### 5. Version Control
- **Challenge**: Generated markdown files create noise in git
- **Solution**: Add output/ to .gitignore, or use separate output repo
- **Alternative**: Keep outputs in git for audit trail

### 6. Cost Management
- **Challenge**: Automated processing could rack up API costs
- **Solution**: Implement daily/monthly quota limits
- **Monitoring**: Log all API calls with cost tracking

## Recommended Next Steps

1. **Start Simple**: Create the folder structure and manually process 2-3 transcripts by copying prompts into Claude Code
2. **Build Slash Commands**: Create Claude Code slash commands for each phase
3. **Automate Incrementally**: Add automation one phase at a time
4. **Gather Feedback**: Use it yourself for a week before full team rollout
5. **Iterate**: Refine prompts based on real-world output quality

## Questions to Consider

1. **Who will run the automation?**
   - Individual desktop (watcher script on your machine)
   - Shared server (team accesses centralized system)
   - Cloud service (GitHub Actions, AWS Lambda, etc.)

2. **How should outputs be shared?**
   - Git repo (commit generated files)
   - Shared drive (Dropbox, Google Drive)
   - CMS integration (directly upload to Media Manager)

3. **What level of automation is desired?**
   - Fully automated (hands-off)
   - Semi-automated (manual approval between phases)
   - Tool-assisted (Claude Code slash commands, manually triggered)

4. **How should errors be handled?**
   - Email notifications
   - Slack alerts
   - Log files only
   - Retry with exponential backoff

5. **Should historical transcripts be reprocessed?**
   - Batch process archive/ for SEO improvements
   - Only process new content going forward
