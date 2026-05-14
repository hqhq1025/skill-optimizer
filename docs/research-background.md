# Research Background

This project treats skills as lifecycle-managed agent infrastructure: discover repeated workflows, audit and personalize existing skills for local use, and generalize personal skills for publication.

## Design Claims

| Claim | Support | Applied In |
| --- | --- | --- |
| Skills should be portable, progressively disclosed artifacts rather than one huge prompt. | Agent Skills for LLMs survey describes portable skill definitions, progressive context loading, and the relationship between skills and MCP. | `skill-generalizer`, static audit checks, reference/script split. |
| Agents can improve by externalizing experience into skills or memory without changing model weights. | Memento-Skills frames reusable structured markdown/code skills as evolving external memory; Reflexion stores verbal feedback in episodic memory; ExpeL extracts knowledge from experience. | `skill-miner`, archive/session mining, audit evidence. |
| A growing skill library needs retrieval/routing quality, not just more instructions. | Memento-Skills uses a skill router; RAG-MCP shows retrieval can reduce prompt bloat and improve tool selection accuracy; tool-description quality work studies how descriptions affect tool use. | `skill-personalizer` trigger fit, undertrigger checks, broad-cluster vs workflow-candidate split. |
| Long instructions should keep critical trigger and safety details near the front. | Lost in the Middle shows long-context models use information best near the beginning or end, with degradation for middle-position evidence. | Frontmatter/description checks, concise `SKILL.md`, references for details. |
| Learned skills need execution feedback and verification before becoming durable. | Voyager stores executable skills after iterative prompting, environment feedback, execution errors, and self-verification. | `skill-miner` candidate validation prompts, `skill-personalizer` workflow completion checks. |
| Public skill distribution requires provenance and safety review. | Agent Skills survey highlights security concerns in community skills and argues for lifecycle governance. | `skill-generalizer` redaction, platform compatibility, publication rubric. |

## Related Projects And Ecosystem

| Project / Ecosystem | Why It Matters |
| --- | --- |
| OpenAI Codex Agent Skills | Official Codex support for skills, bundled scripts/references/assets, and plugin packaging. |
| Claude Code Skills | Official personal/project/plugin skill scopes and description-driven loading. |
| Cursor Agent Skills | Native Agent Skills support alongside rules and commands. |
| OpenCode Agent Skills | Native `skill` tool and repo/home skill discovery. |
| Google / Gemini Agent Skills | Google official skill repository and Gemini CLI / Antigravity-oriented skill installation. |
| Awesome Agent Skills / Antigravity Awesome Skills | Community evidence that cross-agent `SKILL.md` packages are becoming a real distribution format. |
| CodeAlive Skills / GitHub Agent Skill projects | Examples of skill packages paired with setup, references, scripts, and cross-agent install instructions. |

## Implications For This Project

1. **Keep lifecycle stages separate.** Research on externalized memory and skill libraries supports generation from experience, but distribution and personalization have different safety and portability constraints.
2. **Audit routing, not just content.** A skill can be well-written but useless if it undertriggers; trigger fit, overtrigger, and conflict checks deserve their own audit surface.
3. **Make mining configurable.** Workflow discovery should use editable pattern definitions because user histories and agent log formats differ.
4. **Treat summaries as hints.** Rollout summaries and compressed memories are valuable for recall, but raw transcripts or concrete examples should confirm candidates before creating a new skill.
5. **Use scripts for deterministic scanning.** Repeated parsing, counting, and sanitization should live in scripts rather than being rewritten ad hoc by agents.
6. **Package for the target platform honestly.** Cross-agent `SKILL.md` portability is real, but UI metadata, permissions, global paths, and plugin packaging differ by agent.

## Papers And References

- Memento-Skills: Let Agents Design Agents, arXiv:2603.18743.
- Agent Skills for Large Language Models: Architecture, Acquisition, Security, and the Path Forward, arXiv:2602.12430.
- Voyager: An Open-Ended Embodied Agent with Large Language Models, arXiv:2305.16291.
- Reflexion: Language Agents with Verbal Reinforcement Learning, NeurIPS 2023 / arXiv:2303.11366.
- ExpeL: LLM Agents Are Experiential Learners, arXiv:2308.10144.
- Lost in the Middle: How Language Models Use Long Contexts, TACL 2024 / arXiv:2307.03172.
- RAG-MCP: Mitigating Prompt Bloat in LLM Tool Selection via Retrieval-Augmented Generation, arXiv:2505.03275.
- Model Context Protocol Tool Descriptions Are Smelly, arXiv:2602.14878.
