# PBS Wisconsin Video Editorial Assistant

An AI-powered workflow for transforming video transcripts into optimized metadata (titles, descriptions, keywords, timestamps) following AP Style Guidelines and SEO best practices.

## Quick Start

### 1. Add Your Transcript

```bash
cp ~/path/to/your/transcript.txt transcripts/2WLI1203HD_ForClaude.txt
```

### 2. Generate Brainstorming Document

In Claude Code:
```
/brainstorm
```

### 3. Review & Draft in CMS

Open `output/2WLI1203HD/01_brainstorming.md` and use it to draft metadata in Media Manager.

### 4. Get Revision Recommendations

Take a screenshot of your draft, then:
```bash
cp ~/Screenshots/draft.png draft_copy/2WLI1203HD_draft.png
```

In Claude Code:
```
/revise 2WLI1203HD
```

### 5. (Optional) SEO Research

If you need keyword analysis:
```bash
cp ~/Screenshots/semrush.png semrush/2WLI1203HD_semrush.png
```

In Claude Code:
```
/research-keywords 2WLI1203HD
```

---

## Available Commands

| Command | Description | Phase |
|---------|-------------|-------|
| `/brainstorm` | Generate initial title/description options from transcript | Phase 1 |
| `/revise` | Review draft copy and provide revision recommendations | Phase 2 |
| `/research-keywords` | Conduct SEO research and generate keyword reports | Phase 3 |
| `/format-transcript` | Generate AP Style formatted transcript | Optional |
| `/create-timestamps` | Generate chapter markers for 15+ min videos | Optional |

---

## Folder Structure

```
claude-editorial-assistant/
├── transcripts/              # Drop raw transcripts here
│   ├── 2WLI1203HD_ForClaude.txt
│   └── archive/             # Move completed transcripts here
│
├── draft_copy/              # Drop draft metadata screenshots here
│   ├── 2WLI1203HD_draft.png
│   └── archive/
│
├── semrush/                 # Drop SEMRush screenshots here (optional)
│   ├── 2WLI1203HD_semrush.png
│   └── archive/
│
├── output/                  # AI-generated reports (organized by Media ID)
│   └── 2WLI1203HD/
│       ├── 01_brainstorming.md
│       ├── 02_copy_revision.md
│       ├── 03_keyword_report.md
│       ├── 04_implementation.md
│       ├── 05_formatted_transcript.md
│       └── 06_timestamp_report.md
│
├── knowledge/               # Reference materials
│   ├── ap_styleguide.pdf
│   ├── Transcript Style Guide.pdf
│   ├── WPM Generative AI Guidelines.pdf
│   └── Media ID Prefixes.md
│
├── system_prompts/          # Modular prompts for each phase
│   ├── current.md           # Full system prompt (legacy)
│   ├── phase1_brainstorming.md
│   ├── phase2_editing.md
│   ├── phase3_analysis.md
│   └── archive/
│
└── .claude/commands/        # Claude Code slash commands
    ├── brainstorm.md
    ├── revise.md
    ├── research-keywords.md
    ├── format-transcript.md
    └── create-timestamps.md
```

---

## Workflow Phases

### Phase 1: Research & Brainstorming

**Input**: Video transcript
**Output**: `01_brainstorming.md`

Analyzes transcript and generates:
- 3 title options (80 char max)
- 2 short description options (100 char max)
- 2 long description options (350 char max)
- 20 SEO keywords (direct + logical/implied)
- Notable quotes and key information

**Command**: `/brainstorm [transcript_path]`

---

### Phase 2: Copy Revision

**Input**: Transcript + Draft metadata screenshot
**Output**: `02_copy_revision.md`

Reviews your draft and provides:
- Side-by-side original vs. revised comparison
- Clear reasoning for each edit
- AP Style compliance checks
- Program-specific formatting (University Place, Here and Now, etc.)
- Updated keyword recommendations

**Command**: `/revise [media_id or screenshot_path]`

---

### Phase 3: SEO Analysis (Optional)

**Input**: Transcript + SEMRush screenshot + Web research
**Output**: `03_keyword_report.md` + `04_implementation.md`

Conducts market research and generates:
- Platform-ready keyword list (ranked by search volume)
- Trending keywords and competitive gaps
- Platform-specific recommendations (YouTube, website, social)
- Prioritized action items with timeline
- Success metrics to track

**Command**: `/research-keywords [media_id or screenshot_path]`

---

### Phase 4: Optional Outputs

#### Formatted Transcript
**Input**: Raw transcript
**Output**: `05_formatted_transcript.md`

AP Style formatted transcript with:
- Full speaker names (first AND last) for every instance
- Proper paragraph breaks
- Non-verbal cues in brackets
- Publication-ready formatting

**Command**: `/format-transcript [media_id]`

#### Timestamp Report
**Input**: Transcript (15+ minute videos only)
**Output**: `06_timestamp_report.md`

Chapter markers for:
- Media Manager (table format with start/end times)
- YouTube (simple timestamp list)
- 5-10 logical chapter breaks
- Descriptive chapter titles

**Command**: `/create-timestamps [media_id]`

---

## File Naming Convention

Transcripts follow PBS Wisconsin's Media ID system:

**Format**: `[PREFIX][NUMBER][FORMAT]_[REVISION]_ForClaude.txt`

- **PREFIX**: Program identifier (see `knowledge/Media ID Prefixes.md`)
- **NUMBER**: Episode/segment number
- **FORMAT**: HD, HDWEB, SM (shortform/social media)
- **REVISION**: REV + date (YYYYMMDD) if applicable

**Examples**:
- `2WLI1203HD_ForClaude.txt` - Wisconsin Life episode 1203, HD
- `9UNP1972HD_REV20250804_ForClaude.txt` - University Place episode 1972, revised 2025-08-04
- `6HNS_ForClaude.txt` - Here and Now Digital Short

---

## Editorial Standards

### AP Style & House Style
- **Down style for headlines**: Only first word and proper nouns capitalized
- **No dashes/colons in titles**; preserve necessary apostrophes and quotations
- **Character limits**: Title (80), Short Description (100), Long Description (350)
- **Title + Short Description pairing**: Must work cohesively (appear together in search results)

### Prohibited Language
❌ Never use:
- Viewer directives: "watch as", "see how", "discover", "learn"
- Promises: "will show", "will teach", "will reveal"
- Sales language: "free", monetary value framing
- Superlatives without evidence: "amazing", "incredible"
- Calls to action: "join us", "don't miss"

✅ Instead:
- State what the content IS
- Describe what happens (facts only)
- Use specific details over promotional adjectives

### Program-Specific Rules

#### University Place
- Include series name as keyword (if part of series)
- Don't use honorific titles ("Dr.", "Professor")
- Avoid inflammatory/bombastic language

#### Here and Now
- **Title**: [SUBJECT] on [topic]
- **Long**: [Org] [title] [name] [verb] [subject]
  - "discuss" for elected officials
  - "explain"/"describe" for others
  - Include party/location for elected (R-Rochester)
- **Short**: [name] on [subject] (simplified from long)

#### The Look Back
- MUST include: Hosts (Nick and Taylor), locations, expert historians
- Focus on WHY it matters > WHAT happened
- Use precise language showing deliberate decisions

---

## Typical Workflow Example

### Standard Video (e.g., Wisconsin Life Episode)

1. **Add transcript** → `/brainstorm` → Review suggestions
2. **Draft in CMS** → Screenshot → `/revise` → Implement edits
3. **(Optional)** SEMRush research → `/research-keywords` → Optimize
4. **Archive inputs** → Done!

**Time**: ~10-15 minutes (vs. 30-45 minutes manual)

### Shortform Content (e.g., Digital Short)

1. **Add transcript(s)** → `/brainstorm` → Get social-optimized copy
2. **Draft in CMS** → Screenshot → `/revise` → Implement edits
3. **Archive inputs** → Done!

**Time**: ~5-10 minutes per short

### Long-Form Educational (e.g., University Place Lecture)

1. **Add transcript** → `/brainstorm` → Review suggestions
2. **Draft in CMS** → Screenshot → `/revise` → Implement edits
3. **SEMRush research** → `/research-keywords` → Optimize for educational search
4. **(Optional)** `/create-timestamps` → Add chapter markers
5. **Archive inputs** → Done!

**Time**: ~15-20 minutes

---

## Archiving Workflow

When you're done with a video:

```bash
# Move inputs to archive
mv transcripts/2WLI1203HD_ForClaude.txt transcripts/archive/
mv draft_copy/2WLI1203HD_draft.png draft_copy/archive/
mv semrush/2WLI1203HD_semrush.png semrush/archive/
```

The `output/2WLI1203HD/` folder remains as your permanent record.

---

## Ethical AI Guidelines

All outputs include a note about ethical AI collaboration:

> This is AI-generated brainstorming content. Ethical use of generative AI involves collaboration and coaching between the AI and human user. Use this content to advise your own writing and editing, not to publish AI-generated content without review and revision.

**Key Principles**:
- AI provides recommendations based on best practices
- Human editor makes final decisions
- Always review and revise before publishing
- Maintain editorial integrity and factual accuracy

---

## Troubleshooting

### Command not found
- Make sure you're in the repository directory
- Check that `.claude/commands/` folder exists
- Try `/help` to see available commands

### Screenshot not reading correctly
- Ensure screenshot shows all metadata fields clearly
- Try providing text manually instead
- Check that filename follows convention: `[MEDIA_ID]_draft.png`

### Character counts seem off
- All counts include spaces
- Use exact character count from output files
- CMS may count differently—trust the AI counts

### Program-specific rules not applied
- Check that Media ID matches a program in `knowledge/Media ID Prefixes.md`
- Filename must start with correct prefix (e.g., `2WLI`, `9UNP`, `2HNW`)

---

## Advanced: Future Automation

See **[AUTOMATION_PLAN.md](AUTOMATION_PLAN.md)** for plans to automate this workflow with:
- File watching (auto-process when transcripts added)
- Python scripts + Claude API integration
- MCP server for Claude Code
- Batch processing capabilities

---

## Documentation

- **[CLAUDE.md](CLAUDE.md)** - Guidance for Claude Code when working in this repo
- **[AUTOMATION_PLAN.md](AUTOMATION_PLAN.md)** - Future automation roadmap
- **[Possible improvements.md](Possible%20improvements.md)** - Feature requests and enhancements
- **`output/EXAMPLE_2WLI1203HD/README.md`** - Example output structure

---

## Support

For issues or questions:
1. Check this README
2. Review the specific slash command file in `.claude/commands/`
3. Check system prompt in `system_prompts/`
4. Consult knowledge base in `knowledge/`

---

**Version**: File-based workflow for Claude Code
**Last Updated**: 2025-10-22
