# 决策：跨 Profile Skill 共享策略

## 背景

4 个助手会用到一些通用工具型 skill（例如 `xlsx`、`docx`、`pdf`、`web_extract` 相关），如果每个 profile 各自维护一份，以后改一处要同步 4 份，容易产生版本漂移。

## 已确认的平台事实

查阅官方文档（profiles 页面）确认：

- Profile 之间**没有原生的"共享"或"父子继承"机制**。
- `hermes profile create --clone` / `--clone-all` 只在创建时刻做一次性复制，之后两边各自独立演化，不会持续同步。
- 换句话说，Hermes 官方不提供"公共 skill 库"概念，这件事需要自己用文件系统手段维护。

## 结论：默认 profile 当主库，用 symlink 链接通用 skill

- 把通用、改动频率低的工具型 skill 放在默认 profile 的 `~/.hermes/skills/` 下作为"主库"。
- 对需要用到这些 skill 的子 profile（`life`/`study`/`finance`/`work`），在其 `~/.hermes/profiles/<name>/skills/` 目录下为对应 skill 建符号链接（symlink）指向主库路径。
- 改一处主库文件，所有链接的 profile 同步生效，没有漂移问题。

### 哪些 skill 适合 symlink（通用、低改动频率）

- `xlsx`、`docx`、`pdf`（文档/表格处理）
- `web_extract` / 搜索类工具 skill（如果有独立 skill 封装）
- 其他纯工具型、和角色无关的 skill

### 哪些 skill 不适合 symlink（角色专属、各自私有）

- 装修/甲醛检测相关知识 → 归 `life`
- 理财策略模板、监控策略 → 归 `finance`
- 学习方法论、知识整理模板 → 归 `study`
- 具体公司的办公模板/格式规范 → 归 `work`

## 风险提示

symlink 是非官方支持的手法。如果 Hermes 未来改变 profile 目录扫描逻辑（比如校验目录内容防篡改、或要求目录内文件必须是普通文件），可能导致 symlink 的 skill 不被识别或触发安全校验失败。这个风险目前判断可以接受，但实施后需要验证 symlink 的 skill 确实能被目标 profile 正常加载。

## 待办

- [ ] 实施阶段先验证 symlink 的 skill 能否被 Hermes 正常识别加载（关键验证点，先做这个再铺开）
- [ ] 确定具体要 symlink 的 skill 清单
- [ ] 为每个子 profile 建立对应的 symlink
