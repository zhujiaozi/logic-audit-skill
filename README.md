# Logic Audit Skill · 逻辑审计

[![License](https://img.shields.io/badge/License-MIT-111111?style=flat-square)](LICENSE)
[![Skill](https://img.shields.io/badge/Skill-Agent-111111?style=flat-square)](SKILL.md)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Supported-6B5B95?style=flat-square)](https://claude.ai)

一个适配 Claude Code / Codex 等 Agent 环境的逻辑审计技能，用于对推理过程进行**系统性深度审查**。

涵盖 13 类逻辑谬误的检测，支持带编号推理链的自动解析、严重度分级、修正链生成。

> 由 [归藏](https://x.com/op7418) 的 guizang-ppt-skill 启发，沉淀于日常逻辑训练场景。

---

## 30 秒安装

```bash
npx skills add https://github.com/zhujiaozi/logic-audit-skill --skill logic-audit
```

或者直接对 AI Agent 说：

> 帮我把 `https://github.com/zhujiaozi/logic-audit-skill` 克隆到 `~/.claude/skills/logic-audit`

安装后重启对话，直接说 **"审计一下这段推理"** 即可触发。

---

## 什么时候用

### 适合触发
- "帮我审计这段推理"
- "检查逻辑漏洞"
- "查一下有没有逻辑错误"
- "修正这个思考链"
- 用户给出了带编号或不带编号的多步推理、论证、因果分析、数学推导、决策链

### 不需要触发
- 日常对话、单步问答
- 无显式推理链的简单请求
- 由 System Prompt 三步自检覆盖的轻量场景

---

## 检测能力一览

技能覆盖 **13 类逻辑问题**，每条发现标注严重度：

| 🔴 严重 | 🟡 可疑 | ⚪ 轻微 |
|---------|---------|---------|
| 推理直接崩溃 | 推理存疑 | 小瑕疵 |

| # | 类别 | 简要说明 |
|---|------|---------|
| 1 | 前提真实性 | 虚假前提、无依据断言、隐藏假设 |
| 2 | 步骤完整性 | 跳过必要推导环节 |
| 3 | 因果混淆 | 相关 ≠ 因果、方向倒置、忽略混杂变量 |
| 4 | 循环论证 | 结论隐含在前提中 |
| 5 | 数值/计算错误 | 算术、统计、概率错误 |
| 6 | 以偏概全 | 小样本推普遍结论 |
| 7 | 非黑即白 | 忽略中间或替代选项 |
| 8 | 稻草人谬误 | 歪曲对方观点后攻击 |
| 9 | 诉诸情感/权威 | 情感渲染或无关权威代替论证 |
| 10 | 滑坡谬误 | 无充分证据的连锁极端后果 |
| 11 | 偷换概念 | 关键词在不同步骤中含义漂移 |
| 12 | 诉诸自然 | 将"自然"等同于"正确/安全" |
| 13 | 合成/分割谬误 | 部分属性推整体，或整体属性推部分 |

---

## 输出样例

```
### 审计报告
- 步骤 1 | 🟡 前提真实性 — "许多研究表明"无具体出处，无法验证
- 步骤 3 | 🔴 非黑即白 — "允许=允许分心"忽略了中间方案
- 步骤 7 | 🔴 以偏概全 — 23人×3天的样本不具代表性

### 修正后的思考链
1. ...
2. ...

### 附加建议
在引用研究数据时注明具体出处（作者、期刊、年份）
```

---

## 评估测试

技能附带 4 组评估用例（`evals/evals.json`），可用 `skill-creator` 运行 benchmark 验证效果：

- 用例 1：因果混淆 + 以偏概全 + 非黑即白（期望检出 3+ 条问题）
- 用例 2：正确推理（期望返回"未发现逻辑错误"）
- 用例 3：过短输入（期望拒绝审计）
- 用例 4：含数值计算错误的混合推理（期望检出 🔴 严重错误）

---

## 项目结构

```
~/.claude/skills/logic-audit/
├── SKILL.md          # 技能核心定义（frontmatter + 审计清单 + 执行流程）
├── README.md         # 本文件
├── LICENSE           # MIT 许可证
└── evals/
    └── evals.json    # 评估用例
```

---

## License

MIT © 2025
