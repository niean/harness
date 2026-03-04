# Harness 项目维护

## 约束
本文仅供自然人使用，未经人工确认、禁止AI修改。

## 蒸馏模板
任务：生成抽象Harness工程模板
输入：本项目的真实Harness工程相关内容
输出：项目无关的通用 Harness 工程模板

## 治理-自动执行
- 提示词：执行 `治理巡检`

## 治理-人工执行
- 维护技能
    - 提示词：扫描所有的Skills、Subagents，发现其中的问题，人工确认后进行修复
    - 提示词：执行 `提取-Skill`
    - 提示词：执行 `提取-Subagent`
- 回填知识
    - 提示词：执行 `回填-AGENTS更新`
    - 提示词：执行 `回填-知识库更新`
    - 提示词：执行 `回填-产品SENSE更新`
    - 提示词：执行 `回填-产品基线更新`
- 治理代码
    - 提示词：执行 `代码质量扫描`
    - 提示词：执行 `废弃代码清理`

## 架构-Skills
- 生成
    - 生成Skill：使用Skill `提取-Skill`，让AI自动提取新的Skill
    - 生成Skill：手动描述任务名、文件名、触发方式、步骤概要等，让AI生成Skill文件并注册到AGENTS.md
- 修改
    - 修改Skill：直接修改 .harness/skills/ 目录下的文件
    - 同步修改 AGENTS.md 中的 Skills 表

## 架构-Subagents
- 生成
    - 生成Subagent：使用Skill `提取-Subagent`，让AI自动提取新的Subagent
    - 生成Subagent：手动描述维度、扫描范围、检查规则等，让AI生成prompt模板文件
- 修改
    - 修改Subagent：直接修改 .harness/subagents/ 目录下的文件
    - 如有关联Skill，同步修改Skill中的subagent引用表
