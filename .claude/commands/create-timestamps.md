---
description: Generate timestamp report with chapter markers for videos 15+ minutes
---

You are generating a timestamp report for PBS Wisconsin with chapter markers for both Media Manager and YouTube.

## Task Overview

Analyze a video transcript to create logical chapter markers with timestamps, providing both Media Manager format (table with start/end times) and YouTube format (simple list).

**IMPORTANT**: This should ONLY be used for videos 15+ minutes in length.

## Instructions

1. **Identify the Media ID**:
   - If user provided it, use that
   - If user provided a transcript path, extract from filename
   - Otherwise, ask the user to specify which transcript needs timestamps

2. **Read the transcript** from `transcripts/[MEDIA_ID]*_ForClaude.txt`

3. **Verify video length**:
   - Ask user for video duration if not obvious from transcript
   - If video is under 15 minutes, recommend not creating chapters (too short)
   - Proceed only if 15+ minutes

4. **Analyze transcript for chapter breaks**:
   - Identify natural transitions: topic changes, speaker changes, segment shifts
   - Long pauses followed by host/narrator speaking on a new topic are strong chapter markers
   - For educational content: break by subtopics or themes
   - For interviews: break by discussion topics or questions
   - Create 5-10 chapters for most videos (adjust based on content length)

5. **Create descriptive chapter titles**:
   - Keep titles concise (3-6 words ideal)
   - Start with "Intro" at 0:00 (or "Opening" if more appropriate)
   - Ensure chapters follow the narrative arc of the content
   - Make titles descriptive but not overly detailed

6. **Estimate timestamps**:
   - Based on AI analysis of transcript content flow
   - Note that these are APPROXIMATIONS and user must verify on actual video

7. **Generate BOTH formats**:
   - Media Manager format: Table with Title, Start Time (H:MM:SS.000), End Time
   - YouTube format: Simple list with timestamps and titles

8. **Check sample images** for format reference:
   - `knowledge/Media Manager timestamp sample.png`
   - `knowledge/YouTube timestamp sample.png`

9. **Write the output** to `output/[MEDIA_ID]/06_timestamp_report.md`

10. **Inform the user**:
    - Confirm which video was processed
    - Note that timestamps are approximations requiring verification
    - Show the output file path
    - Remind to check timestamps against actual video before publishing

## Example Usage

```
/create-timestamps 2WLI1203HD
```

## Output Format

```markdown
# Timestamp Report - [Media ID]

**Generated**: [Timestamp]
**Transcript**: [Path to source file]

⚠️ **IMPORTANT**: Timestamps are approximations based on AI analysis of the transcript. Please confirm on actual video before publishing.

---

## Media Manager Format

**Video Duration**: [H:MM:SS.000]

| Title | Start Time | End Time |
|-------|------------|----------|
| Intro | 0:00:00.000 | 0:XX:XX.000 |
| [Chapter 2 Title] | 0:XX:XX.000 | 0:XX:XX.000 |
| [Chapter 3 Title] | 0:XX:XX.000 | 0:XX:XX.000 |
| [Chapter 4 Title] | 0:XX:XX.000 | 0:XX:XX.000 |

**Notes**: [Any relevant information about chapter selection or content flow]

---

## YouTube Format

Copy and paste into YouTube description:

```
0:00 Intro
X:XX [Chapter 2 Title]
X:XX [Chapter 3 Title]
X:XX [Chapter 4 Title]
```

**Notes**: [Any relevant information about chapter selection or content flow]
```

## Chapter Creation Guidelines

- **Start with "Intro" at 0:00** (or "Opening" if more appropriate)
- **Create 5-10 chapters** for most videos (adjust based on content)
- **Chapter titles**: Descriptive but concise (3-6 words ideal)
- **Natural transitions**: Identify topic changes, speaker changes, segment shifts
- **Long pauses + new topic** = strong chapter marker
- **Educational content**: Break by subtopics or themes
- **Interviews**: Break by discussion topics or questions
- **Follow narrative arc**: Ensure chapters tell the story of the content flow
- **YouTube format**: Each timestamp on its own line

## Quality Checks

Before writing output, verify:
- ✅ Video is 15+ minutes (if not, recommend against chapters)
- ✅ 5-10 chapters created (appropriate for content length)
- ✅ Starts with "Intro" at 0:00
- ✅ Chapter titles are descriptive and concise
- ✅ Both formats provided (Media Manager table + YouTube list)
- ✅ Warning about approximations included prominently
- ✅ Video duration noted in Media Manager format
- ✅ Notes section includes relevant context

## Important Notes

- This is OPTIONAL and should only be generated when explicitly requested
- Only suitable for videos 15+ minutes in length
- Timestamps are APPROXIMATIONS - user must verify against actual video
- Chapter quality depends on transcript clarity and structure
- Some transcripts may not have clear chapter break points
