# Skill Optimizer

[中文版](./README.zh-CN.md)

Three Agent Skills for turning coding-agent work into better `SKILL.md` files:

- **skill-miner** — mine coding-agent history, archives, memories, and repeated work to surface skill-worthy workflows with evidence.
- **skill-personalizer** — audit and adapt newly created, downloaded, forked, or community skills to one user's own tools, habits, directories, and session history.
- **skill-generalizer** — turn local, private, personal skills into publishable skills for GitHub, marketplaces, teams, or public sharing.

The split is intentional. Generating, personalizing, and publishing skills are different jobs:

| Goal | Skill | Optimization Direction |
| --- | --- | --- |
| Mine repeated workflows | `skill-miner` | Scan real agent usage, cluster repeated workflows, and draft evidence-backed candidate skills. |
| Fit inward | `skill-personalizer` | Preserve the original optimizer's audit checks, then add local defaults, user phrasing, preferred tools, verification habits, and workflow shortcuts. |
| Publish outward | `skill-generalizer` | Remove private context, generalize examples, make install and README claims portable. |

See [Research Background](./docs/research-background.md) for related agent-skill ecosystems, comparable projects, and papers that motivate session mining, skill libraries, trigger auditing, progressive disclosure, and lifecycle governance.

## Installation

Copy the command below into your agent's chat:

### Claude Code

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer
```

### Codex

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer into ~/.codex/skills/
```

### Other Agent Skills-compatible agents

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer into ~/.agents/skills/
```

Manual install:

```bash
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
mkdir -p ~/.agents/skills
cp -r /tmp/skill-optimizer/skills/skill-miner ~/.agents/skills/
cp -r /tmp/skill-optimizer/skills/skill-personalizer ~/.agents/skills/
cp -r /tmp/skill-optimizer/skills/skill-generalizer ~/.agents/skills/
rm -rf /tmp/skill-optimizer
```

For Codex-only installs, use `~/.codex/skills/`:

```bash
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
mkdir -p ~/.codex/skills
cp -r /tmp/skill-optimizer/skills/skill-generalizer ~/.codex/skills/
cp -r /tmp/skill-optimizer/skills/skill-miner ~/.codex/skills/
cp -r /tmp/skill-optimizer/skills/skill-personalizer ~/.codex/skills/
rm -rf /tmp/skill-optimizer
```

For Claude Code-only installs, use `~/.claude/skills/`.

## Platform Support

| Agent | Support level | Recommended path |
| --- | --- | --- |
| Codex | Native Agent Skills, plus optional plugin metadata. | `~/.codex/skills/` or `.agents/skills/` |
| Claude Code | Native skills in personal, project, and plugin scopes. | `~/.claude/skills/` or `.claude/skills/` |
| Cursor | Native Agent Skills and rules/commands; skills are discoverable by Agent. | `.agents/skills/`, `.cursor/skills/`, or global skills |
| OpenCode | Native `skill` tool and repo/home skill discovery. | `.agents/skills/`, `.opencode/skills/`, or `~/.config/opencode/skills/` |
| Gemini CLI / Google agents | Agent Skills open format is documented by Google; `GEMINI.md` remains the always-on context mechanism. | `.agents/skills/` or installer-managed skills |

The safest public layout is `skills/<name>/SKILL.md` in the repo plus install instructions that copy into `.agents/skills/` or the target agent's native skill directory.

## Usage

Ask for the direction you want:

```text
Mine my coding-agent history and find repeated workflows that should become skills.
```

```text
Audit and tune my installed skills; tell me which ones are undertriggering, too noisy, or too generic.
```

```text
Turn this local skill into a public GitHub-ready skill.
```

```text
I downloaded this skill. Tune it to my local workflow and usage habits.
```

```text
This skill does not trigger when I say things naturally. Personalize it for me.
```

## What Each Skill Does

### skill-miner

- coding-agent session history, memory summaries, repo notes, repeated scripts, and project folders
- recurring user intents, shorthand, tool chains, artifacts, and verification patterns
- candidates that are repeated and non-obvious enough to become skills
- whether a candidate should stay personal, be generalized for publication, or be skipped
- includes `scripts/scan_sessions.py` for a deterministic first-pass scan of Codex, Claude Code, Gemini/Antigravity task files, and exported transcripts from other agents
- includes archived Codex sessions and rollout summaries by default, with flags to disable archive/summary sources

Example:

```bash
python3 skills/skill-miner/scripts/scan_sessions.py --days 30 --limit 300 --min-count 3
python3 skills/skill-miner/scripts/scan_sessions.py --export ~/Downloads/cursor-chat-export.json
python3 skills/skill-miner/scripts/scan_sessions.py --patterns ./my-patterns.json
python3 skills/skill-miner/scripts/scan_sessions.py --no-include-archives --no-include-summaries
```

### skill-generalizer

- private paths, hosts, credentials, account names, transcript quotes, and internal repo facts
- public portability of commands, examples, README claims, and install instructions
- frontmatter that describes when to use the skill rather than the workflow
- packaging structure for public distribution

### skill-personalizer

- local installed copies and nearby project instructions
- real user phrasing and recurring task patterns
- preferred CLIs, MCP tools, paths, aliases, and verification commands
- undertrigger, overtrigger, and unnecessary-question friction
- original optimizer-style audit checks: trigger fit, user reaction, workflow completion, static quality, conflicts, environment consistency, token economics, and P0/P1/P2 fixes

## Compatibility

Works with agents that support the Agent Skills folder convention:

- Claude Code
- Codex
- Cursor
- OpenCode
- Gemini CLI

## Research Background

This project is informed by Agent Skills ecosystem work and LLM-agent research on externalized memory, skill libraries, retrieval/routing, and long-context behavior. See [docs/research-background.md](./docs/research-background.md).

## License

MIT
