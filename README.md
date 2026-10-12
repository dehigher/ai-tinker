# AI Assistants — Hermes 多助手实施方案

基于 Hermes Agent 的 profile 机制，为日常生活打造多个独立角色助手（生活 / 学习 / 理财 / 办公），
并逐步演进出子任务流水线和进度看板。本目录是方案文档和相关配置的长期维护仓库。

## 目录结构

```
.
├── README.md              # 本文件：总体方案与进展索引
├── decisions/             # 方案讨论的结论记录（按主题拆分，一个文件一个决策点）
│   ├── 00-overview.md         # 总体架构与三层设计
│   ├── 01-profiles.md         # 4 个 profile 的划分与模型选型
│   ├── 02-pipelines.md        # 子任务流水线策略（delegate_task vs kanban）
│   ├── 03-dashboard.md        # 看板方案（粗粒度总览 → 细粒度卡片）
│   └── 04-skill-sharing.md    # 跨 profile skill 共享策略（symlink 主库）
├── profiles/              # 每个 profile 的配置草稿/快照（非 ~/.hermes 本体，这里只存可维护的声明式配置）
│   ├── life/
│   ├── study/
│   ├── finance/
│   └── work/
├── dashboard-plugin/       # 桌面看板插件源码（desktop-plugins）
└── CHANGELOG.md           # 实施进度记录，每次落地变更追加一条
```

## 当前状态

方案已讨论完成，尚未开始实施。详见 `decisions/` 下各文件和 `CHANGELOG.md`。

## 快速索引

- 整体设计思路：[decisions/00-overview.md](decisions/00-overview.md)
- 4 个助手配置：[decisions/01-profiles.md](decisions/01-profiles.md)
- 子任务流水线：[decisions/02-pipelines.md](decisions/02-pipelines.md)
- 看板方案：[decisions/03-dashboard.md](decisions/03-dashboard.md)
- Skill 共享策略：[decisions/04-skill-sharing.md](decisions/04-skill-sharing.md)
