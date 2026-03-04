# Skill: 代码质量扫描

触发：人工指令。

## 步骤

### Step 1 — 读取规范

读取 AGENTS.md 中的代码生成、质量守护、安全规范，提取检查规则。

### Step 2 — 并行扫描

使用 `use_subagents` 并行启动多个 subagent，每个聚焦一个检查维度。读取 `.harness/subagents/` 目录下对应 prompt 模板文件的内容，作为各 subagent 的 prompt 参数。

| Subagent | 维度 | prompt 模板 |
|----------|------|------------|
| {{序号}} | {{扫描维度名称}} | .harness/subagents/{{subagent文件名}}.md |

### Step 3 — 汇总报告

主 agent 汇总所有 subagent 的结果，合并为统一违规清单，按严重程度排序，输出最终报告。

### Step 4 — 等待确认并修复

通过 ask_followup_question 工具向用户展示违规清单（违规内容必须写在 question 参数中，不能写在工具调用之前的文本里，否则用户看不到），等待用户确认哪些需要修复。按用户确认的清单逐项修复代码。
