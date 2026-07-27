# Orchestrate Work

[中文](#中文) | [English](#english)

## 中文

`orchestrate-work` 是一个面向 Codex 的个人 Skill，用于把非简单任务组织成“主控、执行代理、独立验收”的工作流，同时避免为了并行而过度拆分任务。

主控始终负责理解用户意图、任务拆解、关键决策、冲突处理、结果整合和最终交付。子代理只接收边界明确的任务包，并返回产物、证据、风险和需要主控处理的事项。

### 主要能力

- 自动判断任务应该直接完成、串行执行还是并行派工。
- 根据任务的不确定性和验证难度选择合适的模型与推理强度。
- 使用标准任务合同限制子代理的目标、范围、交付物和升级条件。
- 避免多个代理同时修改相同文件或外部状态。
- 为重要结果增加独立验收，而不是让执行代理自行证明正确。
- 为子任务设置时间盒，并在超时后使用已有证据继续交付。
- 控制协调成本，小型任务由主控直接完成。

### 直接完成门槛

满足以下情况时不会创建执行代理：

- 整个任务可以在不超过三次短工具调用内完成和验证。
- 工作步骤强耦合，无法独立验收。
- 描述、等待和复核子代理的成本预计超过直接执行成本的约 25%。
- 任务依赖尚未解决的用户选择。

多个评估维度或报告标题不等于多个独立工作流。

### 安装

PowerShell：

```powershell
git clone https://github.com/Linsongrong/orchestrate-work.git "$env:USERPROFILE\.codex\skills\orchestrate-work"
```

macOS / Linux：

```bash
git clone https://github.com/Linsongrong/orchestrate-work.git ~/.codex/skills/orchestrate-work
```

安装后开启一个新的 Codex 任务，使 Skill 被重新发现。

### 使用

Skill 已启用隐式调用。面对存在独立工作流或需要独立验收的复杂任务时，Codex 可以自动使用它，不需要每次点名。

也可以显式调用：

```text
Use $orchestrate-work to complete this task with appropriate delegation and verification.
```

### 工作流

```text
用户目标
  -> 主控判断是否值得派工
  -> 建立依赖关系和任务边界
  -> 执行代理处理独立工作流
  -> 必要时由独立代理验收
  -> 主控解决冲突并整合结果
  -> 向用户交付一个统一结论
```

### 文件结构

```text
orchestrate-work/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── routing-policy.md
    └── task-contract.md
```

- `SKILL.md`：核心决策与编排流程。
- `routing-policy.md`：模型、推理强度、并行和升级规则。
- `task-contract.md`：执行代理与验收代理的任务合同。
- `openai.yaml`：Codex 界面信息和隐式调用配置。

## English

`orchestrate-work` is a personal Codex Skill for coordinating non-trivial work through a controller, bounded worker agents, and independent verification without creating agents merely for the sake of parallelism.

The controller always owns user intent, decomposition, consequential decisions, conflict resolution, integration, and final delivery. Workers receive bounded task contracts and return artifacts, evidence, risks, and decisions that require controller attention.

### Key capabilities

- Decide whether work should be handled directly, sequentially, or in parallel.
- Route tasks to an appropriate model and reasoning effort based on uncertainty and verification difficulty.
- Bound worker objectives, scope, deliverables, and escalation conditions with a standard task contract.
- Prevent concurrent agents from editing the same files or external state.
- Add independent verification for important results instead of relying on worker self-assessment.
- Timebox delegated work and continue from available evidence when a worker overruns its collection window.
- Account for coordination cost and keep small tasks with the controller.

### Direct-work gate

The Skill does not create execution workers when:

- The complete task can be performed and verified in three or fewer short tool calls.
- The steps are tightly coupled and cannot be verified independently.
- Specifying, waiting for, and reviewing a worker is expected to cost more than roughly 25% of direct execution.
- The task depends on an unresolved user choice.

Multiple review dimensions or report headings do not automatically constitute independent workstreams.

### Installation

PowerShell:

```powershell
git clone https://github.com/Linsongrong/orchestrate-work.git "$env:USERPROFILE\.codex\skills\orchestrate-work"
```

macOS / Linux:

```bash
git clone https://github.com/Linsongrong/orchestrate-work.git ~/.codex/skills/orchestrate-work
```

Start a new Codex task after installation so the Skill can be discovered.

### Usage

Implicit invocation is enabled. Codex can automatically use the Skill for complex tasks with independent workstreams or meaningful verification needs.

It can also be invoked explicitly:

```text
Use $orchestrate-work to complete this task with appropriate delegation and verification.
```

### Workflow

```text
User objective
  -> Controller applies the delegation gate
  -> Controller maps dependencies and ownership
  -> Workers execute independent workstreams
  -> An independent agent verifies important results when needed
  -> Controller resolves conflicts and integrates evidence
  -> User receives one coherent outcome
```

### Repository structure

```text
orchestrate-work/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── routing-policy.md
    └── task-contract.md
```

- `SKILL.md`: Core orchestration and decision workflow.
- `routing-policy.md`: Model, reasoning effort, parallelism, budget, and escalation rules.
- `task-contract.md`: Worker and verifier assignment contracts.
- `openai.yaml`: Codex UI metadata and implicit invocation policy.
