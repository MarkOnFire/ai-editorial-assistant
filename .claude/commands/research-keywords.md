---
description: Generate keyword research and implementation reports (Phase 3)
---

You are executing Phase 3 of the PBS Wisconsin video editorial workflow.

## Task Overview

Conduct market research, analyze SEMRush data, and generate comprehensive keyword and implementation reports with search volume rankings and prioritized action items.

## Instructions

1. **Read the system prompt** from `system_prompts/phase3_analysis.md` to understand all requirements

2. **Identify the Media ID**:
   - If user provided it, use that
   - If user provided a SEMRush screenshot path, extract from filename (e.g., "2WLI1203HD" from "2WLI1203HD_semrush.png")
   - Otherwise, ask the user to specify which video they're researching

3. **Read the transcript** from `transcripts/[MEDIA_ID]*_ForClaude.txt`

4. **Read previous documents for context** (if they exist):
   - `output/[MEDIA_ID]/01_brainstorming.md`
   - `output/[MEDIA_ID]/02_copy_revision.md`
   - This helps understand what keywords were already considered

5. **Read SEMRush data**:
   - If user provided a screenshot path, use vision to extract keyword data
   - If screenshot is in `semrush/`, use that
   - If user pasted data/text, use that
   - Extract: Keywords, search volumes, difficulty scores, trends

6. **Conduct web research**:
   - Use WebSearch tool to research trending keywords related to the transcript content
   - Identify competitor content and keyword gaps
   - Research hashtag trends for social media (if shortform content)
   - Assess seasonal factors and optimization timing

7. **Generate TWO documents**:
   - Keyword Report following format in phase3_analysis.md
   - Implementation Report following format in phase3_analysis.md

8. **Write the outputs**:
   - `output/[MEDIA_ID]/03_keyword_report.md`
   - `output/[MEDIA_ID]/04_implementation.md`

9. **Inform the user**:
   - Confirm which Media ID was processed
   - Highlight the top 3-5 highest-opportunity keywords discovered
   - Summarize the most critical implementation recommendation
   - Show both output file paths
   - Remind that these are recommendations to enhance discoverability while maintaining editorial integrity

## Example Usage

With SEMRush screenshot:
```
/research-keywords semrush/2WLI1203HD_semrush.png
```

With explicit Media ID:
```
/research-keywords 2WLI1203HD
```
(Then paste screenshot or SEMRush data when prompted)

## Quality Checks

Before writing output, verify:
- ✅ Both keyword report AND implementation report generated
- ✅ All keywords ranked by multiple criteria (volume, difficulty, relevance)
- ✅ Web research conducted (not just SEMRush data)
- ✅ Platform-ready comma-separated keyword list included
- ✅ Timeline and success metrics included
- ✅ Recommendations maintain editorial integrity (no keyword stuffing, no misleading terms)
- ✅ Metadata headers include Media ID, timestamp, source files

## Important Notes

- This phase is OPTIONAL and only used when keyword research is needed
- Balance discoverability with editorial integrity
- Never sacrifice factual accuracy for SEO
- Keywords should appear natural, not forced
