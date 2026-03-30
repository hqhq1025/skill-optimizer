---
name: skill-optimizer
description: "Use when the user wants to analyze, audit, or improve their Agent Skills (SKILL.md files). Triggers on /optimize-skill, /skill-audit, 'optimize skills', 'analyze skills', 'check my skills', 'skill quality'. Also use proactively when the user mentions skills aren't triggering, skills feel broken, or asks why a skill didn't fire."
---

## Rules

- Audit all 8 dimensions. If data is missing, report `N/A — insufficient session data` instead of inventing a number.
- Default to read-only audit mode, but if the user explicitly asks to improve the skill or skill repository after the audit, you may apply the agreed fixes.
- Prefer real session transcripts. When auditing a source repository or docs-first skill collection with sparse invocation history, fall back to validator output, example prompts, validation logs, CI, and review checklists. Label those conclusions as lower confidence.
- For Codex, skill loading is not the same as skill invocation. Look for workflow markers, explicit prompt/result evidence, or repo-owned validation records before claiming the skill was actually used.
- Calibrate severity against repository maturity and public claims. Missing live routing evidence is a true P0 only when the repository claims the skills are routing-proven, production-ready, or already validated in real agent use. For honest docs-first, draft, or beta repositories, report it as the highest-priority P1 or next-milestone evidence gap instead.
- Distinguish a true workflow leak from a short action anchor in the description. Brief action language is acceptable when it separates adjacent skills; flag only step-by-step execution detail or output-format leakage in frontmatter.
- Use the native shell for the host OS. Prefer PowerShell on Windows and a POSIX shell elsewhere; do not require Bash if it is unavailable.
- Quantify with evidence whenever possible, and cite the research basis for rewrite suggestions.

## Overview

Analyze skills with **session data + static quality checks** and output a prioritized report with P0/P1/P2 fixes. This skill supports both installed-skill audits and source-repository audits for public skill collections that are still building usage evidence.

## Quick Start

1. Read `references/session-analysis.md` and collect the strongest available evidence set.
2. Read `references/audit-framework.md` and run all 8 dimensions.
3. Read `references/report-template.md` and deliver the report in that shape.

## Output Format

When answering:

1. state the scope, evidence set, and confidence level
2. show an overview table with `N/A` where data is missing
3. list P0, P1, and P2 findings
4. provide all 8 dimensions for each audited skill
5. separate findings about the audited skills from findings about the optimizer itself if both are in scope
6. if relevant, distinguish blockers for the repository's current milestone from evidence work needed for the next maturity step

## Resources

- `references/session-analysis.md`: installed-skill mode, source-repo mode, and platform-specific evidence collection
- `references/audit-framework.md`: the 8 dimensions, scoring rules, and static-check rubric
- `references/report-template.md`: report shape and wording guidance
- `references/repo-mode-example.md`: example reasoning for a docs-first public skill repository with sparse routing telemetry
