---
type: concept
title: "Agent Memory Systems"
aliases: ["代理人記憶系統", "Memory Stream", "Working Memory", "Long-term Memory"]
tags:
  - concept
  - agent/architecture
  - memory
introduced_by: "[[Shinn2023-Reflexion]]"
last_updated: 2026-11-22
---

# Agent Memory Systems (代理人記憶系統)

## 1. 概念分類
AI Agent 的記憶體系在學術與工業界通常分為三大層級：
1. **工作記憶 (Working Memory)**：當前上下文視窗內的提示詞、當前對話歷史與即時 Observation（如 [[Yao2022-ReAct]]）。
2. **短期情境記憶 (Short-term Contextual Memory)**：跨 Trial 的滑動自省紀錄緩衝區（如 [[Shinn2023-Reflexion]] 中的 Memory Buffer）。
3. **長期檢索記憶 (Long-term Episodic & Semantic Memory)**：包含時效衰減、重要性權重與語義向量檢索的持久化記憶流（如 [[Park2023-GenerativeAgents]] 中的 Memory Stream）。
