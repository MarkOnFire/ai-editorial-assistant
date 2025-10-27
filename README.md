# AI Editorial Assistant

This repository collects the working instructions and reference material used to run an AI-powered video metadata editor. It is tuned for assistants that transform long-form transcripts into AP-style titles, descriptions and supporting SEO collateral while collaborating with a human editor. Many style and output decisions assume you are working within a public media environment, uploading content to [PBS Media Manager](https://media.console.pbs.org/admin/shows/) and [YouTube](https://studio.youtube.com/channel/UCtnFS8kY2D3VLaEtt_Jk2ZA) as your primary platforms, and have access to an SEO analysis platform like [SEMRush](https://www.semrush.com/). 

- `custom_instructions/` contains the current system prompt used in production (`Haiku 4.5 version.md`) plus archived iterations under `earlier_versions/`. Update the primary file when rolling out prompt changes and move superseded copies into the archive so the evolution stays traceable.
- `project_knowledge/` stores the static resources the assistant can cite: AP style references, timestamp examples, program-specific guidelines, and other evergreen context that inform editing decisions. Keep organization tight and favor concise summaries over raw source dumps when possible.
- `Possible improvements.md` is a running backlog of future enhancements—richer reference packs, alternative model strategies and workflow ideas such as batch editing.

### Using the repository

1. Load the latest instructions into your AI platform’s custom-instructions slot — the most recent version had the best results with Anthropic Claude Haiku 4.5. Confirm the character-count rules, program checklists and delivery templates still reflect current production needs.
2. Sync `project_knowledge/` with whatever handbooks, PDFs or screenshots the assistant should treat as authoritative. Replace outdated documents promptly so the guidance stays consistent.
3. When experimenting with prompt revisions, work in a branch and keep detailed commit messages. This makes it easy to compare behaviour across versions and roll back if a change underperforms.

### Tips

- Review the backlog in `Possible improvements.md` during planning sessions and promote accepted ideas into real tasks.
- If you add large binaries (PDFs, PNGs), verify they are under version-control limits or consider linking to an external CMS.
- Document notable prompt changes (what was tweaked and why) inside the top of the new instruction file so future editors understand the intent.
- The tools for short form video are very, very early — plan on using those reports exclusively for brainstorming or getting an initial draft of social copy together.
- This branch is primarily here for archival purposes, shifted to an agent-approach in October 2025.
