# AGENTS.md -- {{项目名}}

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
| 提取-Harness模板 | 人工指令 | .harness/skills/extract-harness-tpl.md |
| 回填-产品SENSE更新 | 人工指令 | .harness/skills/backfill-prd-sense.md |
| 回填-AGENTS更新 | 人工指令 | .harness/skills/backfill-agents.md |
| 治理巡检 | 人工指令 | .harness/skills/governance-review.md |
| 任务总结 | AI自动触发（任务完成后） | .harness/skills/task-summary.md |

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
    00-harness-ops.md  -- Harness 项目维护（蒸馏模板、治理操作入口，人工维护）
    01-harness-desc.md -- 通用 AI 协作工程方法论（项目无关，可跨项目复用）
    02-harness-dev.md  -- Harness 开发流程（初始化、人机协作开发，人工维护）
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
{{项目名}}/
  Sources/             -- {{项目源码目录说明}}
    {{模块A}}/         -- {{模块A说明}}
    {{模块B}}/         -- {{模块B说明}}
    {{模块C}}/         -- {{模块C说明}}
  Resources/           -- {{资源目录说明}}
{{项目名}}Tests/       -- 单元测试
```

## 构建与测试

```bash
# {{构建命令说明}}
{{构建命令}}

# {{测试命令说明}}
{{测试命令}}

# {{环境配置说明（如有）}}
{{环境配置命令}}
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

- 日志内容禁用 emoji 图标、加粗、斜体等润色，使用普通文字
- 日志输出统一使用项目约定的日志框架，禁止使用语言原生打印（如 print / console.log / System.out 等）
- {{项目编码规范条目1}}
- {{项目编码规范条目2}}
- {{项目编码规范条目3}}

## 架构边界

- {{项目架构边界条目1，如：UI 层不直接调用底层 API，通过 Coordinator 或 Manager 中转}}
- {{项目架构边界条目2，如：全局状态管理通过统一入口控制，不在局部绕过}}
- {{项目架构边界条目3}}

## 质量守护

- 代码提交前必须通过构建命令，零警告，不允许遗留任何 Warning
- 新增或修改 Manager / Coordinator / Service 层逻辑时，应同步补充或更新单元测试
- 错误信息分两层：面向用户的提示使用自然语言、不含技术细节；面向开发者的日志使用项目日志框架，可包含错误码和上下文
- 日志中禁止输出 API Key、Access Key、Token 等敏感字段；如需标识密钥，仅输出末四位（如 `key=***abcd`）
- 网络请求必须设置超时，不允许无限等待
- 异步操作必须在非主线程执行，回调结果切回主线程更新 UI
- {{项目质量守护条目1}}
- {{项目质量守护条目2}}

## 安全规范

- 密钥只允许存储在安全存储中（如 Keychain / Vault / 环境变量），不得硬编码在源码中、不得写入明文配置、不得写入日志
- 包含密钥的配置文件禁止提交到版本库，必须加入 .gitignore
- 所有外部 API 调用必须使用 HTTPS，不得降级为 HTTP
- API 响应必须校验 HTTP 状态码和数据完整性，不信任未经校验的外部输入；解码失败时按错误处理，不静默忽略
- {{项目安全规范条目1}}
- {{项目安全规范条目2}}

## 上下文知识库

| 文件 | 何时查阅 |
|------|---------|
| .harness/context/users/01-prd-sense.md | 功能迭代前，确认产品定位、体验原则和判断准则 |
| .harness/context/agents/01-overview.md | 任何任务开始时，了解项目边界和入口 |
| .harness/context/agents/02-architecture.md | 涉及模块新增、依赖关系、跨层调用时 |
| .harness/context/agents/03-conventions.md | 涉及编码约定、质量约定、安全约定的实现细节时 |
| .harness/context/agents/04-glossary.md | 对项目术语不清楚时 |
| .harness/context/agents/05-data-boundaries.md | 涉及数据结构、存储格式、配置格式时 |
| .harness/context/agents/06-file-map.md | 确定功能对应哪些源文件时 |
| .harness/context/agents/07-key-patterns.md | 实现跨模块调用、状态管理、并发控制等通用模式时 |
| .harness/context/users/01-prd-baseline.md | 实现新功能或页面时，确认功能需求与产品约束 |
| .harness/context/users/01-prd-specs.md | 需要了解某功能的原始产品需求规格、或处理历史遗留逻辑时 |
| .harness/docs/02-harness-dev.md | 了解 Harness 开发流程（项目初始化、人机协作开发步骤）时 |
