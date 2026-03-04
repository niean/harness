# Harness 工程文档清单

本目录是 AI 协作基础设施的统一目录，与应用代码分离。以下列出所有文件的路径、用途和维护方。

---

## 目录结构

```
AGENTS.md                          -- AI Agent 唯一入口（根目录）
.harness/
  README.md                        -- 本文件，文档清单
  skills/                          -- AI 可复用操作定义
    feature-iterate.md             -- 功能迭代
    build-verify.md                -- 构建验证
    backfill-knowledge.md          -- 回填-知识库更新
    backfill-prd-baseline.md       -- 回填-产品基线更新
    backfill-prd-sense.md          -- 回填-产品SENSE更新
    backfill-agents.md             -- 回填-AGENTS更新
    code-quality-scan.md           -- 代码质量扫描
    dead-code-cleanup.md           -- 废弃代码清理
    extract-skill.md               -- 提取-Skill
    extract-subagent.md            -- 提取-Subagent
    governance-review.md           -- 治理巡检
    task-summary.md                -- 任务总结
  subagents/                       -- Subagent prompt 模板
    （按项目需要创建，文件名 kebab-case）
  docs/                            -- 人工维护的方法论与参考文档
    00-harness-ops.md              -- Harness 项目维护（操作入口）
    01-harness-desc.md             -- 通用 AI 协作工程方法论
    02-dev-summary.md              -- 开发总结
  context/
    agents/                        -- AI 知识库（AI 可读写）
      01-overview.md               -- 项目概览
      02-architecture.md           -- 架构与模块边界
      03-conventions.md            -- 约定与约束（实现细节）
      04-glossary.md               -- 术语表
      05-data-boundaries.md        -- 数据与类型边界
      06-file-map.md               -- 功能与文件映射
      07-key-patterns.md           -- 关键代码模式
    users/                         -- 人工定义的原始信息（AI 只读）
      01-prd-sense.md              -- 产品定位、体验原则与判断准则
      01-prd-baseline.md           -- 稳定的产品需求基线
      01-prd-specs.md              -- 原始产品需求规格与演进记录
```

---

## 文件说明

### 入口文件

| 文件 | 用途 | 维护方 |
|------|------|--------|
| AGENTS.md | AI Agent 唯一入口，声明项目背景、通用规范、项目规范、Skills注册、知识库索引 | AI 维护，人工审批 |

### Skills（.harness/skills/）

可复用的 AI 操作单元，每个文件定义一个 Skill 的触发条件和执行步骤。

| 文件 | 用途 | 触发方式 |
|------|------|---------|
| feature-iterate.md | 接收需求、读取上下文、落盘临时spec、编码、回填知识、删除临时spec | 人工下发功能需求 |
| build-verify.md | 执行编译和测试，确认零警告零错误 | 功能迭代完成后自动执行，或人工指令 |
| backfill-knowledge.md | 对比实际代码与知识库文档，修正过时描述、压缩冗余内容 | 人工指令 |
| backfill-prd-baseline.md | 同步需求规格(specs)与需求基线(baseline) | 人工指令 |
| backfill-prd-sense.md | 从知识库和近期变更中提取产品判断准则变化，经人工确认后更新prd-sense | 人工指令 |
| backfill-agents.md | 从知识库和实际代码中提取变化，经人工确认后更新AGENTS.md | 人工指令 |
| code-quality-scan.md | 按规范并行扫描源码，输出违规清单 | 人工指令 |
| dead-code-cleanup.md | 识别未使用的代码和资源，确认后删除 | 人工指令 |
| extract-skill.md | 从近期编码活动中发现可复用操作模式，抽象为新Skill | 人工指令 |
| extract-subagent.md | 从近期编码活动中发现适合并行化的检查任务，抽象为新Subagent模板 | 人工指令 |
| governance-review.md | 编排多个Skill按顺序执行一轮完整治理（质量扫描、废弃清理、构建验证、知识回填、AGENTS校正） | 人工指令 |
| task-summary.md | 每次任务完成后输出总结报告（意图、步骤、结果、合规、资源消耗） | AI自动触发 |

### Subagents（.harness/subagents/）

并行子任务的 prompt 模板，由 Skills（如代码质量扫描）调用。按项目需要创建，每个文件定义一个扫描维度的任务、范围、检查规则和输出格式。

### Docs（.harness/docs/）

人工维护的方法论和参考文档，AI 修改前必须经过人工确认。

| 文件 | 用途 | 维护方 |
|------|------|--------|
| 00-harness-ops.md | Harness项目维护操作入口（治理操作、架构维护等人工操作指引） | 人工 |
| 01-harness-desc.md | 通用AI协作工程方法论（项目无关，可跨项目复用） | 人工 |
| 02-dev-summary.md | 开发总结 | 人工 |

### AI 知识库（.harness/context/agents/）

AI 可自主更新的项目知识，随项目演进持续维护。

| 文件 | 用途 | 何时查阅 |
|------|------|---------|
| 01-overview.md | 项目概览（技术栈、入口、核心流程） | 任何任务开始时 |
| 02-architecture.md | 架构与模块边界（分层、依赖关系） | 涉及模块新增、跨层调用时 |
| 03-conventions.md | 约定与约束实现细节（编码、质量、安全的权威定义） | 涉及实现细节时 |
| 04-glossary.md | 术语表 | 对术语不清楚时 |
| 05-data-boundaries.md | 数据与类型边界（数据结构、磁盘存储、配置格式） | 涉及数据结构变更时 |
| 06-file-map.md | 功能与文件映射 | 确定功能对应哪些源文件时 |
| 07-key-patterns.md | 关键代码模式（跨模块交互、核心流程实现） | 实现关键流程时 |

### 人工定义信息（.harness/context/users/）

人工编写的源头信息，AI 只读不写。如遇与 agents/ 内容冲突，必须提示人工确认。

| 文件 | 用途 | 何时查阅 |
|------|------|---------|
| 01-prd-sense.md | 产品定位、体验原则与判断准则 | 功能迭代前 |
| 01-prd-baseline.md | 稳定的产品需求基线 | 实现新功能时 |
| 01-prd-specs.md | 原始产品需求规格与演进记录 | 了解需求历史时 |

---

## 新项目接入

将本模板应用到新项目时，按以下步骤操作：

1. 复制 AGENTS.md 到项目根目录，替换 {{占位符}} 为项目实际信息
2. 复制 .harness/ 目录到项目根目录
3. 在 .harness/context/agents/ 下按需创建知识库文档，填入项目实际内容
4. 在 .harness/context/users/ 下放入产品需求等人工文档
5. 在 .harness/subagents/ 下按项目需要创建扫描维度的 prompt 模板
6. 根据项目构建工具，修改 build-verify.md 中的构建和测试命令
7. 根据项目代码规范，修改 code-quality-scan.md 中的扫描维度和 subagent 引用
8. 在 AGENTS.md 的 Skills 表中注册所有 Skill，在知识库索引表中注册所有文档

---

## 维护原则

- 每类知识有且只有一个归属文档，不重复维护
- 知识库是活文档，代码变更后同步回填
- agents/ 目录 AI 可自主更新，users/ 目录 AI 只读
- 所有文件使用 UTF-8 编码，LF 换行
- 文件名使用小写英文 kebab-case
- 文档内容禁用 emoji、加粗、斜体等润色格式
