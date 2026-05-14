# Skill Optimizer

[English](./README.md)

这是三个用于把 coding-agent 工作沉淀成更好 `SKILL.md` 的 Agent Skills：

- **skill-miner**：从 coding-agent 历史、archive、memory 和重复工作里挖矿，找出有证据支撑的候选 skills。
- **skill-personalizer**：审计并把新建、下载、fork 或社区里的 skill 调成适合某个用户真实工具、习惯、目录和 session 历史的个人版。
- **skill-generalizer**：把本地、私有、个人化的 skill 提炼成可发布到 GitHub、marketplace、团队或公开社区的通用 skill。

拆成三个 skill 是故意的。发现、个人化、公开发布是不同工作：

| 目标 | Skill | 优化方向 |
| --- | --- | --- |
| 挖掘重复工作流 | `skill-miner` | 扫描真实 agent 使用史，聚类重复工作流，生成带证据的候选 skill。 |
| 向内贴合 | `skill-personalizer` | 继承旧版 optimizer 的审计检查，再加入本地默认路径、用户常用说法、偏好工具、验证习惯和工作流捷径。 |
| 向外发布 | `skill-generalizer` | 去掉私有上下文，泛化例子，让安装说明和 README 说法可迁移。 |

相关生态项目和论文依据见 [Research Background](./docs/research-background.md)，包括 session mining、skill library、触发审计、渐进式披露和生命周期治理。

## 安装

把下面的指令复制到你的 agent 对话里：

### Claude Code

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer
```

### Codex

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer into ~/.codex/skills/
```

### 其他兼容 Agent Skills 的 agent

```text
Install the skills from https://github.com/hqhq1025/skill-optimizer into ~/.agents/skills/
```

手动安装：

```bash
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
mkdir -p ~/.agents/skills
cp -r /tmp/skill-optimizer/skills/skill-miner ~/.agents/skills/
cp -r /tmp/skill-optimizer/skills/skill-personalizer ~/.agents/skills/
cp -r /tmp/skill-optimizer/skills/skill-generalizer ~/.agents/skills/
rm -rf /tmp/skill-optimizer
```

如果只装到 Codex：

```bash
git clone https://github.com/hqhq1025/skill-optimizer.git /tmp/skill-optimizer
mkdir -p ~/.codex/skills
cp -r /tmp/skill-optimizer/skills/skill-generalizer ~/.codex/skills/
cp -r /tmp/skill-optimizer/skills/skill-miner ~/.codex/skills/
cp -r /tmp/skill-optimizer/skills/skill-personalizer ~/.codex/skills/
rm -rf /tmp/skill-optimizer
```

如果只装到 Claude Code，把目标目录换成 `~/.claude/skills/`。

## 平台支持

| Agent | 支持形态 | 推荐路径 |
| --- | --- | --- |
| Codex | 原生 Agent Skills，并可选 plugin metadata。 | `~/.codex/skills/` 或 `.agents/skills/` |
| Claude Code | 原生 skills，支持个人、项目和插件作用域。 | `~/.claude/skills/` 或 `.claude/skills/` |
| Cursor | 原生 Agent Skills，同时有 rules/commands；Agent 会发现并选择 skill。 | `.agents/skills/`、`.cursor/skills/` 或全局 skills |
| OpenCode | 原生 `skill` tool，支持 repo/home skill discovery。 | `.agents/skills/`、`.opencode/skills/` 或 `~/.config/opencode/skills/` |
| Gemini CLI / Google agents | Google 已公开说明 Agent Skills 开放格式；`GEMINI.md` 仍是 always-on 项目上下文机制。 | `.agents/skills/` 或 installer 管理的 skills |

公开 repo 最稳的布局是保留 `skills/<name>/SKILL.md`，并提供复制到 `.agents/skills/` 或目标 agent 原生目录的安装说明。

## 用法

直接说清你想要的方向：

```text
扫描我的 coding-agent 使用历史，挖出哪些重复工作流应该变成 skills。
```

```text
审计并调优我安装的 skills，看看哪些漏触发、误触发或者太冗长。
```

```text
把这个本地 skill 提炼成可以发布到 GitHub 的通用 skill。
```

```text
我下载了这个 skill，按我的本地工作流和使用习惯调一下。
```

```text
这个 skill 在我自然说话时不会触发，帮我个人化优化一下。
```

## 三个 Skill 分别做什么

### skill-miner

- coding-agent session 历史、memory summary、repo notes、重复脚本和项目目录
- 高频用户意图、自然缩写、工具链、产物和验证模式
- 哪些候选足够重复、非显然，值得变成 skill
- 候选应该留作个人版、进一步通用化发布，还是跳过
- 内置 `scripts/scan_sessions.py`，可以对 Codex、Claude Code、Gemini/Antigravity task files，以及其他 agent 导出的 transcripts 做确定性的第一轮扫描
- 默认也会纳入 archived Codex sessions 和 rollout summaries；可用参数关掉 archive/summary 数据源

示例：

```bash
python3 skills/skill-miner/scripts/scan_sessions.py --days 30 --limit 300 --min-count 3
python3 skills/skill-miner/scripts/scan_sessions.py --export ~/Downloads/cursor-chat-export.json
python3 skills/skill-miner/scripts/scan_sessions.py --patterns ./my-patterns.json
python3 skills/skill-miner/scripts/scan_sessions.py --no-include-archives --no-include-summaries
```

### skill-generalizer

- 私有路径、host、凭证、账号名、聊天记录引用和内部 repo 事实
- 命令、例子、README 声明和安装说明是否可公开迁移
- frontmatter 是否只描述触发场景，而不是塞工作流
- 公开分发所需的目录结构和包装质量

### skill-personalizer

- 本地安装副本和项目说明
- 用户真实措辞和高频任务模式
- 偏好的 CLI、MCP 工具、路径、alias 和验证命令
- 漏触发、误触发、重复提问等摩擦点
- 继承旧版 optimizer 的审计检查：触发匹配、用户反应、workflow 完成度、静态质量、冲突、环境一致性、token 经济性和 P0/P1/P2 修复

## 兼容性

兼容采用 Agent Skills 目录约定的 agent：

- Claude Code
- Codex
- Cursor
- OpenCode
- Gemini CLI

## 研究背景

本项目参考了 Agent Skills 生态，以及 LLM agent 在外部记忆、skill library、检索/路由和长上下文行为方面的研究。详见 [docs/research-background.md](./docs/research-background.md)。

## 许可证

MIT
