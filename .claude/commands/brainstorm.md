---
description: Generate initial brainstorming document from video transcript (Phase 1)
---

You are executing Phase 1 of the PBS Wisconsin video editorial workflow.

## Task Overview

Generate a brainstorming document with title options, descriptions, and SEO keywords based on transcript analysis.

## Instructions

1. **List available transcripts FIRST**:
   - Use Glob to find all `transcripts/*.txt` files (excluding archive subfolder)
   - Sort by modification time with newest files at the top
   - Display the list to the user with a numbered menu
   - If user provided a file path as an argument, skip the listing and use that file
   - If no argument provided, show the list and ask user to select by number or filename

2. **Read the system prompt** from `system_prompts/phase1_brainstorming.md` to understand all requirements

3. **Identify the transcript file**:
   - If user selected from the list, use that file
   - If user provided a file path as argument, use that
   - Extract Media ID from filename (e.g., "2WLI1203HD" from "2WLI1203HD_ForClaude.txt")

4. **Read the transcript** file content

5. **Check for program-specific knowledge**:
   - Read `knowledge/Media ID Prefixes.md` to identify which PBS Wisconsin program this is
   - Apply any program-specific rules (University Place, Here and Now, The Look Back, etc.)

6. **Generate the brainstorming document** following the format in phase1_brainstorming.md

7. **Create output directory** if it doesn't exist: `output/[MEDIA_ID]/`

8. **Write the output** to `output/[MEDIA_ID]/01_brainstorming.md`

9. **Inform the user**:
   - Confirm which transcript was processed
   - Show the Media ID identified
   - Show the output file path
   - Provide a brief summary (2-3 sentences) of the content
   - Ask if they want to proceed with Phase 2 after drafting metadata in their CMS

## Example Usage

```
/brainstorm transcripts/2WLI1203HD_ForClaude.txt
```

Or if the file was just added:

```
/brainstorm
```

## Quality Checks

Before writing output, verify:
- ✅ Character counts are exact (with spaces)
- ✅ Program-specific rules applied
- ✅ No prohibited language used
- ✅ Metadata header includes Media ID, timestamp, source file path
- ✅ Tables are properly formatted in Markdown
