---
description: Generate AP Style formatted transcript with proper speaker identification
---

You are generating a formatted transcript for PBS Wisconsin following AP Style Guidelines.

## Task Overview

Transform a raw video transcript into properly formatted text suitable for publication, following AP Style and PBS Wisconsin's Transcript Style Guide.

## Instructions

1. **Identify the Media ID**:
   - If user provided it, use that
   - If user provided a transcript path, extract from filename
   - Otherwise, ask the user to specify which transcript to format

2. **Read the raw transcript** from `transcripts/[MEDIA_ID]*_ForClaude.txt`

3. **Read formatting guidelines**:
   - Check `knowledge/Transcript Style Guide.pdf` for PBS Wisconsin-specific rules
   - Primary reference: `knowledge/ap_styleguide.pdf` for AP Style compliance

4. **Format the transcript** according to these rules:
   - **Speaker identification**: Use full name (first AND last) in bold for EVERY instance
     - Format: `**Jane Smith:**` (never shortened to `**Jane:**` or `**Smith:**`)
   - **New paragraph** for each speaker change
   - **Verbatim transcription**: Maintain all speaker content exactly as spoken
   - **Non-verbal cues** in square brackets: `[laughter]`, `[adjusts microphone]`, `[pauses]`
   - **AP Style compliance** for all formatting, punctuation, capitalization
   - **Preserve all content**: No omissions, maintain complete accuracy

5. **Structure the output**:
   ```markdown
   # [Subject/Program Name] - Interview Transcript

   **[Speaker Full Name]:**
   [Content of what they said...]

   **[Speaker Full Name]:**
   [Content of what they said...]
   ```

6. **Write the output** to `output/[MEDIA_ID]/05_formatted_transcript.md`

7. **Inform the user**:
   - Confirm which transcript was formatted
   - Show the output file path
   - Note any special formatting decisions made (if applicable)
   - Remind to review proper nouns, technical terms, and non-verbal cues for accuracy

## Example Usage

```
/format-transcript 2WLI1203HD
```

Or with explicit path:

```
/format-transcript transcripts/2WLI1203HD_ForClaude.txt
```

## Formatting Example

**Correct:**
```markdown
# Wisconsin Innovation - Interview Transcript

**Jane Smith:**
Hello everyone, thank you for joining today's session on climate change. [adjusts microphone] I'm excited to be here with our guest, John Doe.

**John Doe:**
Thanks for having me, Jane. I'd like to start by addressing some of the misconceptions about renewable energy.

**Jane Smith:**
Absolutely, John. Could you elaborate on what you think is the biggest misconception?

**John Doe:**
Well, Jane, I think the biggest misconception is that renewable energy is unreliable. [pauses] That's simply not supported by the data we're seeing today.
```

**Incorrect:**
```markdown
Jane: Hello everyone...
John: Thanks for having me...
```
(Missing full names, missing bold formatting, missing paragraph breaks)

## Quality Checks

Before writing output, verify:
- ✅ Every speaker instance uses full name (first AND last)
- ✅ Speaker names are in bold with colon
- ✅ New paragraph for each speaker change
- ✅ Non-verbal cues in square brackets
- ✅ AP Style compliance throughout
- ✅ Verbatim accuracy (nothing omitted or changed)
- ✅ Proper nouns capitalized correctly
- ✅ Title/header includes program or subject name

## Important Notes

- This is OPTIONAL and should only be generated when explicitly requested
- Transcript formatting is time-intensive; only do this if user specifically asks
- The formatted transcript is for publication/archival purposes
- Accuracy is paramount - when in doubt about a name or term, note it for user review
