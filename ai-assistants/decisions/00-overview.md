# 总体架构：三层设计

## 背景

目标：基于 Hermes Agent 的 profile 机制打造多个角色化的个人助手，并对它们进行统一的进度/状态管理（"看板"）。

讨论中把需求拆成三层，分开决策：

1. **Profile 层**——用 Hermes profile 隔离不同角色助手的配置、memory、skills、session。
2. **流水线层**——单个助手内部，任务太大时如何拆解成多个子 agent 协作。
3. **看板层**——跨助手的状态/进度可视化。

## 结论摘要

| 层 | 方案 | 状态 |
|---|---|---|
| Profile | 4 个独立 profile：生活 / 学习 / 理财 / 办公，各自分化模型配置 | 待实施 |
| 流水线 | 默认用 `delegate_task` 一次性委派子 agent；不预先搭建常驻 kanban 队列，等真实反复需求出现再升级 | 待实施 |
| 看板 | 先做粗粒度总览插件（4 助手忙碌状态），细粒度子任务卡片看板留到流水线层升级到 kanban 时再加 | 待实施 |
| Skill 共享 | 通用工具型 skill 用 symlink 从主库（默认 profile）链接到各子 profile；角色专属 skill 各自私有 | 待实施 |

## 关键事实（来自官方文档确认）

- Profile 是完全独立的 Hermes home 目录（`~/.hermes/profiles/<name>/`），各自拥有 `config.yaml`、`.env`、`SOUL.md`、memories、sessions、skills、cron、state.db。
- Profile 之间没有原生的"共享"或"继承"机制。`hermes profile create --clone` / `--clone-all` 只是创建时刻的一次性复制，之后两边独立演化，不会持续同步。
- 两个 agent 进程绝不能指向同一个 profile——都会自动写 memory，互相读取对方写入的内容，状态会在不受控的情况下复合污染。这是 profile 隔离设计的核心理由之一。
- Subagent（`delegate_task` 产生的）和 profile 是两个不同维度：subagent 是一次性的新对话，不是新的持久配置身份。
- `kanban` toolset 存在，是"多 agent 工作队列"的工具原语（gated to workers），但没有默认可视化 UI，需要配套看板插件渲染。

## 待确认 / 后续决策点

- Profile 划分和模型选型的具体细节 → `01-profiles.md`
- 子任务流水线的触发时机和实现方式 → `02-pipelines.md`
- 看板插件的具体数据来源和渲染方案 → `03-dashboard.md`
- Skill 共享的 symlink 清单和维护方式 → `04-skill-sharing.md`
