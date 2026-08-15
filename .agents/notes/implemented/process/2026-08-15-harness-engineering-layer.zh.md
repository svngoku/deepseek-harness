# Agent Note: 在既有文档标准之上叠加 harness 工程脚手架层

Status: implemented

[English](2026-08-15-harness-engineering-layer.md) | 中文

## Problem

仓库缺少 init-repo 假定每个会话起步即有的脚手架：没有执行计划生命周期（没有 `docs/exec-plans/`，也没有计划索引），没有产品方向、安全姿态、可靠性、质量跟踪的概念参考，没有 `.env.example`，也没有 `.opencode/` 本地配置。与此同时，这些内容的相邻事实大多已有强归属：分层文档标准带字数预算、双语配对、机制化门禁、CI，以及根 AGENTS.md 地图。天真地补齐缺口会为既有分层已拥有的事实制造第二归属——在小写 `architecture.md` 旁边立一个大写 `ARCHITECTURE.md`、手抄目录、在普遍配对的语料里塞仅英文文档。

## Decision

harness 工程层以标准叶层中的概念参考落地，而不是平行的常备文档：`docs/PRODUCT_SENSE.md`、`docs/SECURITY.md`、`docs/RELIABILITY.md`、`docs/QUALITY_SCORE.md` 和 `docs/PLANS.md` 都是普通双语配对，把触及的每个事实链接到其归属分层，只把方向、不变量和跟踪表留在本页。质量评分由具名门禁推导出等级，而非主观印象；可靠性一页直言开发者预览阶段不存在服务 SLO。

执行计划是 `docs/exec-plans/active/` 与 `docs/exec-plans/completed/` 加一份 `_template.md` 之下的持久工作产物：计划记录工作，Agent Note 保留决策，PLANS.md 索引活跃集合。整个 `docs/exec-plans/` 子树排除在翻译配对之外，视为高频变更的规划残留：每份计划在数天内即被记录、取代、移动，评审过的中文翻译在合并时就已过时。`docs/PLANS.md` 本身——持久的索引与生命周期定义——保持配对。排除项记录在 `scripts/translation-pairing.manifest.json`，并列入 `docs/i18n/README.md` 的排除清单，维持 manifest「仅显式排除」的约定。

层内每张图都是归属文档内的 Mermaid——掌舵循环、信任分区、回滚时序、评分循环、计划生命周期——由 `verify-mermaid` 门禁解析，并在每个双语配对中字节一致（配对结构签名已强制这一点）。`.env.example` 记录真实 API 测试读取的两个变量（`DEEPSEEK_API_KEY`、可选 `DEEPSEEK_BASE_URL`），并指向 docs/testing.md 的密钥策略。`.opencode/AGENTS.md` 与 `.opencode/config.json` 完全服从根地图，自身不声明任何覆盖。

## Alternatives considered

**全盘采用 init-repo 的常备文档命名（docs 根下 `ARCHITECTURE.md`、`DESIGN.md`）。** 仓库的分层体系已拥有这些职责（`architecture.md`、`development.md`、subsystems、cookbook），大小写不敏感的文件系统会与既有小写文件冲突，doc-budgets manifest 也会多出无预算的重复文档。把该层嵌入既有分层，保持一事一归属。

**让执行计划也做双语配对。** 计划天天变更，是工作状态而非发布参考；配对约定的存在是为了让评审过的内容跨语言一致。翻译它们只会成倍增加工作量而没有读者，因此子树被排除，持久索引保持配对。

**把概念参考放进新的 `docs/concepts/` 子目录。** 五份文档与既有顶层参考（`testing.md`、`defensive-patterns.md`）同级，子目录会把它们从所属的导航面中割离。

**既然仓库已有 AGENTS.md 就完全跳过 `.opencode/`。** 该目录是第三方工具读取的本地配置面；两行转发指针没有成本，还能防止某个 agent 或工具凭空造出与根地图脱节的覆盖。

## Consequences

仓库获得了 init-repo 的表面积——概念参考、计划生命周期、环境模板、本地配置——没有第二归属，也没有新门禁面：既有 `doc-sync` 门禁（mermaid、wrap、链接、配对、预算、Note 格式）原样覆盖每个新文件。未来的计划成本是一份 `exec-plans/active/` 下的英文文档加 PLANS.md 的一行表格。配对 manifest 首次出现目录前缀排除，排除清单必须继续区分工作状态（排除）与持久参考（配对）；这条线现在写进了 i18n 约定。质量评分的等级是对具名门禁的断言：门禁覆盖变化时，等级行必须在同一变更中跟进，否则表格说谎。
