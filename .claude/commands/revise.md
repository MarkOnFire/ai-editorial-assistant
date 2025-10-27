---
description: Generate copy revision document from draft metadata (Phase 2)
---

You are executing Phase 2 of the PBS Wisconsin video editorial workflow.

## Task Overview

Analyze draft metadata (from screenshot or text) and generate a copy revision document with side-by-side comparisons and clear reasoning for edits.

## Instructions

1. **Read the system prompt** from `system_prompts/phase2_editing.md` to understand all requirements

2. **Identify the Media ID**:
   - If user provided it, use that
   - If user provided a draft screenshot path, extract from filename (e.g., "2WLI1203HD" from "2WLI1203HD_draft.png")
   - Otherwise, ask the user to specify which video they're revising

3. **Read the transcript** from `transcripts/[MEDIA_ID]*_ForClaude.txt`

4. **Read the draft copy**:
   - If user provided a screenshot path, use vision to extract the draft metadata fields
   - If screenshot is in `draft_copy/`, use that
   - If user pasted text, use that
   - Extract: Title, Short Description, Long Description, Keywords (if present)

5. **Check for program-specific knowledge**:
   - Read `knowledge/Media ID Prefixes.md` to identify which PBS Wisconsin program this is
   - Apply any program-specific rules (University Place, Here and Now, The Look Back, etc.)

6. **Generate the copy revision document** following the format in phase2_editing.md

7. **Write the output** to `output/[MEDIA_ID]/02_copy_revision.md`

8. **Inform the user**:
   - Confirm which Media ID was processed
   - Summarize the key changes recommended (2-3 main points)
   - Show the output file path
   - Ask if they'd like to proceed with Phase 3 (SEO research) or if revisions are complete

## Example Usage

With screenshot:
```
/revise draft_copy/2WLI1203HD_draft.png
```

With explicit Media ID:
```
/revise 2WLI1203HD
```
(Then paste screenshot or draft text when prompted)

## Quality Checks

Before writing output, verify:
- ✅ All original copy accurately extracted/recorded
- ✅ Character counts are exact for both original and revised
- ✅ Each revision has clear reasoning explained
- ✅ Program-specific rules applied
- ✅ No prohibited language in revisions
- ✅ Metadata header includes Media ID, timestamp, source files
