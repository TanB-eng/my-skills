# BoT_eng

`BoT_eng` 是一套面向多模块、长期迭代或上下文较复杂项目的自适应、规格优先 MVP 开发方法。

它先让人与 AI 明确产品方向和 MVP，按需用原型澄清交互，确定架构与核心接口，再由 AI 给出有依据的开工判定。用户集中确认基线后，按业务模块串行实现、测试和联调，最终验收核心用户路径。OpenSpec、Superpowers 和 Spec Kit 都是按需工具。

显示名称是 `BoT_eng`，实际 Skill 名称和调用方式为 `bot-eng`、`$bot-eng`。

## 核心理念

```text
先把方向想清楚
-> 按需梳理原型和核心交互
-> 确定技术路线、业务模块及核心接口
-> 对抗式审查与开工判定
-> 集中确认 MVP 基线
-> 串行实现模块，随实现补测试并真实联调
-> 跑通核心用户路径
-> 整体验收，再根据真实体验打磨
```

关键原则：

- 规格确认前不急着施工。
- 顶层按业务能力拆分，不按前端、后端、数据库分层拆分。
- 默认使用能可靠交付的最轻流程。
- 默认一次只施工一个业务模块，不让多个模块并行写共享代码；独立研究或审查只有在不改变共享实现状态时才可并行。
- 代码和测试一起完成。
- 只在产品行为、架构、公开接口或验收标准变化时同步权威文档。
- 先证明 MVP 核心价值，再投入 UI 打磨、少见边界和未来规模设计。

## 三种模式

### MVP mode（默认）

适合从零构建 MVP、简单或中等复杂模块、低风险迭代。

- 建立产品和架构基线。
- 按模块串行实现并运行针对性测试。
- 模块文档和契约按需创建。
- 不强制 OpenSpec、Superpowers、逐任务审批或全量文档。
- 新项目把基线和开工判定一起展示，集中确认一次再施工；已有项目核验现有基线，不重复审批已确定的范围。
- 模块切换、新对话、任务摘要和低风险实现选择都不是审批关口。

基线批准或验证后，AI 默认自动串行推进低风险模块。每个模块开始时只需简短说明“目标、范围、验证方式”，随后直接实现，不等待用户再次确认。

首个可运行纵向切片等关键节点应展示实际结果，让用户有机会尽早纠偏，但默认不停工等待确认；只有用户主动要求检查点时才暂停。

### Controlled mode

适合复杂模块、重要或尚不稳定的跨模块接口、数据迁移、兼容性要求或返工成本较高的变更。

仅仅存在一个小而稳定的跨模块接口，不足以触发升级；安全边界、不可逆操作、迁移、兼容性或高返工成本才是更强的升级信号。

- 为当前模块补充必要的模块说明和公开契约。
- 使用 OpenSpec 管理 proposal、spec、design 和 tasks。
- OpenSpec 生成任务后暂停，等待用户确认，再进入代码实现。

### Strict mode

仅在用户明确要求，或项目具有合规、安全、兼容、不可逆操作等高风险条件时使用。

- 使用更详细的规格、审批、分阶段交付检查点和回归验证。
- 启用前应说明额外时间与上下文成本。

AI 应说明当前选择的模式和理由。只有存在明确风险或用户主动要求时，才能从 MVP mode 升级到更重流程。

对于已有项目的小功能或 Bug，不重新跑完整全局设计；先读取现有规格和代码，在原架构内做最小必要修改。

## 默认项目文档

典型的中大型项目可以使用：

```text
docs/
  PRD.md
  architecture.md
  tech-stack.md
  progress.md
  decisions.md
```

| 文件 | 作用 |
| --- | --- |
| `PRD.md` | 产品问题、用户、核心路径、MVP、非目标和验收标准 |
| `architecture.md` | 架构骨架、模块关系、依赖、数据归属和跨模块规则 |
| `tech-stack.md` | 技术选型、限制和理由 |
| `progress.md` | 当前状态、完成内容、测试、阻塞点和下一步 |
| `decisions.md` | 以后容易被反复讨论的重要决定和原因 |

不要在项目一开始机械创建空文档。只有文档确实承载有用信息时才创建。

小型 MVP 可以只使用 `docs/spec.md` 和简短的 `docs/progress.md`，把产品、架构和技术选择放在同一个规格文件中。内容增长到难以维护时再拆分。

使用合并的 `docs/spec.md` 时，后续产品、架构或技术事实变化，应更新该文件中对应章节，不要因为缺少拆分文件而漏记。

以下文件按需创建：

```text
docs/modules/<module>.md
docs/design.md
docs/contracts/<contract>.md
openspec/changes/<change-id>/
```

- 模块说明用于复杂模块的边界、验收标准和状态交接。
- 交互说明优先写进现有产品规格，复杂时再独立为 `docs/design.md`。
- 契约覆盖核心前后端对接和有意义的跨模块接口；即使前后端属于同一业务模块，也要约定必要的数据交换。
- 小项目可以在 `docs/spec.md` 中说明接口，或使用 `docs/contracts/api.md`。已有正式接口定义时直接复用，不重复维护字段清单。没有网络接口的项目不强加后端。
- OpenSpec 只用于值得采用更严格变更流程的模块。

## 使用流程

### 1. 共同确定项目基线

启动提示词：

```text
Use $bot-eng in MVP mode. Do not write code yet.
Help me clarify the product goal, users, core journey, MVP, non-goals,
architecture options, technology route, modules, risks, and acceptance criteria.
Use a simple interaction sketch only where needed, and agree on core interfaces.
Write only useful baseline docs, assess readiness with evidence, and present
them together for my approval before product implementation.
```

AI 应与你反复讨论并梳理：

- 产品解决的问题和目标用户
- 核心用户路径
- MVP 必须具备的能力
- 明确暂不实现的内容
- 架构方案和技术路线
- 业务模块、依赖和实现顺序
- 风险、假设和验收标准

AI 应区分你的明确要求、已有代码或实验提供的证据、尚未验证的假设。优先调查能自行查证的问题，只追问会影响产品范围、核心交互、架构、风险或验收的选择；不为了凑问题数量而追问，也不因达到问题数量上限就忽略阻碍。

交互尚不清楚时，先展示简洁原型或页面草图，覆盖关键操作以及相关的加载、空数据和错误状态。已有明确界面时可复用；纯后端或命令行项目用输入输出示例代替，不必生成 UI。视觉精修可以后置。

结合交互与业务规则，在基线阶段约定 MVP 核心接口：操作及归属、请求与响应字段、必要的权限、成功和失败示例，以及实际需要的异步、分页或重复提交规则。架构引用权威接口定义，不按每个按钮机械生成一个接口；未来接口和内部实现细节可以延后。

提交你确认前，AI 应检查需求、交互、接口和架构是否一致，主动寻找核心路径断裂、隐藏假设、职责重叠、循环依赖和过度设计。然后给出开工判定：

| 判定 | 条件及下一步 |
| --- | --- |
| 可开工 | 核心路径、范围、能力与数据归属、首个切片和验收方式明确，没有已知阻碍；展示基线供你确认 |
| 带假设可开工 | 剩余假设可控且可逆；说明内容、验证方法、在依赖它的工作开始前何时验证，以及失败后的处理方式，随基线一起确认 |
| 暂不能开工 | 核心结果不可验收、方案矛盾、归属不清或关键可行性风险尚未解决；指出具体阻碍和最小澄清或已授权实验 |

判定依据要引用相关规格、代码、资料或实际实验；没有证据就明确标注，不能凭文档齐全或一个分数宣布可靠。首个页面容易实现，也不能掩盖会使整个 MVP 失败的技术未知。

AI 在 `docs/progress.md` 保留简短结论、依据、首个切片及验证方式、未决假设或阻碍、下一步。新项目和重大基线变化时执行；复杂变更可以在既有计划中局部复用。普通模块切换或换对话不重复检查和审批。

开工判定只代表足以开始验证 MVP。只有你明确批准展示过的基线后，才进入正式实现，不把 AI 自审通过当作你的批准。

你调整方向后，AI 应先同步相关规格，并说明哪些模块、测试或验收假设受到影响，再继续讨论或施工。

基线确认前可做只读调查和静态交互草图；可运行原型或技术实验需要已有的明确范围授权，隔离实施并标明模拟部分。实验授权不等于批准整套 MVP。正式基线确认后可记录：

```text
Status: MVP baseline approved
Version: 0.1
```

### 2. 按业务能力拆模块

推荐：

```text
user-auth
file-management
document-parse
knowledge-retrieval
ai-chat
admin
```

不建议把顶层模块直接拆成：

```text
frontend
backend
database
components
api
```

一个合适的模块应具有单一业务目标、明确职责、清晰数据归属、小型公开接口和可独立验证的结果。

每类持久化数据或有状态能力应有一个明确归属模块。其他模块通过公开接口写入，不直接依赖其私有表或内部 Service。

项目初始化、部署、可观测性、共享设计系统等横切技术基础可以作为明确的前置工作或共享基础设施，但不冒充用户价值模块；只在当前 MVP 确实需要时建立，并保持单一归属和小型公开边界。

只有当前模块的测试、进度记录、公开输出和必要交接事实都已更新，才能进入下一个模块。如果后续模块必须读取前一个模块的内部实现，应先修复边界或契约。

### 3. 串行实现 MVP

模块提示词：

```text
Use $bot-eng. Continue the next business module in MVP mode.
Read the current project docs and repository state, check the module boundary,
briefly state the goal, scope, and verification method, then implement the
smallest useful slice without waiting for approval. Add focused tests, run
relevant checks, and update progress. Pause only for required authorization,
a high-impact unresolved decision, or a product or architecture baseline change.
```

每个模块执行：

```text
读取当前规格和代码
-> 核对模块目标、边界、依赖和到期需要验证的假设
-> 判断是否需要升级流程
-> 根据主要不确定性选择实现顺序
-> 实现推动核心路径的最小功能，随实现补测试
-> 接入真实组件，运行针对性测试与相关集成、回归检查
-> 修复失败
-> 更新进度和发生变化的权威文档
-> 进入下一模块
```

“一个模块一个对话”是推荐的上下文组织方式，不是制造额外文档和审批的理由。

对于技术路线明确的界面驱动模块，默认顺序为“前端交互（必要时用明确标注的模拟数据）→ 后端逻辑和数据存储 → 真实联调”，测试随实现补充。前后端共同遵循核心接口约定。

算法、AI 效果或外部服务是主要未知时，先验证关键能力。纯后端或本地工具省略不必要的界面和网络层。每个模块只做到本轮 MVP 所需程度，不先完成所有模块的前端，也不把单个模块所有增强功能做完才进入下一模块。

模块换对话前，AI 只需留下简短交接；新对话从已经批准的基线和当前进度直接继续，不重新讨论已经确定的设计，除非新证据表明原设计存在问题。

简单模块不创建详细任务卡、需求编号、追踪表或文件范围清单。只要在对话中用三项说明即可：

```text
目标：这个模块本轮要产生什么可用结果
范围：本轮包含和不包含什么
验证：AI 将运行什么检查，用户最终看到什么
```

只有缺少这些详细工件会造成明显歧义、协作风险、审计要求或高昂返工时，才补充任务卡、追踪关系或升级到 OpenSpec。

### 4. 按风险升级到 OpenSpec

出现以下情况时，可以切换到 Controlled mode：

- 模块包含多项复杂业务规则
- 需要新增或修改重要、尚不稳定或有兼容性影响的公开契约
- 涉及数据库迁移或兼容性
- 存在多个差异明显的设计方向
- 失败或返工成本较高
- 需要清晰的变更历史

提示词：

```text
Use $bot-eng with OpenSpec Controlled mode for this module.
Explore the design, create the smallest useful OpenSpec plan,
show me the tasks, file scope, risks, and tests,
then wait for my explicit approval before writing implementation code.
```

典型流程：

```text
/opsx:explore       可选：需求或风险仍需探索
/opsx:new           创建变更
/opsx:continue      创建下一个必要工件
/opsx:ff            仅在需求已很清晰时快速生成规划工件
暂停并等待人工确认
/opsx:apply         实现已批准的任务
/opsx:verify        验证实现与规格一致
/opsx:sync          必要时同步稳定规格
/opsx:archive       归档已完成变更
```

OpenSpec 生成 tasks 后，AI 必须先展示任务、文件范围、接口和数据影响、测试计划、风险和未决假设。只有用户明确批准后，才能执行 `/opsx:apply` 或写实现代码。

OpenSpec 不可用时，低风险模块可以继续使用 MVP mode；高风险变更不能静默降级，应改用等价的简洁计划与人工确认，或询问用户如何继续。

### 5. MVP 整体验收

提示词：

```text
Review the MVP against the approved PRD and core user journey.
Run the relevant tests and build checks, identify missing behavior and known gaps,
and separate must-fix issues from post-MVP polish.
```

MVP 完成应满足：

- 核心用户路径可以端到端运行。
- 已确认的 MVP 验收标准满足。
- 重要失败路径有合理反馈。
- 测试和构建检查提供可复验证据。
- UI 打磨、增强功能和非 MVP 工作被明确列入后续。

AI 完成串行施工和验证后，应集中展示核心用户路径、测试证据和已知缺口，由用户进行一次最终 MVP 验收；无需在中间逐模块重复验收。

## 文档同步规则

文档描述当前事实，不记录每次内部改动。

必须同步：

- 产品范围、用户或验收标准变化
- 模块边界、依赖、数据归属或系统结构变化
- 技术路线发生重要变化
- 跨模块公开契约变化
- 核心交互或前后端接口变化；同步权威说明、受影响的调用方、模拟数据和相关测试
- 项目阶段、测试状态或下一步变化
- 形成以后不应反复推翻的重要决定

通常无需同步全局文档：

- 私有函数改名
- 普通内部重构
- CSS 微调
- 不改变外部行为的文件整理
- 普通测试实现细节

## 测试策略

测试深度与风险匹配：

- 本地行为使用单元或功能测试。
- 重要模块边界使用集成测试。
- 每个关键 MVP 能力保留少量高价值冒烟路径。
- MVP 完成前验证完整核心用户路径、可用测试套件和构建。

如果项目尚无自动测试框架，先采用当前能提供的最小可靠验证并说明限制。只有收益值得时才引入测试框架，不要让一个小型 MVP 模块变成测试工具搭建项目。

无需在每个微小修改后强制运行全量回归，但不能在没有验证证据时宣称模块或 MVP 完成。

核心路径依赖真实后端、持久化或外部服务时，必须验证相应连接和必要的数据留存；模拟数据演示不能证明整条路径完成。真实依赖不可用时，明确记录为部分完成并标注未验证路径。

## 可选工具

- Superpowers：需求探索、TDD、调试或高风险完成验证。
- OpenSpec：复杂模块和高影响变更。
- Spec Kit：用户明确需要完整项目级 Spec-Driven Development 时。

不要因为工具已安装就自动调用，也不要为同一个变化同时运行两套重叠规格流程。

## 下载与安装

### 克隆仓库

```powershell
git clone https://github.com/TanB-eng/my-skills.git
```

Windows PowerShell：

```powershell
$skillRoot = if ($env:CODEX_HOME) {
  Join-Path $env:CODEX_HOME "skills"
} else {
  Join-Path $HOME ".codex\skills"
}

$target = Join-Path $skillRoot "bot-eng"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item ".\my-skills\bot-eng\*" $target -Recurse -Force
```

macOS / Linux：

```bash
git clone https://github.com/TanB-eng/my-skills.git
mkdir -p "${CODEX_HOME:-$HOME/.codex}/skills"
cp -R my-skills/bot-eng "${CODEX_HOME:-$HOME/.codex}/skills/bot-eng"
```

也可以在 GitHub 点击 `Code` -> `Download ZIP`，解压后复制完整的 `bot-eng` 文件夹。

保留完整结构：

```text
bot-eng/
  SKILL.md
  agents/
    openai.yaml
```

安装或更新后重新打开 Codex 会话。

## 更新本地 Skill

```powershell
Set-Location .\my-skills
git pull

$skillRoot = if ($env:CODEX_HOME) {
  Join-Path $env:CODEX_HOME "skills"
} else {
  Join-Path $HOME ".codex\skills"
}

$target = Join-Path $skillRoot "bot-eng"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item ".\bot-eng\*" $target -Recurse -Force
```

Skill 源文件：[bot-eng/SKILL.md](bot-eng/SKILL.md)

## License

本仓库当前未声明开源许可证。公开使用、修改或再分发前，请由仓库所有者补充合适的 `LICENSE` 文件。
