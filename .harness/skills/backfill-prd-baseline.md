# Skill: 回填-产品基线更新

触发：人工指令。

步骤：
1. 读取 .harness/context/users/01-prd-specs.md 和 .harness/context/users/01-prd-baseline.md
2. 对比 specs 迭代记录与 baseline，找出 baseline 中缺失或过时的描述
3. 通过 ask_followup_question 工具向用户展示候选变更清单（候选内容必须写在 question 参数中，不能写在工具调用之前的文本里，否则用户看不到），等待用户确认
4. 按用户确认的内容，最小化修改 01-prd-baseline.md
5. 输出变更摘要
