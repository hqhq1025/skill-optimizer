# Audit Framework

Run all 8 dimensions every time.

If a dimension lacks enough evidence, report `N/A — insufficient session data` or `low confidence — inferred from source-repo evidence`.

## 4.1 Trigger Rate

- Compare actual invocations with relevant user-task opportunities.
- Never treat Codex skill loading by itself as invocation.

## 4.2 Post-Invocation User Reaction

- Read the next few user turns after invocation.
- Classify positive, correction, negative, or silent switch.

## 4.3 Workflow Completion Rate

- Extract the skill's intended steps from `SKILL.md`.
- Check whether the assistant output completed those steps, or whether validation logs show the workflow reaching the expected end state.

## 4.4 Static Quality Analysis

Check at least these areas:

| Check | Pass Criteria |
|-------|---------------|
| Frontmatter format | Only `name` + `description`, total < 1024 chars |
| Name format | Letters, numbers, hyphens only |
| Description trigger | Starts with "Use when..." or contains explicit trigger conditions |
| Description workflow leak | Do not front-load step-by-step workflow or output-format detail in frontmatter |
| Description disambiguation | Short action anchors are acceptable when they separate adjacent skills |
| Description pushiness | Claims the right use cases instead of staying purely passive |
| Overview section | Present |
| Rules section | Present |
| Word count | Flag if the body is unnecessarily large |
| Narrative anti-pattern | Avoid post-hoc storytelling inside the core skill |
| YAML quoting safety | Quote descriptions that contain `: ` |
| Critical info position | Core trigger and boundary info appears early |
| Description 250-char check | Primary routing clues appear before common truncation points |
| Progressive disclosure | Detailed content lives in `references/` when possible |

## 4.5a Overtrigger

- Look for invocations that were immediately rejected, corrected, or abandoned.

## 4.5b Undertrigger

- Look for user tasks that match the skill's capability but did not result in invocation.
- In source-repo mode, treat this as low confidence unless you have direct routing evidence.

## 4.6 Cross-Skill Conflicts

Compare adjacent skills for:

- overlapping trigger vocabulary
- overlapping workflow territory
- contradictory boundaries or guidance

## 4.7 Environment Consistency

Check referenced:

- file paths
- directories
- CLI tools
- repo-owned docs or examples

Document intentional host-side prerequisites separately from broken references.

## 4.8 Token Economics

Consider:

- description length
- body length
- progressive disclosure quality
- trigger frequency when available

Large skills without references or usage evidence are candidates for compression.

## Composite Score

Use a 5-point scale:

| Score | Meaning |
|-------|---------|
| 5 | Strong live routing evidence, healthy outcomes, clean static quality |
| 4 | Good static quality and workflow evidence, but some gaps remain |
| 3 | Meaningful concerns or missing proof in multiple dimensions |
| 2 | Major routing, quality, or environment problems |
| 1 | Broken or fundamentally misaligned |

Redistribute weights if a scored dimension is `N/A`.

## Severity Calibration

Do not treat every evidence gap as the same class of problem.

Use `P0` when:

- the repository makes claims that are contradicted by the evidence
- a skill is broken, misleading, or unsafe to use
- the current release or milestone explicitly depends on live routing proof

Use `P1` when:

- the repository is honest that it is draft, beta, docs-first, or still collecting validation evidence
- the missing item is the next most valuable proof step, but not a contradiction of current public claims
- the issue weakens confidence more than it breaks current usability

Use `P2` when:

- the issue is mostly polish, wording, or future-proofing
- fixing it would improve routing odds but is not the main confidence bottleneck
