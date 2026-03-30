# Report Template

Use this shape unless the user asks for something else.

```markdown
# Skill Optimization Report
**Date**: {date}
**Scope**: {all / specified skills}
**Evidence**: {session count, validation logs, CI, validators, etc.}
**Confidence**: {high / medium / low}
**Release stage**: {production / public beta / docs-first / draft}

## Overview
| Skill | Trigger | Reaction | Completion | Static | Undertrigger | Token | Score |
|-------|---------|----------|------------|--------|--------------|-------|-------|
| example-skill | 2/7 | 100% | 86% | strong | 1 miss | 320w | 4/5 |

## P0 Fixes
1. ...

## P1 Improvements
1. ...

## P2 Optional Optimizations
1. ...

## Milestone Fit
- current-milestone blockers: ...
- next-milestone evidence work: ...

## Per-Skill Diagnostics
### {skill-name}
#### 4.1 Trigger Rate
...
#### 4.2 User Reaction
...
#### 4.3 Workflow Completion
...
#### 4.4 Static Quality
...
#### 4.5a Overtrigger
...
#### 4.5b Undertrigger
...
#### 4.6 Cross-Skill Conflicts
...
#### 4.7 Environment Consistency
...
#### 4.8 Token Economics
...
```

Keep the report honest:

- `N/A` is better than a fake metric.
- Call out confidence level whenever the evidence is mostly static or curated.
- Quote or summarize the exact user task when claiming undertrigger.
- If the repository is still in a draft or beta stage, say whether a missing proof item blocks the current milestone or only the next maturity step.
