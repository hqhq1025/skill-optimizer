# Session Analysis

Use the strongest evidence available, and say what kind of evidence you used.

## Installed-Skill Mode

Use this when the skills are already installed under an agent skill directory.

Scan these locations if they exist:

| Source | Claude Code | Codex | Shared |
|--------|-------------|-------|--------|
| Skill files | `~/.claude/skills/*/SKILL.md` | `~/.codex/skills/*/SKILL.md` | `~/.agents/skills/*/SKILL.md` |
| Session transcripts | `~/.claude/projects/**/*.jsonl` | `~/.codex/sessions/**/*.jsonl` | — |

Notes:

- A user may have more than one platform installed. Scan every relevant location and deduplicate by skill name.
- For Claude Code, explicit `Skill` tool use is strong invocation evidence.
- For Codex, skill loading in `base_instructions` is not enough. Look for workflow markers, structured output sections, or explicit prompt/result evidence that the skill actually shaped the answer.

## Source-Repository Mode

Use this when the skills live in a repository that is being prepared, validated, or published, but may not yet have enough live routing history.

Prefer these sources:

- repository `skills/` folders and their `SKILL.md` frontmatter
- `references/` files, `agents/openai.yaml`, and repo-owned validators
- validation logs, review checklists, CI workflows, and recorded forward tests
- local session transcripts that show maintainers working on the repository

Important:

- Maintainer sessions about authoring the repository are not the same as end-user skill consumption.
- If the only evidence is authoring or curated validation, mark trigger rate, user reaction, and undertrigger findings as low confidence or `N/A`.
- Still run all 8 dimensions. The goal is to expose evidence gaps, not to pretend they do not exist.
- Judge severity in context. A docs-first beta repo that openly says its skills are still being validated does not have the same routing-evidence burden as a repo claiming production-ready agent routing.

## Shell Guidance

- On Windows, prefer PowerShell-native commands.
- On macOS/Linux, prefer the native POSIX shell.
- Do not require Bash-specific syntax when the environment does not provide Bash.

## Evidence Ranking

Prefer evidence in this order:

1. direct invocation evidence from session transcripts
2. prompt/result validation logs and forward-test records
3. repository validators, CI, and static structure
4. inferred risk from wording overlap or missing boundaries

Lower-ranked evidence can support a claim, but should not be presented as if it were direct usage telemetry.
