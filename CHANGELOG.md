# Changelog

## v2.0.0 - Skill Lifecycle Toolkit

This is a major redesign of Skill Optimizer.

### Changed

- Replaced the single `skill-optimizer` skill with three focused lifecycle skills:
  - `skill-miner`
  - `skill-personalizer`
  - `skill-generalizer`
- Moved optimizer-style auditing into `skill-personalizer`.
- Added deterministic session mining via `skills/skill-miner/scripts/scan_sessions.py`.
- Added support for active sessions, archived Codex sessions, rollout summaries, Gemini/Antigravity task files, and exported transcripts.
- Added configurable workflow mining patterns in `skills/skill-miner/references/patterns.json`.

### Added

- Codex UI metadata via `agents/openai.yaml` for all three skills.
- Research background documentation.
- AI-readable project files: `llms.txt`, `llms-full.txt`, and `repo-metadata.json`.
- GitHub issue templates, PR template, `CITATION.cff`, `robots.txt`, and `sitemap.xml`.

### Breaking

- The old `skills/skill-optimizer/SKILL.md` path was removed.
- Users should install the new `skill-miner`, `skill-personalizer`, and `skill-generalizer` folders.

## v1.0.0 - Initial Release

- Initial single-skill optimizer release.
