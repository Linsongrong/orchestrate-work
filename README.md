# Orchestrate Work

[中文](#中文) | [English](#english)

```mermaid
flowchart TD
    U["用户目标 / User objective"] --> M{"模式 / Mode"}
    M -->|"真正简单且局部 / Truly trivial and local"| D["主控直接完成 / Direct controller work"]
    M -->|"非简单或长程 / Non-trivial or long-running"| P["Phase profile and budget / 阶段档位与预算"]
    P --> L["持久任务账本 / Durable task ledger"]
    L --> W["依赖波次 / Dependency waves"]
    W --> S["侦察 / Scout"]
    W --> E["执行 / Executor"]
    E --> I["集成 / Integrator"]
    S --> C["主控决策 / Controller decision"]
    I --> V["独立验收 / Independent verifier"]
    V -->|"拒绝 / Reject"| E
    V -->|"通过 / Accept"| FZ["Phase freeze / 阶段冻结"]
    FZ --> C
    C --> W
    C --> F["最终集成验收与交付 / Final integrated verification and delivery"]
    D --> F
```

## 中文

`orchestrate-work` 是一个面向 Codex 的个人 Skill，用于以“主控优先”的方式协调非简单或长程工作。主控负责管理和裁决，而不是承担主要执行分支；侦察、实现、集成和独立验收交给边界明确的直接子代理。

显式调用 `$orchestrate-work` 时，除非任务确实简单且局部，否则会进入编排模式。隐式调用仍然启用，可用于存在独立工作流、大量探索或工具输出、长时间实现，或者重要结果需要独立验收的任务。

### 核心行为

- 建立持久任务账本，记录目标、验收标准、非目标、既定决策、依赖、工作状态、后续工作和风险；每个波次或重大决策后更新，并在上下文压缩后恢复。
- 每个 phase 先选择 assurance profile：默认 `formal`；只有明确要求快速原型时使用 `prototype`；`release` 只在明确请求时使用。`formal` 保留全部原型保障并增加相称的独立验证，`release` 再增加环境、依赖和发布审计。原型减少重复文档 Gate 和全历史复验，但保留关键独立验收与端到端证据。
- 对付费模型调用、真实运行、远程写入、不可逆操作和密钥持久化设置显式预算；预算耗尽后，子代理必须交回主控续发，不能自行重试。
- 按依赖波次派遣所有已就绪且互不冲突的工作流，不默认只派一个代理，也不为强依赖工作强行并行。
- 使用四种角色：侦察代理、执行代理、集成代理和验收代理。子代理不得继续创建代理；验收代理只报告，不修复产物。
- 根据任务复杂度选择模型：`gpt-5.6-luna` 处理确定性提取和常规检查，`gpt-5.6-terra` 处理常规实现、测试、文档和诊断，`gpt-5.6-sol` 处理复杂实现、研究、冲突分析和重要验收。
- 为文件、目录、产物和外部状态分配唯一所有者。只有真正互不影响时才共享工作树，否则使用隔离工作树并安排集成代理。
- 并发共享工作树时在总账记录路径 ownership map；候选进入独立验收后冻结功能路径，验收代理只读，集成代理只改被分配的整合路径。
- 区分功能候选、独立验收结论与状态整合：验收结论不可改候选，状态整合不可改 verdict；是否分别提交由仓库策略决定。
- 子代理只接收任务局部上下文，并以紧凑的结构返回产物位置、证据、发现和风险；原始日志留在子代理上下文中。
- 使用完成门槛、软超时和停滞检测，而不是固定分钟数。第一次验收失败退回原执行代理；第二次失败后重新评估模型、代理或方案；持续失败时停止该路线。
- 重要产物需要独立验收，最终集成结果必须完成适当验收。只有所有验收标准通过后才声明完成。
- 新分支只有在验收所必需、阻塞当前工作或推翻关键假设时才进入范围。重大范围变化由用户决定。
- phase 被接受后，先记录完成交付物、未完成非目标、最新 Gate、下一 objective、profile 和授权；没有这一步，“继续”不能自然扩展为下一大阶段。
- 子代理只能使用完成合同所需的专业技能，不能再次调用 `orchestrate-work`、派工或新建任务；需要拆分时必须交回主控。
- 默认使用直接子代理。只有工作长期独立、需要用户单独控制或强工作树隔离时，才建议新建用户任务，并且必须先获得明确授权。

### 安装

PowerShell：

```powershell
git clone https://github.com/Linsongrong/orchestrate-work.git "$env:USERPROFILE\.codex\skills\orchestrate-work"
```

macOS / Linux：

```bash
git clone https://github.com/Linsongrong/orchestrate-work.git ~/.codex/skills/orchestrate-work
```

Codex 会自动检测 Skill 更新；若列表或调用结果未刷新，再重启 Codex。

### 使用

```text
Use $orchestrate-work to coordinate this task through bounded agents and independent verification.
```

### 文件

- `SKILL.md`：两种模式、主控边界和核心控制循环。
- `references/routing-policy.md`：角色、模型、波次并行、所有权、恢复和升级规则。
- `references/task-contract.md`：四种角色的任务合同和紧凑返回格式。
- `agents/openai.yaml`：Codex 界面信息和隐式调用配置。

## English

`orchestrate-work` is a personal Codex Skill for controller-first coordination of non-trivial or long-running work. The controller manages and judges rather than owning a main execution branch; bounded direct child agents perform scouting, implementation, integration, and independent verification.

Explicit `$orchestrate-work` invocation enters orchestration mode unless the task is genuinely trivial and local. Implicit invocation remains enabled for tasks with independent workstreams, substantial exploration or tool output, long implementation, or important results that need an independent check.

### Core behavior

- Create a durable task ledger for the objective, acceptance criteria, non-goals, settled decisions, dependencies, work status, next work, and risks. Update it after each wave or material decision and restore it after context compaction.
- Select an assurance profile for each phase: `formal` by default, `prototype` only for an explicitly requested rapid prototype, and `release` only when explicitly requested. Formal retains every prototype safeguard and adds proportional independent verification; release adds environment, dependency, and release audits. Prototype removes repeat documentation Gates and full-history revalidation, not critical independent verification or end-to-end evidence.
- Set an explicit budget for paid model calls, real runs, remote writes, irreversible actions, and secret persistence. Workers must return to the controller for renewal rather than retrying after exhaustion.
- Dispatch all ready, non-conflicting workstreams in dependency waves. Do not default to one agent or force parallelism onto serial work.
- Use four roles: scout, executor, integrator, and verifier. Child agents may not spawn; verifiers report and never repair artifacts.
- Route adaptively: `gpt-5.6-luna` for deterministic extraction and routine checks, `gpt-5.6-terra` for routine implementation, tests, docs, and diagnosis, and `gpt-5.6-sol` for complex implementation, research, conflict analysis, and important verification.
- Assign one owner to each file, directory, artifact, and external state. Share a worktree only for truly non-interacting changes; otherwise isolate worktrees and appoint an integrator.
- Record a path ownership map for concurrent writers in a shared worktree. Freeze candidate paths during independent verification; verifiers are read-only and integrators modify only assigned integration paths.
- Keep functional candidates, independent verdicts, and status integration separate. Verdicts never modify candidates, and status integration never changes a verdict; repository policy decides whether they become separate commits.
- Give agents task-local context and require compact returns with artifact locations, evidence, findings, and risks. Keep raw logs in worker context.
- Use completion gates, soft timeouts, and stall detection instead of fixed minute limits. Return a first verification failure to the original executor, reassess the model, agent, or approach after a second failure, and stop a repeatedly failing route.
- Independently verify important artifacts and always verify the final integrated result appropriately. Claim completion only after every acceptance criterion passes.
- Admit side branches only when acceptance requires them, current work is blocked, or a key assumption is disproved. Ask the user about material scope changes.
- After accepting a phase, record completed deliverables, explicit non-goals, the latest Gate, next objective, profile, and authority. Without that freeze, "continue" cannot silently become the next large phase.
- Child agents may use specialist skills needed for their contract, but may not invoke `orchestrate-work`, delegate, or create tasks; they return decomposition needs to the controller.
- Use direct child agents by default. Suggest a new user-owned task only for long-lived independently steerable work or strong worktree isolation, and obtain explicit authorization first.

### Installation

PowerShell:

```powershell
git clone https://github.com/Linsongrong/orchestrate-work.git "$env:USERPROFILE\.codex\skills\orchestrate-work"
```

macOS / Linux:

```bash
git clone https://github.com/Linsongrong/orchestrate-work.git ~/.codex/skills/orchestrate-work
```

Codex detects Skill updates automatically. Restart Codex only if the skill list or invocation does not refresh.

### Usage

```text
Use $orchestrate-work to coordinate this task through bounded agents and independent verification.
```

### Files

- `SKILL.md`: Two modes, controller boundaries, and the core control loop.
- `references/routing-policy.md`: Roles, models, wave parallelism, ownership, recovery, and escalation.
- `references/task-contract.md`: Contracts and compact returns for all four roles.
- `agents/openai.yaml`: Codex UI metadata and implicit invocation policy.
