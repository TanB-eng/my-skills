# BoT_eng

`BoT_eng` 是一套面向中大型 AI 辅助软件项目的文档驱动、串行模块开发方法。

它先用全局文档确定产品、架构和技术约束，再按业务能力拆分模块。每个模块使用一个独立对话，并通过 OpenSpec 完成规格、设计、任务、实现、验证和归档。

显示名称是 `BoT_eng`，Skill 文件夹和实际调用名称遵循 Codex 命名规范，使用 `bot-eng` 和 `$bot-eng`。

## 适用场景

- 项目包含多个业务模块，单次对话难以完成。
- 希望降低模块之间的耦合度。
- 希望出现错误时能快速定位到具体模块。
- 希望一个模块使用一个独立对话，减少上下文污染。
- 希望通过文件和 Git 恢复进度，不依赖 AI 的隐藏记忆。
- 希望后续模块不会静默破坏已经完成的模块。

不适合一次性脚本、很小的功能修改或无需长期维护的实验。

## 核心方法

```text
项目想法
-> 澄清需求
-> 建立并验证全局文档
-> 按业务能力拆分模块
-> 一个模块一个新对话
-> 完善模块边界
-> 使用 OpenSpec 完成模块
-> 测试、验证、同步和归档
-> 更新项目记忆
-> 进入下一个模块
```

顶层模块按业务能力拆分，例如：

```text
user-auth
file-upload
document-parse
knowledge-base
ai-chat
admin
deployment
```

不要把前端、后端、数据库、API 直接作为顶层业务模块。每个业务模块内部再覆盖前端、后端、Service、数据库、API 和测试。

## 项目文档结构

使用 `BoT_eng` 的项目建议采用：

```text
docs/
  PRD.md
  architecture.md
  tech-stack.md
  progress.md
  decisions.md

  contracts/
    <public-contract>.md

  modules/
    00-project-setup.md
    01-<business-module>.md
    02-<business-module>.md

openspec/
  specs/
  changes/
```

文件职责：

| 内容 | 权威来源 |
| --- | --- |
| 产品范围、用户、MVP、非目标 | `docs/PRD.md` |
| 系统结构、模块关系、数据所有权 | `docs/architecture.md` |
| 技术约束和技术选型 | `docs/tech-stack.md` |
| 跨模块公开接口 | `docs/contracts/*` |
| 模块边界和最终摘要 | `docs/modules/<module>.md` |
| 当前模块变更过程 | `openspec/changes/<change-id>/` |
| 同步后的稳定行为规格 | `openspec/specs/*` |
| 当前 OpenSpec 任务状态 | OpenSpec tasks |
| 项目里程碑和下一步 | `docs/progress.md` |
| 长期决策及原因 | `docs/decisions.md` |

## 依赖

模块开发阶段需要：

- `bot-eng`：负责全局文档、模块边界、串行顺序和跨对话记忆。
- `openspec`：负责每个模块的 proposal、spec、design、tasks、apply、verify、sync 和 archive。

可选增强：

- Superpowers：用于需求探索、TDD、调试和完成前验证。
- Spec Kit：可用于项目级规格工作，但不要与 OpenSpec 在同一模块内重复维护两套实施流程。

如果项目要求严格遵循本 Skill，而 `$openspec` 或 OpenSpec CLI 不可用，应先完成安装，不要静默跳过模块规格流程。

## 下载与安装

### 方法一：克隆整个仓库

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

安装后重新打开 Codex 会话，让 Skill 被重新发现。

### 方法二：下载 ZIP

1. 打开本仓库。
2. 点击 `Code`。
3. 选择 `Download ZIP`。
4. 解压后，将整个 `bot-eng` 文件夹复制到 Codex 的 `skills` 目录。

不要只复制 `SKILL.md`。应保留完整结构：

```text
bot-eng/
  SKILL.md
  agents/
    openai.yaml
```

## 检查是否安装成功

在新对话中输入：

```text
Use $bot-eng，介绍你的项目开发流程。
```

如果 Codex 能识别 `$bot-eng` 并读取 Skill，说明安装成功。

## 完整使用流程

### 1. 启动项目并生成全局文档

```text
Use $bot-eng。

先不要写代码。探索并澄清我的项目目标、目标用户、MVP、
非目标、核心用户路径、业务模块、数据流、技术风险和验收标准。

然后创建：
docs/PRD.md
docs/architecture.md
docs/tech-stack.md
docs/progress.md
docs/decisions.md
```

### 2. 验证全局文档

```text
Use $bot-eng 验证 docs/PRD.md、docs/architecture.md
和 docs/tech-stack.md。

检查产品功能是否都有模块承接、模块是否有产品依据、
数据是否只有一个所有者、依赖是否单向、是否存在循环依赖、
跨模块契约是否完整，以及第一个模块是否可以开始。
```

验证通过后，文档可以标记：

```text
Status: Approved for Module 1
Version: 0.1
```

### 3. 为当前模块开启新对话

每个模块使用一个独立对话，并读取：

```text
docs/PRD.md
docs/architecture.md
docs/tech-stack.md
docs/progress.md
docs/decisions.md
docs/modules/<current-module>.md
```

如果当前模块依赖其他模块，再读取对应的 `docs/contracts/*`。

启动提示词：

```text
Use $bot-eng and $openspec。

当前对话只负责 docs/modules/<module>.md 对应的模块。
读取全局文档、当前模块文件、Git 状态和 OpenSpec 状态。

先恢复上下文并完善模块边界，不要直接写代码。
确认职责、非职责、数据所有权、文件所有权、公开契约、
单向依赖、测试、验收标准和模块尺寸。

边界检查通过后，为该模块建立明确的 OpenSpec change。
```

### 4. 使用 OpenSpec 完成模块

典型流程：

```text
/opsx:explore
/opsx:new module-01-user-auth
/opsx:continue module-01-user-auth
暂停，展示 OpenSpec tasks 和实现摘要，等待人工确认
/opsx:apply module-01-user-auth
/opsx:verify module-01-user-auth
/opsx:sync module-01-user-auth
/opsx:archive module-01-user-auth
```

模块已经非常明确时，可以用 `/opsx:ff` 快速生成规划工件。

注意：`/opsx:continue` 或 `/opsx:ff` 生成 proposal、spec、design、tasks 之后，必须暂停。AI 需要先展示 change id、任务清单、预计修改文件、契约变化、测试计划和风险点，等待你明确确认后，才能进入 `/opsx:apply` 写实现代码。

实施提示词：

```text
Use $bot-eng and $openspec。

继续当前模块的 OpenSpec change。
只有在我已经明确确认 OpenSpec tasks 和实现摘要之后，
才可以通过 /opsx:apply 执行任务。

只修改当前模块拥有的文件以及经过批准的契约和文档。

每完成一组可验证任务就运行测试并提交 Git。
完成后运行模块测试、集成测试和完整累积冒烟测试，
检查文件修改是否越界，再执行 /opsx:verify。

验证通过后更新模块文件、progress.md 和 decisions.md，
必要时同步规格，最后归档 change。
```

### 5. 完成模块并进入下一模块

一个模块只有同时满足以下条件才算完成：

- OpenSpec proposal、spec、design 和 tasks 与实现一致。
- 进入 `/opsx:apply` 前，用户已经明确确认 OpenSpec tasks 和实现摘要。
- `/opsx:verify` 通过。
- 模块验收标准通过。
- 模块测试和相关集成测试通过。
- 当前模块的核心场景已加入累积冒烟测试。
- 完整累积冒烟测试通过。
- Git 文件边界检查通过。
- `progress.md` 和 `decisions.md` 已更新。
- 模块文件记录最终行为和归档的 change ID。
- 必要的规格已经同步，OpenSpec change 已归档。

完成后开一个新对话，再处理下一个模块。

## 关键约束

### 数据所有权

每个数据库表或持久化数据结构只能有一个归属模块。跨模块写入必须经过归属模块的公开 API 或 Service。

### 单向依赖

后面的模块只能依赖前面已经完成的模块。禁止循环依赖。

### 公开契约

模块只能依赖其他模块发布在 `docs/contracts/*` 中的公开接口和类型，不能依赖内部实现。

### 文件所有权

每个模块声明自己拥有的文件和目录。模块完成前使用 Git diff 检查是否修改了边界外文件。

### 累积回归

每个模块留下少量高价值的自动化冒烟测试。后续任何模块完成前，都要运行所有已完成模块的冒烟测试。

## 已完成模块需要返工

不要从其他模块的对话里顺手修改已经归档的模块。

应当：

```text
暂停当前模块并提交
-> 更新 progress.md
-> 为旧模块打开独立修复对话
-> 创建新的 OpenSpec fix change
-> 分析下游影响
-> 修复并运行完整回归测试
-> 更新文档
-> sync 和 archive
-> 恢复原模块
```

change ID 示例：

```text
module-01-user-auth-fix-session-expiry
```

## 更新本地 Skill

如果已经克隆仓库：

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

更新后重新打开 Codex 会话。

## 仓库内容

```text
bot-eng/
  SKILL.md
  agents/
    openai.yaml
```

Skill 源文件：[bot-eng/SKILL.md](bot-eng/SKILL.md)

## License

本仓库当前未声明开源许可证。公开使用、修改或再分发前，请由仓库所有者补充合适的 `LICENSE` 文件。
