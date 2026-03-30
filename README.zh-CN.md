# Skill Optimizer

[English](./README.md)

诊断和优化你的 [Agent Skills](https://agentskills.io)（SKILL.md 文件）— 基于**真实 session 数据 + 学术研究支撑的静态分析**，输出 P0/P1/P2 优先级修复报告。

支持 **Claude Code**、**Codex** 以及所有兼容 Agent Skills 开放标准的 agent。自动检测平台并扫描对应路径。

大多数 skill 审计工具只做 SKILL.md 的静态检查。这个工具还会挖掘你的真实 session 记录，量化触发率、用户满意度、workflow 完成率和漏触发缺口，最终为每个 skill 打出 5 分制综合评分。

它也支持一种 **source repository 审计模式**：当一个公开 skill 仓库还在积累真实使用证据时，可以退回到 validator、example prompts、validation log、CI 和维护者 session 这些次级证据，同时明确把触发率、用户反应之类缺少真实路由样本的维度标成低置信度或 `N/A`。

## 功能

**6 个评分维度**（加权计入综合分）：

| 维度 | 检测内容 |
|------|---------|
| **触发率** | skill 实际被调用的频率 vs. 应该被调用的频率 |
| **用户反应** | 用户在 skill 触发后是接受、纠正还是拒绝？ |
| **Workflow 完成率** | skill 定义的步骤执行到了第几步？ |
| **静态质量** | 14 项检查：YAML 安全、CSO 合规、信息位置、字数等 |
| **漏触发** | 错失机会 — 用户需要但 skill 未被调用 |
| **Token 经济性** | 性价比和渐进式加载分层合规性 |

**3 个定性维度**（报告但不计分）：

| 维度 | 检测内容 |
|------|---------|
| **误触发** | 假阳性 — skill 触发了但用户不需要 |
| **跨 Skill 冲突** | 触发关键词重叠和 skill 间矛盾指导 |
| **环境一致性** | 文件路径失效、CLI 工具缺失、目录不存在 |

## 审计模式

### 已安装 Skill 模式

当目标 skill 已经装在 `~/.claude/skills/`、`~/.codex/skills/` 或 `~/.agents/skills/` 下时，优先使用这个模式。它的置信度最高，因为可以直接从 session transcript 里寻找真实调用证据。

### Source Repository 模式

当你审计的是一个准备公开、还没积累足够 live routing 数据的 skill 仓库时，使用这个模式。优化器仍然会跑完整 8 维，但会把 repo validator、example prompts、validation log、review checklist 和 CI 当成 fallback evidence，而不会假装它们等同于真实路由遥测。

## 什么时候必须补 Routing Eval

routing-eval / transcript 证据并不是对每个仓库都同等紧急。

- 当仓库声称这些 skills 已经 routing-proven、production-ready，或者已经在真实 agent 使用里验证过时，这就是 `P0`。
- 当仓库明确把自己定位成 docs-first、draft 或 beta，而且已经诚实地区分“已证明”和“待验证”时，这更适合作为 `P1` 或下一阶段证据工作。
- 当审计目标只是做静态清理、仓库本身也没有做任何路由成熟度承诺时，这项工作可以先不排在最前面。

## 安装

复制下面的指令，直接粘贴到你的 agent 对话中即可自动安装：

### Claude Code

```
Install the skill-optimizer skill from https://github.com/hqhq1025/skill-optimizer
```

### Codex

```
Install the skill-optimizer skill from https://github.com/hqhq1025/skill-optimizer into ~/.codex/skills/
```

### 其他 Agent（Cursor、OpenCode、Gemini CLI 等）

```
Install the skill-optimizer skill from https://github.com/hqhq1025/skill-optimizer into ~/.agents/skills/
```

<details>
<summary>手动安装</summary>

```bash
# Claude Code
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
cp -r /tmp/skill-optimizer/skills/skill-optimizer ~/.claude/skills/
rm -rf /tmp/skill-optimizer

# Codex
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
cp -r /tmp/skill-optimizer/skills/skill-optimizer ~/.codex/skills/
rm -rf /tmp/skill-optimizer

# 共享路径（任意 agent）
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
cp -r /tmp/skill-optimizer/skills/skill-optimizer ~/.agents/skills/
rm -rf /tmp/skill-optimizer
```

```powershell
# Windows PowerShell 示例（Codex）
$target = Join-Path $env:TEMP 'skill-optimizer'
git clone https://github.com/hqhq1025/skill-optimizer.git $target
New-Item -ItemType Directory -Force -Path "$HOME\\.codex\\skills" | Out-Null
Copy-Item -Recurse -Force "$target\\skills\\skill-optimizer" "$HOME\\.codex\\skills\\"
Remove-Item -Recurse -Force $target
```

</details>

## 使用

```
/optimize-skill              # 扫描所有 skills
/optimize-skill my-skill     # 单个 skill
/optimize-skill skill-a skill-b  # 多个 skills
```

生成的诊断报告包含：
- **总览表** — 所有 skill 一览，含评分
- **P0 修复** — 必须解决的阻断性问题
- **P1 改进** — 体验提升建议
- **P2 优化** — 可选微调
- **逐 Skill 诊断** — 每个 skill 的全部 8 个维度详情

## 多平台 Session 分析

自动检测可用平台并扫描所有 session 数据：

| 平台 | Skills 路径 | Session 数据路径 |
|------|------------|-----------------|
| Claude Code | `~/.claude/skills/` | `~/.claude/projects/**/*.jsonl` |
| Codex | `~/.codex/skills/` | `~/.codex/sessions/**/*.jsonl` |
| 共享 | `~/.agents/skills/` | — |

对于 Codex，`base_instructions` 里出现 skill 被加载，并不等于 skill 真的被调用。优化器会继续寻找 workflow marker 或明确的 prompt/result 证据，再把它计作一次 invocation。

如果审计的是 source repository，而不是已经安装到本地目录的 skill，优化器还可以读取：

- 仓库自带 validator
- `references/` 文件和 `agents/openai.yaml`
- example prompts 与 validation log
- CI workflow 与 forward-test 记录

## 研究背景

分析维度基于同行评审的学术研究：

| 研究 | 关键发现 | 应用于 |
|------|---------|--------|
| [Memento-Skills](https://arxiv.org/abs/2603.18743) (2026) | Skills 作为结构化文件需要准确的路由；未被路由的 skill 无法通过读写学习循环自我改进 | 漏触发检测、复合风险评估 |
| [MCP Description Quality](https://arxiv.org/abs/2602.18914) (2026) | 好描述达到 72% 工具选择率 vs. 20% 随机基线（3.6 倍提升） | Description 质量检查、基于证据的改写建议 |
| [Lost in the Middle](https://arxiv.org/abs/2307.03172) (Liu et al., TACL 2024) | LLM 注意力呈 U 型曲线 — 中间内容被忽略 | 关键信息位置检查 |
| [Prompt Format Impact](https://arxiv.org/abs/2411.10541) (He et al., 2024) | 仅改格式就能带来 9-40% 的性能差异 | 静态质量分析 |
| [IFEval](https://arxiv.org/abs/2311.07911) (Zhou et al., 2023) | LLM 难以处理多约束提示 | 触发条件数量检查 |
| [Meincke et al.](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5357179) (2025) | 说服性指令在不同模型间效果不一致 | MUST/NEVER 密度指导 |

## 工作原理

```
识别目标 skills（扫描 ~/.claude/skills/、~/.codex/skills/、~/.agents/skills/）
       ↓
采集 session 数据（自动检测平台，扫描 JSONL 记录）
       ↓
运行 8 个分析维度（6 个评分 + 3 个定性）
       ↓
计算综合评分（6 个评分维度的加权平均）
       ↓
输出 P0/P1/P2 优先级修复报告
```

当 session 数据不足时，优化器仍然会坚持跑完整 8 个维度，并把证据不足的指标明确标成 `N/A`，而不是伪造分数。

## 示例仓库审计

示例：一个 docs-first 的小程序 skill 仓库里有 4 个 public skills，validator 和 CI 都通过，也有 example prompts、validation log 和 forward-test 记录，但历史 session 主要还是维护者在建设仓库，而不是已经安装好的 skills 在真实对话里被稳定路由。

这时正确的审计结论应该是：

- 静态质量和渐进式加载可以正常打分
- workflow completion 可以把 validation log 当作中等置信度证据
- 触发率、用户反应、漏触发要标成低置信度或 `N/A`
- 缺少 routing transcript 应该被列成“下一阶段最重要的 `P1`”，而不是机械地判成 `P0`；除非仓库已经对外宣称自己有真实路由证明

## 示例输出

```markdown
# Skill Optimization Report
**Date**: 2026-03-30
**Scope**: `miniprogram_skills` 里的全部 public skills
**Evidence**: validator 通过、CI、validation log、8 条维护者 session
**Confidence**: static=high, workflow=medium, routing=low
**Release stage**: docs-first public beta

## Overview
| Skill | Trigger | Reaction | Completion | Static | Undertrigger | Token | Score |
|-------|---------|----------|------------|--------|--------------|-------|-------|
| miniapp-devtools-cli-repair | N/A | N/A | strong | strong | N/A | strong | 4/5 |
| miniapp-devtools-gui-check | N/A | N/A | strong | strong | N/A | strong | 4/5 |

## P0 Fixes
当前证据集下无 P0。

## P1 Improvements
1. 给每个 public skill 补 1 条 installed-skill transcript 或可重放 routing eval。
2. 给相邻 skill 边界补负路径验证。

## Milestone Fit
- current-milestone blockers: 除仓库已声明的 beta 限制外，无新增阻塞
- next-milestone evidence work: transcript-backed routing proof

## Per-Skill Diagnostics
### miniapp-devtools-gui-check
#### 4.1 Trigger Rate
N/A — 缺少足够的 live routing evidence
#### 4.3 Workflow Completion
Strong。validation log 证明过一次真实的窄路由宿主机检查并生成报告。
#### 4.6 Cross-Skill Conflicts
Moderate but controlled。主要重叠对象是 miniapp-devtools-cli-repair。
```

这就是 source-repository 模式下期望的输出风格：保留完整 8 维，诚实标注证据等级，不因为缺少 live routing 数据就伪造一个看起来很确定的结论。

**评分维度（加权平均）：**
- 触发率：25%
- 用户反应：20%
- Workflow 完成率：15%
- 静态质量：15%
- 漏触发：15%
- Token 经济性：10%

**定性维度**（误触发、跨 skill 冲突、环境一致性）以示例形式报告，不影响数值评分。

## 兼容性

兼容所有支持 [Agent Skills](https://agentskills.io) 开放标准的 agent：
- Claude Code
- Codex
- Cursor
- OpenCode
- Gemini CLI

## 社区

- [LINUX DO](https://linux.do) — 本项目首发社区

## 许可证

MIT
