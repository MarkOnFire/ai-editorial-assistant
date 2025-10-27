# Example Output Structure

This folder demonstrates how outputs are organized for each Media ID.

## Folder Contents

When you process a transcript through the PBS Wisconsin editorial workflow, you'll get numbered markdown files corresponding to each phase:

### Core Workflow (Always Generated)

- **`01_brainstorming.md`** - Phase 1 output with title options, descriptions, and initial keywords
- **`02_copy_revision.md`** - Phase 2 output comparing your draft copy to AI recommendations
- **`03_keyword_report.md`** - Phase 3 output with SEO research and keyword rankings (optional, only when you provide SEMRush data)
- **`04_implementation.md`** - Phase 3 output with prioritized action items (generated with keyword report)

### Optional Outputs (Generated on Request)

- **`05_formatted_transcript.md`** - AP Style formatted transcript with proper speaker identification
- **`06_timestamp_report.md`** - Chapter markers for Media Manager and YouTube (15+ minute videos only)

## Workflow Example

### Step 1: Add Transcript
```bash
cp ~/Downloads/2WLI1203HD_ForClaude.txt transcripts/
```

Then run:
```
/brainstorm
```

Result: `output/2WLI1203HD/01_brainstorming.md` is created

---

### Step 2: Draft Metadata in CMS

- Open `01_brainstorming.md`
- Use suggestions to create draft in Media Manager
- Take screenshot of draft

---

### Step 3: Add Draft Screenshot
```bash
cp ~/Screenshots/draft.png draft_copy/2WLI1203HD_draft.png
```

Then run:
```
/revise 2WLI1203HD
```

Result: `output/2WLI1203HD/02_copy_revision.md` is created

---

### Step 4 (Optional): SEO Research

If you need keyword research:

```bash
cp ~/Screenshots/semrush.png semrush/2WLI1203HD_semrush.png
```

Then run:
```
/research-keywords 2WLI1203HD
```

Result: Both `03_keyword_report.md` and `04_implementation.md` are created

---

### Step 5 (Optional): Additional Outputs

For formatted transcript:
```
/format-transcript 2WLI1203HD
```

For timestamps (15+ min videos):
```
/create-timestamps 2WLI1203HD
```

---

## File Naming Convention

All files use the Media ID as the folder name and numbered files for easy sequencing:

```
output/
├── 2WLI1203HD/          # Wisconsin Life episode
│   ├── 01_brainstorming.md
│   ├── 02_copy_revision.md
│   └── ...
├── 9UNP1972HD/          # University Place episode
│   ├── 01_brainstorming.md
│   └── ...
└── 6HNS/                # Here and Now Digital Short
    ├── 01_brainstorming.md
    └── ...
```

This makes it trivial to find all outputs for a specific video.

## Archiving Completed Work

When you're done with a video, move inputs to archive folders:

```bash
mv transcripts/2WLI1203HD_ForClaude.txt transcripts/archive/
mv draft_copy/2WLI1203HD_draft.png draft_copy/archive/
mv semrush/2WLI1203HD_semrush.png semrush/archive/
```

The `output/2WLI1203HD/` folder remains as your permanent record of AI-generated recommendations.
