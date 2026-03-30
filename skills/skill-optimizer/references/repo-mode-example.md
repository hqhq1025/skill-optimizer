# Repo-Mode Example

This is a good pattern for auditing a public skill repository that is still maturing.

## Example Scenario

- the repository has 4 public skills
- each skill has a concise `SKILL.md`, `references/`, and example prompts
- CI and local validators pass
- there are validation logs and forward tests
- most historical sessions are maintainer sessions about building the repo, not clean installed-skill consumption
- the repository describes itself as docs-first, draft, or beta

## Correct Audit Conclusion

- static quality can still score highly
- workflow completion can often be judged from validation logs
- trigger rate, user reaction, and undertrigger may need to be `N/A` or low confidence
- missing live routing evidence is usually the top `P1` or "next-milestone evidence work", not automatically `P0`

## When It Becomes P0

Upgrade that same gap to `P0` only if one of these is true:

- the repository claims the skills are already routing-proven in real agent use
- the release gate explicitly requires transcript-backed routing evidence
- users would be misled into assuming the routing quality is already proven

## Recommended Next Step

Add one small routing-eval pack instead of building a huge telemetry system first:

1. one installed-skill transcript or replayable eval prompt per public skill
2. one nearby non-use prompt for each adjacent skill pair
3. one short note separating "repo-authoring evidence" from "installed-skill usage evidence"
