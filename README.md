# Learning-First Development

> **Working software is not enough. You should still understand the system you are building.**
>
> **软件能运行还不够。你仍然应该理解自己正在构建的系统。**

[English](#english) · [中文](#中文)

---

# English

## What is Learning-First Development?

`learning-first-development` is a coding-agent workflow designed to keep **software progress aligned with the user's mental model and ownership of the project**.

AI coding agents can make a project move extremely quickly.

That is useful — until implementation complexity grows faster than your understanding of the system.

A common failure loop looks like this:

```text
I don't understand the domain
        ↓
AI implements the feature
        ↓
The codebase becomes harder to understand
        ↓
I become less confident changing it myself
        ↓
I delegate even more to AI
        ↓
I understand even less
```

Learning-First Development is designed to break that loop.

Its governing principle is:

> **Project progress must not significantly outpace the user's mental model of the system.**

The goal is not merely:

```text
WORKING SOFTWARE
```

but:

```text
WORKING SOFTWARE
        +
USER UNDERSTANDING
        +
PROJECT OWNERSHIP
```

---

## What this skill does

This skill changes the coding-agent interaction from:

```text
User request
    ↓
AI writes code
    ↓
Done
```

into a controlled development loop:

```text
Understand
    ↓
Assess risk
    ↓
Expose blindspots
    ↓
Build a mental model
    ↓
Analyze impact
    ↓
Plan one conceptual change
    ↓
Implement
    ↓
Verify
    ↓
Explain
    ↓
Continue
```

It is primarily an **orchestration and development-process skill**, not a code-generation shortcut.

The agent still writes code.

The difference is that implementation is structured so that you can continue answering questions such as:

* What part of the system am I changing?
* Why does this code exist?
* What happens when this request enters the application?
* Which modules participate in this behavior?
* What assumptions are we making?
* What exactly changed?
* How do we know it works?
* If it breaks tomorrow, where should I start investigating?

---

## Workflow

For non-trivial work, the workflow looks roughly like this:

```text
GOAL / CONTEXT
      ↓
TASK ASSESSMENT
      ↓
BLINDSPOT PASS ───────────── conditional
      ↓
FEATURE MAP ──────────────── conditional
      ↓
COMPREHENSION GATE A
      ↓
IMPACT ANALYSIS
      ↓
IMPLEMENTATION PLAN
      ↓
APPROVAL GATE B
      ↓
ONE CONCEPTUAL STEP
      ↓
VERIFY
   ↙       ↘
FAIL       PASS
 ↓          ↓
SYSTEMATIC  EXPLAIN THE DIFF
DEBUGGING        ↓
 ↓        COMPREHENSION GATE C
 └──────→ VERIFY       ↓
                  NEXT STEP
                      ↓
             KNOWLEDGE CONSOLIDATION
```

This is an **adaptive workflow**, not mandatory ceremony.

A typo should not require architecture analysis.

Authentication, billing, migrations, concurrency, infrastructure, or unfamiliar systems probably should.

---

## Risk-based routing

Every task starts with a lightweight assessment.

The agent considers factors such as:

* user familiarity;
* codebase familiarity;
* task complexity;
* architectural impact;
* reversibility;
* security and data risk;
* domain unfamiliarity;
* requirement uncertainty.

It then selects the appropriate route.

### Fast Path

For small, localized, reversible work:

```text
Risk: LOW
Scope: LOCALIZED
Mental-model impact: minimal
Route: FAST PATH
```

Typical examples:

* typo fixes;
* copy changes;
* localized CSS changes;
* obvious configuration edits;
* small changes in an already-understood area.

The agent performs the bounded change, verifies what is appropriate, and explains the result.

No unnecessary architecture lecture.

No artificial quiz.

No approval loop for every five-line diff.

### Guided Workflow

Used when the task is unfamiliar, cross-cutting, risky, or architecturally significant.

Examples include:

* authentication;
* payments and subscriptions;
* database migrations;
* concurrency;
* distributed systems;
* infrastructure;
* security-sensitive behavior;
* unfamiliar areas of a codebase.

The guided route introduces stronger analysis, ownership gates, and verification.

---

## Blindspot Pass

Before working in an unfamiliar or high-risk domain, the agent asks:

> **What might we not know that matters to this implementation?**

For example, subscription billing is not simply:

```text
create checkout button
→ charge customer
```

Relevant blindspots may include:

```text
Subscription lifecycle
├── trial
├── active
├── past_due
├── canceled
└── unpaid

Webhooks
├── authenticity
├── duplicate delivery
├── retries
└── out-of-order events

Data consistency
├── provider state
└── local database state

Entitlements
├── when access begins
└── when access is revoked
```

The goal is not to teach an entire textbook.

The goal is to expose **unknowns that could materially change the implementation**.

---

## Feature-level mental models

When the relevant code path is unfamiliar, the agent builds a focused map before editing.

Example:

```text
Browser
  ↓
POST /api/login
  ↓
AuthController
  ↓
AuthService
  ↓
UserRepository
  ↓
Database
```

Repository claims should be distinguished as:

```text
EVIDENCE
What the agent directly observed.

INFERENCE
What the agent concludes from that evidence.

UNKNOWN
What has not yet been established.
```

Confidence may also be reported:

```text
Confidence: HIGH | MEDIUM | LOW
```

This helps prevent an important AI failure mode:

> **A plausible guess being presented as a fact about the codebase.**

The workflow prefers **feature-level exploration** over repeatedly analyzing the entire repository.

---

## Comprehension Gate A

Before substantial implementation in an unfamiliar area, the agent may verify that the user's mental model is sufficient.

For example:

> In your own words, what path does this request take from the API entry point to persistence?

This is not an exam.

You do not need complete mastery of every framework detail.

The threshold is closer to:

> **Can I explain the relevant system conceptually, and do I know approximately where to investigate if something fails?**

If the user explicitly says:

> "I don't understand why this code exists."

implementation should pause.

The workflow returns to explanation and mental-model repair instead of silently producing more code.

---

## Impact analysis

Before substantial edits, the agent identifies:

```text
What will change?
Where will it change?
Why does it need to change?
What behavior should exist afterward?
```

When relevant, analysis also includes:

* affected modules;
* interfaces and contracts;
* data-model changes;
* migrations;
* tests;
* compatibility concerns;
* external systems;
* likely failure modes.

The principle is simple:

> **You should know what is about to happen before the agent starts changing the system.**

---

## One conceptual change at a time

A core rule is:

> **ONE CONCEPTUAL CHANGE AT A TIME**

This does **not** mean one file at a time.

One behavior may legitimately require several coordinated files.

Good:

```text
Step 1: Introduce Invitation state

Files:
- invitation.ts
- schema.sql
- invitation.test.ts
```

This is still one conceptual change.

Bad:

```text
Implement:
- invitations
- email delivery
- authorization
- expiration
- admin UI
- audit logging
- retries
```

Large features should be divided into understandable and independently verifiable steps.

---

## Approval Gate B

Before a substantial implementation step, the agent presents the bounded change:

```text
Current step:
Files likely affected:
Behavior being introduced:
Why this step exists:
How it will be verified:
```

For non-trivial work, implementation waits until that step is approved unless the user has already clearly authorized the exact change.

The purpose is not ceremony.

It is to prevent:

```text
User asks for feature
        ↓
Agent changes 20 files
        ↓
User receives "Done"
```

without understanding what happened in between.

---

## Implementation and TDD

When appropriate, implementation can follow a small TDD loop:

```text
RED
 ↓
Create or identify a failing test

GREEN
 ↓
Implement the minimum required behavior

VERIFY
 ↓
Run the relevant checks

EXPLAIN
 ↓
Describe what changed and why
```

TDD is used when it improves confidence and understanding.

It is not forced onto changes where it provides little value.

---

## Verification is part of implementation

The workflow does not treat:

```text
code written
```

as equivalent to:

```text
task completed
```

Verification can include:

* unit tests;
* integration tests;
* type checking;
* linting;
* builds;
* targeted manual checks;
* runtime observation;
* artifact inspection.

Results should be reported explicitly:

```text
VERIFIED:
What was actually run or observed.

NOT VERIFIED:
Important checks that were not performed.

BLOCKED:
Checks that could not be performed and why.
```

The agent should never claim something passed unless it actually ran or directly observed the relevant verification.

---

## Systematic debugging

Debugging is a **failure branch**, not a mandatory final phase.

When verification fails:

```text
OBSERVATION
     ↓
EVIDENCE
     ↓
HYPOTHESIS
     ↓
SMALLEST USEFUL EXPERIMENT
     ↓
RESULT
     ↓
CONFIRM / REJECT
     ↓
FIX
     ↓
VERIFY
```

The agent should avoid:

```text
try fix A
try fix B
change config C
refactor D
add retry E
```

all at once.

Instead:

> **Change one meaningful variable at a time and preserve the evidence.**

A workaround that happens to pass does not automatically prove that the root cause is understood.

---

## Explain the diff

After a meaningful implementation step, the agent explains:

```text
What changed?
Why?
Which modules changed?
How did the data/control flow change?
How was it verified?
What did not change?
```

The goal is not line-by-line narration.

The goal is to update your mental model of the system.

---

## Comprehension Gate C

For meaningful conceptual changes, the agent may ask one focused question after implementation.

Example:

> Why is `Invitation` represented separately from `Membership`?

The purpose is not quiz theater.

It is a lightweight check that implementation complexity has not moved significantly ahead of user understanding.

---

## Knowledge consolidation

At useful milestones, the workflow can consolidate durable project knowledge.

Depending on the repository and user preference:

```text
docs/
├── mental-model.md
├── decisions.md
├── unknowns.md
└── architecture.md
```

These files are optional.

The workflow should not automatically pollute every repository with documentation.

When persistent state is unavailable, the agent can instead produce a compact checkpoint:

```text
Current phase:
Current goal:
Relevant mental model:
Known unknowns:
Approved implementation step:
Last verified result:
Next action:
```

This can be carried into another session without pretending that persistent agent memory exists.

---

## Modes

### Learning Mode

Best for intentionally learning an unfamiliar project or domain.

Emphasizes:

* blindspots;
* feature maps;
* explanations;
* comprehension checks;
* smaller implementation steps.

Example:

```text
$learning-first-development Use learning mode.
Help me add authentication, but I have never implemented auth before.
```

### Balanced Mode

The default.

The agent decides how much process is justified by risk and familiarity.

```text
$learning-first-development Add project invitations.
Keep me oriented and wait before each substantial conceptual step.
```

### Fast Mode

For users who already understand the relevant system and want less ceremony.

```text
$learning-first-development Use fast mode.
Change this localized copy and report exactly what you verified.
```

Fast mode reduces process overhead.

It does **not** permit:

* fabricated evidence;
* fabricated verification;
* hidden high-risk consequences;
* silent scope expansion.

---

## Usage

Invoke the skill explicitly when you want to use this development workflow:

```text
$learning-first-development <your task>
```

Examples:

```text
$learning-first-development
Add audit logging, but keep me oriented and wait before each substantial conceptual step.
```

```text
$learning-first-development
Use balanced mode to investigate why checkout retries duplicate orders.
```

```text
$learning-first-development
Use learning mode. I need to add OAuth, but I do not understand the authentication flow yet.
```

```text
$learning-first-development
Use fast mode for this localized copy change and report exactly what you verified.
```

---

## Repository structure

```text
learning-first-development/
├── SKILL.md
│
├── agents/
│   └── openai.yaml
│
└── references/
    ├── operating-model.md
    └── evaluation-and-examples.md
```

### `SKILL.md`

Primary workflow definition.

### `references/operating-model.md`

Detailed routing, state-machine, gate, transition, and fallback behavior.

### `references/evaluation-and-examples.md`

Behavioral scenarios used to evaluate whether the workflow behaves correctly.

### `agents/openai.yaml`

Agent-facing metadata.

---

## Design principles

### 1. User ownership over AI takeover

The agent can implement code.

The user should retain understanding and decision ownership.

### 2. Understanding over generated explanation

Producing more explanation is not automatically better.

Analysis should stop when it has answered the current engineering decision.

### 3. Evidence over plausibility

A convincing explanation is not necessarily a correct explanation.

### 4. Small conceptual steps over giant diffs

Changes should remain understandable and reviewable by a human.

### 5. Verification over confidence

"I think this works" is not verification.

### 6. Adaptive process over bureaucracy

Process should become stricter as risk, uncertainty, unfamiliarity, and architectural impact increase.

---

## Anti-patterns

The skill explicitly tries to prevent:

### AI takeover

```text
Request
→ huge implementation
→ "Done"
```

### Analysis theater

Large reports that do not affect a decision.

### Quiz theater

Meaningless comprehension questions used only to satisfy a workflow step.

### Tutorial overload

Explaining every framework concept instead of what matters now.

### Scope creep

Refactoring unrelated code during an approved task.

### Fake certainty

Presenting inference as repository fact.

### Fake verification

Claiming success without checking it.

### Process deadlock

Using so many gates that ordinary development becomes unusable.

---

## Why this exists

AI dramatically lowers the cost of producing software.

But it can also lower the amount of contact a developer has with the software being produced.

That can create a new form of technical debt:

> **The codebase grows faster than the developer's mental model of it.**

Learning-First Development treats that gap as something worth actively managing.

AI should increase your leverage.

It should not require surrendering your understanding of the project.

---

## Core principle

```text
The agent accelerates implementation.

The user retains understanding and ownership.
```

> **Working software + user understanding.**

---

# 中文

## 什么是 Learning-First Development？

`learning-first-development` 是一套面向 AI 编程 Agent 的开发工作流。

它的目标是让：

> **项目的实现进度，与用户对项目的理解和掌控程度保持同步。**

AI 编程 Agent 可以极大提高开发速度。

问题在于，当 AI 实现系统的速度远远超过你理解系统的速度时，你可能会逐渐失去对项目的掌控。

常见的恶性循环是：

```text
我不了解这个领域
        ↓
让 AI 实现功能
        ↓
项目变得更加复杂
        ↓
我越来越看不懂代码
        ↓
我越来越不敢自己修改
        ↓
进一步依赖 AI
        ↓
对项目理解越来越少
```

Learning-First Development 就是为了打断这个循环。

它的最高原则是：

> **项目推进速度不应该显著超过用户建立项目 Mental Model 的速度。**

目标不只是：

```text
能运行的软件
```

而是：

```text
能运行的软件
      +
用户理解
      +
项目掌控权
```

---

## 这个 Skill 做什么？

普通的 AI 编程流程通常是：

```text
用户提出需求
    ↓
AI 写代码
    ↓
完成
```

Learning-First Development 将其变成：

```text
理解问题
    ↓
评估风险
    ↓
发现认知盲区
    ↓
建立 Mental Model
    ↓
分析影响范围
    ↓
规划一个概念性修改
    ↓
实现
    ↓
验证
    ↓
解释变化
    ↓
继续下一步
```

它首先是一套 **开发流程与编排 Skill**，而不是一个“让 AI 写更多代码”的 Skill。

AI 仍然可以负责大量实现工作。

区别在于，你应该始终能够回答：

* 我现在修改的是系统的哪一部分？
* 这段代码为什么存在？
* 一个请求进入系统以后会经过哪里？
* 哪些模块参与这个行为？
* 当前实现依赖哪些假设？
* AI 刚刚到底修改了什么？
* 我们怎么知道它真的能工作？
* 如果明天这里坏了，我应该先从哪里排查？

---

## 工作流

对于非简单任务，整体流程大致如下：

```text
目标 / 上下文
      ↓
任务评估
      ↓
Blindspot Pass ───────────── 按需
      ↓
Feature Map ──────────────── 按需
      ↓
理解检查 Gate A
      ↓
影响分析
      ↓
实施计划
      ↓
批准 Gate B
      ↓
一次一个 Conceptual Change
      ↓
验证
   ↙       ↘
失败       成功
 ↓          ↓
系统化       解释修改
Debug           ↓
 ↓          理解检查 Gate C
 └──────→ 再次验证      ↓
                    下一步
                      ↓
                  知识沉淀
```

这是一套**自适应工作流**，不是固定仪式。

修改一个错别字不应该先做架构分析。

但下面这些任务通常值得更加谨慎：

* 身份认证；
* 支付与订阅；
* 数据库迁移；
* 并发；
* 分布式系统；
* 基础设施；
* 安全相关功能；
* 完全陌生的代码区域。

---

## 基于风险进行路由

每个任务首先进行轻量评估。

Agent 会考虑：

* 用户对领域是否熟悉；
* 用户对代码库是否熟悉；
* 任务复杂度；
* 架构影响范围；
* 修改是否容易回滚；
* 安全和数据风险；
* 领域陌生程度；
* 需求是否存在较多不确定性。

然后选择合适的开发路径。

### Fast Path

适用于小范围、低风险、容易回滚的修改：

```text
Risk: LOW
Scope: LOCALIZED
Mental-model impact: minimal
Route: FAST PATH
```

例如：

* 修复错别字；
* 修改文案；
* 局部 CSS 调整；
* 明确的配置修改；
* 修改用户已经非常熟悉的代码。

这种情况下，Agent 可以直接完成有限范围的修改，然后进行适当验证并解释结果。

不需要架构课。

不需要强行提问。

也不需要每修改五行代码就请求一次批准。

### Guided Workflow

适用于：

* 陌生任务；
* 跨模块任务；
* 高风险任务；
* 架构影响较大的任务。

例如：

* Authentication；
* Payment / Subscription；
* Database Migration；
* Concurrency；
* Distributed System；
* Infrastructure；
* Security；
* 用户完全不了解的代码区域。

此时工作流会提高分析、理解检查和验证强度。

---

## Blindspot Pass：先找出你不知道什么

在陌生或者高风险领域开始开发之前，Agent 首先应该思考：

> **有哪些我们现在还没有意识到、但可能直接影响实现的问题？**

例如“增加订阅支付”并不只是：

```text
创建支付按钮
→ 收钱
```

还可能涉及：

```text
Subscription 生命周期
├── trial
├── active
├── past_due
├── canceled
└── unpaid

Webhook
├── 验证真实性
├── 重复事件
├── retry
└── 乱序事件

数据一致性
├── 支付平台状态
└── 本地数据库状态

权限
├── 什么时候开放功能
└── 什么时候撤销功能
```

Blindspot Pass 的目的不是在正式开发之前先学完一整本教材。

它只关注：

> **那些会实际改变当前实现方案的未知因素。**

---

## Feature-level Mental Model

当用户不熟悉相关代码时，Agent 应该先建立当前 Feature 的 Mental Model。

例如：

```text
Browser
  ↓
POST /api/login
  ↓
AuthController
  ↓
AuthService
  ↓
UserRepository
  ↓
Database
```

分析代码库时，应明确区分：

```text
EVIDENCE
Agent 在代码、测试或运行结果中直接观察到的事实。

INFERENCE
Agent 根据 Evidence 做出的推断。

UNKNOWN
目前还没有被确认的部分。
```

必要时还可以标记：

```text
Confidence: HIGH | MEDIUM | LOW
```

这可以减少 AI 编程中非常危险的一种情况：

> **把一个“听起来合理”的猜测，当成代码库里的事实。**

同时，工作流优先建立 **Feature-level Mental Model**，而不是每做一个功能都重新分析整个 Repository。

---

## 理解检查 Gate A

如果用户正在进入一个陌生领域，在大规模实现之前，Agent 可以进行一次简单的理解检查。

例如：

> 请用自己的话描述一下，这个请求从 API 入口进入系统以后，到最终写入数据库会经过哪些部分？

这不是考试。

用户不需要掌握框架中的所有细节。

真正需要确认的是：

> **我是否能够在概念层面解释这个系统？如果它坏了，我是否大概知道应该从哪里开始排查？**

如果用户明确说：

> “我不理解为什么这里需要这段代码。”

Agent 应该暂停进一步实现。

先修复 Mental Model，再继续增加代码。

---

## Impact Analysis：修改之前先说明影响

开始重要修改之前，Agent 应该先说明：

```text
准备修改什么？
在哪里修改？
为什么需要修改？
修改以后系统应该出现什么行为？
```

必要时还应分析：

* 受影响模块；
* Interface / Contract；
* 数据模型；
* Migration；
* Tests；
* Backward Compatibility；
* External Systems；
* Failure Modes。

核心原则是：

> **Agent 开始修改系统之前，用户应该大致知道即将发生什么。**

---

## 一次只做一个 Conceptual Change

这套工作流最重要的规则之一是：

> **ONE CONCEPTUAL CHANGE AT A TIME**

也就是：

> **一次只完成一个概念层面的变化。**

它并不意味着“一次只能修改一个文件”。

一个功能行为本来就可能跨多个文件。

合理：

```text
Step 1：增加 Invitation 状态

Files:
- invitation.ts
- schema.sql
- invitation.test.ts
```

虽然修改三个文件，但它们共同实现一个概念：

> 系统开始拥有 Invitation 这个状态。

不合理：

```text
一次实现：
- Invitation
- 邮件发送
- Authorization
- Expiration
- Admin UI
- Audit Log
- Retry
```

大型功能应该拆成：

* 能理解；
* 能验证；
* 能独立讨论；

的小步骤。

---

## Approval Gate B

在开始一个重要实现步骤之前，Agent 应该先展示：

```text
当前步骤：
预计影响哪些文件：
准备新增什么行为：
为什么需要这个步骤：
准备如何验证：
```

对于非简单任务，通常应该在用户批准这个 Conceptual Change 后再开始实施。

目的不是制造流程。

而是避免：

```text
用户提出一个需求
        ↓
AI 修改 20 个文件
        ↓
AI："Done."
```

但用户完全不知道中间发生了什么。

---

## Implementation 与 TDD

适合使用 TDD 的场景，可以采用：

```text
RED
 ↓
创建或找到一个失败测试

GREEN
 ↓
实现最少必要代码

VERIFY
 ↓
运行验证

EXPLAIN
 ↓
解释发生了什么
```

TDD 是一种工具，而不是宗教。

只有当它确实能够提高：

* 正确性；
* 可验证性；
* 用户理解；

时才应该使用。

---

## Verification 是实现的一部分

Learning-First Development 不认为：

```text
代码写完了
```

等于：

```text
任务完成了
```

验证可以包括：

* Unit Test；
* Integration Test；
* Type Check；
* Lint；
* Build；
* 手工验证；
* Runtime Observation；
* Artifact Inspection。

结果应该明确区分：

```text
VERIFIED:
实际运行或观察过的内容。

NOT VERIFIED:
没有进行的重要检查。

BLOCKED:
由于某些原因无法进行的检查。
```

Agent 不应该因为“代码看起来没问题”就声称：

> All tests pass.

只有真正运行并观察到结果，才能说已经验证。

---

## Systematic Debugging

Debugging 是失败分支，不是每个任务必须执行的最后一步。

当验证结果与预期不一致时：

```text
OBSERVATION
     ↓
EVIDENCE
     ↓
HYPOTHESIS
     ↓
最小有效实验
     ↓
RESULT
     ↓
确认 / 否定假设
     ↓
FIX
     ↓
VERIFY
```

避免：

```text
试 Fix A
试 Fix B
改 Config C
顺手 Refactor D
再加 Retry E
```

一起做。

更好的原则是：

> **一次只改变一个有意义的变量，并保留证据。**

某个 workaround 恰好成功，也不代表已经找到了 Root Cause。

---

## Explain the Diff：修改之后更新用户的 Mental Model

完成一个重要 Conceptual Change 后，Agent 应解释：

```text
改了什么？
为什么？
哪些模块发生变化？
Data Flow / Control Flow 怎么变化？
如何验证？
哪些东西没有变化？
```

目标不是逐行朗读 Git Diff。

目标是：

> **让用户的 Mental Model 与最新代码重新同步。**

---

## 理解检查 Gate C

完成重要概念变化之后，可以进行一个轻量理解检查。

例如：

> 为什么这里需要把 `Invitation` 和 `Membership` 分成两个概念？

目的不是考试。

而是检查：

> **代码复杂度是不是又开始跑在用户理解前面。**

---

## Knowledge Consolidation：沉淀长期认知

在合适的里程碑，可以把已经形成的知识保存下来。

例如：

```text
docs/
├── mental-model.md
├── decisions.md
├── unknowns.md
└── architecture.md
```

这些文件都是可选的。

Skill 不应该默认污染所有 Repository。

如果当前 Agent 没有可靠的持久记忆，也可以生成一个简单的 Workflow Checkpoint：

```text
Current phase:
Current goal:
Relevant mental model:
Known unknowns:
Approved implementation step:
Last verified result:
Next action:
```

下次开启新会话时，可以把它作为上下文继续工作。

---

## 模式

### Learning Mode

适用于：

> 我正在进入一个自己完全不熟悉的领域，希望做项目的同时真正学会它。

强调：

* Blindspot；
* Mental Model；
* 解释；
* 理解检查；
* 更小的 Implementation Step。

示例：

```text
$learning-first-development Use learning mode.
Help me add authentication, but I have never implemented auth before.
```

---

### Balanced Mode

默认模式。

Agent 根据任务风险和用户熟悉程度，自行决定需要多少流程。

```text
$learning-first-development Add project invitations.
Keep me oriented and wait before each substantial conceptual step.
```

---

### Fast Mode

适用于用户已经理解相关系统，只希望 AI 加速执行。

```text
$learning-first-development Use fast mode.
Change this localized copy and report exactly what you verified.
```

Fast Mode 可以减少流程，但不能允许：

* 伪造 Evidence；
* 伪造 Verification；
* 隐藏高风险后果；
* 偷偷扩大 Scope。

---

## 使用方法

需要这套工作流时，显式调用：

```text
$learning-first-development <你的任务>
```

例如：

```text
$learning-first-development
Add audit logging, but keep me oriented and wait before each substantial conceptual step.
```

```text
$learning-first-development
Use balanced mode to investigate why checkout retries duplicate orders.
```

```text
$learning-first-development
Use learning mode. I need to add OAuth, but I do not understand the authentication flow yet.
```

```text
$learning-first-development
Use fast mode for this localized copy change and report exactly what you verified.
```

---

## Repository 结构

```text
learning-first-development/
├── SKILL.md
│
├── agents/
│   └── openai.yaml
│
└── references/
    ├── operating-model.md
    └── evaluation-and-examples.md
```

### `SKILL.md`

定义主要工作流。

### `references/operating-model.md`

定义：

* Routing；
* State Machine；
* Gate；
* Transition；
* Fallback Behavior。

### `references/evaluation-and-examples.md`

用于测试 Skill 是否真的按照预期工作。

### `agents/openai.yaml`

Agent-facing metadata。

---

## 设计原则

### 1. User Ownership > AI Takeover

AI 可以实现代码。

但是理解、判断和项目 ownership 应该保留在用户手中。

### 2. Understanding > Generated Explanation

解释得更多不代表理解得更好。

分析应该在解决当前工程决策后停止。

### 3. Evidence > Plausibility

“听起来合理”不等于“代码就是这样工作的”。

### 4. Small Conceptual Steps > Giant Diffs

优先让每一次变化保持在人能够理解和 Review 的范围内。

### 5. Verification > Confidence

> “我觉得应该可以。”

不是 Verification。

### 6. Adaptive Process > Bureaucracy

风险越高、领域越陌生、不确定性越大，流程应该越严格。

任务越简单、用户越熟悉，流程应该越轻。

---

## 需要避免的 Anti-patterns

### AI Takeover

```text
需求
→ 大规模实现
→ "Done"
```

### Analysis Theater

生成大量分析，但并没有帮助实际决策。

### Quiz Theater

为了满足流程而提出没有价值的问题。

### Tutorial Overload

不管是否相关，把整个框架从头讲一遍。

### Scope Creep

实现一个功能时顺手修改大量无关内容。

### Fake Certainty

把推断描述成代码库事实。

### Fake Verification

没有运行测试却声称测试通过。

### Process Deadlock

流程复杂到开发本身无法正常进行。

---

## 为什么需要这个项目？

AI 大幅降低了生产软件的成本。

但是它同时也可能降低开发者与代码本身接触的程度。

这会形成一种新的技术债：

> **Codebase 增长的速度，超过了开发者 Mental Model 增长的速度。**

Learning-First Development 将这种差距视为一种需要主动管理的问题。

AI 应该增加你的杠杆。

而不是要求你放弃对项目的理解。

---

## 核心原则

```text
AI Agent 负责加速实现。

用户保留理解与项目掌控权。
```

> **Working software + user understanding.**
