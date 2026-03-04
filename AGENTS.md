# AGENTS.md -- {{项目名称}}

{{项目一句话描述}}

---

# 一、通用规范（项目无关）

## Skills（可复用操作）

Skills 是可复用的 AI 操作单元。触发后，AI 读取对应文件、按定义步骤执行。触发方式见下表"触发"列。详细定义见 `.harness/skills/` 目录。

| Skill | 触发 | 文件 |
|-------|------|------|
| 功能迭代 | 人工下发功能需求 | .harness/skills/feature-iterate.md |
| 回填-知识库更新 | 人工指令 | .harness/skills/backfill-knowledge.md |
| 回填-产品基线更新 | 人工指令 | .harness/skills/backfill-prd-baseline.md |
| 代码质量扫描 | 人工指令 | .harness/skills/code-quality-scan.md |
| 废弃代码清理 | 人工指令 | .harness/skills/dead-code-cleanup.md |
| 构建验证 | 功能迭代完成后自动执行，或人工指令 | .harness/skills/build-verify.md |
| 提取-Skill | 人工指令 | .harness/skills/extract-skill.md |
| 提取-Subagent | 人工指令 | .harness/skills/extract-subagent.md |
| 回填-产品SENSE更新 | 人工指令 | .harness/skills/backfill-prd-sense.md |
| 回填-AGENTS更新 | 人工指令 | .harness/skills/backfill-agents.md |
| 治理巡检 | 人工指令 | .harness/skills/governance-review.md |
| 任务总结 | AI自动触发（任务完成后） | .harness/skills/task-summary.md |
| {{Skill名称}} | {{触发方式}} | .harness/skills/{{skill文件名}}.md |

触发规则：表中标注"AI自动触发"的 Skill，AI 必须在对应时机自动执行，不需要人工指令。当前自动触发清单：
- 任务完成后：执行 Skill: 任务总结（适用于所有任务，包括功能迭代、人工指令触发的 Skill、以及其他独立任务）

## 文件与文档

- 除非明确要求，不要主动创建 README 文件
- 不要删除任何项目文件，包括文档、代码等（临时 spec 文件 `agent-specs-*.md` 除外，任务结束后必须删除、且使用`rm -f`非交互模式）
- AI 自动生成的文档（Skill、Subagent、知识库等），文件名必须使用小写英文（kebab-case），文档正文中补充对应的中文名称或说明
- Skill、Subagent 文件名使用小写英文（kebab-case），技能名称（文件内标题）使用中文（英文专有名词除外）
- .harness/context/users/ 目录是人工定义的原始信息，AI 可以读取、但不允许自动修改；如遇 users/ 内容与 AI 知识库（.harness/context/agents/）描述冲突，必须提示给用户，经确认后才能修改
- .harness/docs/ 目录是人工维护的方法论与参考文档，AI 修改前必须经过人工确认
- 文档内容禁用 emoji 图标、加粗、斜体等润色，使用普通文字
- 功能迭代的临时 spec 文件落盘到 `.harness/context/agents/agent-specs-${事项}.md`，任务结束后必须删除；该文件不属于持久知识，仅供单次迭代过程中保持上下文连续性

## 上下文知识库管理

- 每类知识有且只有一个归属文档，不重复维护
- 上下文窗口有限，不需要的文档一律不加载
- 接到任务时按需查阅 .harness/context/ 目录（不需要全部读完）

## 维护

每次修改约束、规范、规则（包括 AGENTS.md、Skills、Subagents、知识库文件）时，必须检查 AGENTS.md 的全局描述，确保新增或变更的内容与已有规则没有矛盾冲突。

当 Agent 因缺少说明而出错时：
1. 将缺失的说明补充到对应的 .harness/context/agents/ 知识库文件
2. 若是普遍性约束，同时在本文件"项目规范"中摘录要点
3. 在下方"上下文知识库"文档列表中更新对应文件的描述

---

# 二、项目规范（项目相关）

## 仓库结构

```
AGENTS.md              -- AI 知识库入口、操作约束RULES（本文件）
.harness/
  skills/              -- AI 可复用操作定义（功能迭代、构建验证等）
  subagents/           -- Subagent prompt 模板（代码质量扫描等并行任务）
  docs/
    00-harness-ops.md  -- Harness 项目维护（人工维护）
    01-harness-desc.md -- 通用 AI 协作工程方法论（项目无关，可跨项目复用）
    02-dev-summary.md  -- 开发总结
  context/
    agents/            -- AI 知识库
      01-overview.md   -- 项目概览
      02-architecture.md -- 架构与模块边界
      03-conventions.md  -- 约定与约束（实现细节）
      04-glossary.md   -- 术语表
      05-data-boundaries.md -- 数据与类型边界
      06-file-map.md   -- 功能与文件映射
      07-key-patterns.md -- 关键代码模式
    users/             -- 人工定义的原始信息（AI只读）
      01-prd-sense.md  -- 产品定位、体验原则与判断准则
      01-prd-baseline.md -- 稳定的产品需求基线
      01-prd-specs.md  -- 原始产品需求规格与演进记录
{{项目源码目录结构}}
```

## 构建与测试

```bash
{{构建命令示例}}

{{测试命令示例}}

{{其他环境配置说明}}
```

## 知识回填规则

Skill: 功能迭代 step 6 的具体回填目标：
- 架构边界变化 -> 02-architecture.md
- 新增术语 -> 04-glossary.md
- 数据结构或存储格式变化 -> 05-data-boundaries.md
- 新增源文件 -> 06-file-map.md
- 新增跨文件模式 -> 07-key-patterns.md
- 产品方向或判断准则调整 -> 提示用户，由人工更新 users/01-prd-sense.md 或触发 Skill: 回填-产品SENSE更新

## 代码生成

以下各节（代码生成、架构边界、质量守护、安全规范）为快速参考摘要，权威定义和实现细节见 .harness/context/agents/03-conventions.md，以 03-conventions.md 为准。

{{项目专属的代码生成规范，例如：}}
{{- 日志输出方式与分类}}
{{- UI 交互约定（手势、导航等）}}
{{- 常量管理规则}}
{{- 数据处理约定（图片、音频、文件等）}}

## 架构边界

{{项目专属的架构边界规则，例如：}}
{{- 分层调用约束（UI层不直接调用底层服务等）}}
{{- 全局状态管理方式}}
{{- 模块间通信规则}}

## 质量守护

{{项目专属的质量守护规则，例如：}}
{{- 编译零警告要求}}
{{- 单元测试覆盖要求}}
{{- 错误信息分层（用户提示 vs 开发者日志）}}
{{- 网络请求超时设置}}
{{- 异步操作线程管理}}
{{- 内存与资源管理}}

## 安全规范

{{项目专属的安全规范，例如：}}
{{- 密钥存储方式}}
{{- 敏感信息日志脱敏}}
{{- 网络传输安全}}
{{- 外部输入校验}}
{{- 本地数据隐私}}

## 上下文知识库

| 文件 | 何时查阅 |
|------|---------|
| .harness/context/users/01-prd-sense.md | 功能迭代前，确认产品定位、体验原则和判断准则 |
| .harness/context/agents/01-overview.md | 任何任务开始时，了解项目边界和入口 |
| .harness/context/agents/02-architecture.md | 涉及模块新增、依赖关系、跨层调用时 |
| .harness/context/agents/03-conventions.md | 涉及编码约定、质量约定、安全约定的实现细节时 |
| .harness/context/agents/04-glossary.md | 对术语不清楚时 |
| .harness/context/agents/05-data-boundaries.md | 涉及数据结构、磁盘存储、配置格式时 |
| .harness/context/agents/06-file-map.md | 确定功能对应哪些源文件时 |
| .harness/context/agents/07-key-patterns.md | 实现跨模块交互、关键流程等模式时 |
| .harness/context/users/01-prd-baseline.md | 实现新功能或页面时，确认功能需求与产品约束 |
| .harness/context/users/01-prd-specs.md | 需要了解某功能的原始产品需求规格、或处理历史遗留逻辑时 |
