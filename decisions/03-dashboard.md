# 决策：看板方案

## 背景

需要一个地方能看到 4 个助手（以及未来可能的子任务流水线）的运行状态和进度，而不是逐个切 profile 去问。

## 结论：先做 (a) 粗粒度总览，(b) 细粒度卡片看板留到流水线升级时再加

### (a) 粗粒度总览（当前要做的）

- 展示 4 个 profile 各自的忙碌状态/最近动作（例如"生活助手刚结束一个任务" / "办公助手正在写文档"）。
- 实现方式：Hermes 桌面插件（`desktop-plugins`），用 `host.request`/`host.onEvent` 轮询/订阅状态，渲染成 4 张卡片。
- 技术要点（来自官方 desktop-plugin-sdk 参考）：
  - 插件落地在 `$HERMES_HOME/desktop-plugins/<id>/plugin.js`，纯 ESM，热重载，不需要改应用本身。
  - 可读状态包括 `host.state.busyBySession`（跨 session 的忙碌 map）、`focusedUsage` 等；**跨 profile 聚合是否可行需要在实施阶段验证**，当前只确认了单 session/单 profile 粒度的状态原子，没有确认是否有现成的"多 profile 状态总览" RPC。
  - UI 用官方组件库（`Button`、`Badge`、`StatusDot` 等），不要硬编码颜色，用主题变量。
- 待验证风险：如果 Hermes 没有现成的"跨 profile 状态聚合" RPC，可能需要插件自己对每个 profile 的 gateway 分别发请求，或者退化为"手动刷新汇总"而非实时订阅。实施时第一步就应验证这一点，再决定具体实现深度。

### (b) 细粒度子任务卡片（暂不做）

- 对应 `02-pipelines.md` 里方式 2（kanban 工作队列）的可视化层。
- 展示单个助手内部任务流水线的卡片状态（例如"论文 A：收集 ✓ / 消化中 / 整理待办"）。
- 升级时机：等学习助手真的需要 kanban 流水线、且确认一次性 delegate_task 不够用时再做。

## 待办

- [ ] 实施阶段先验证"跨 profile 状态聚合"是否有现成 RPC/API
- [ ] 搭建最小可用的 4 卡片总览插件
- [ ] 用户体验验证：卡片信息是否够用，要不要加最近一条消息摘要
