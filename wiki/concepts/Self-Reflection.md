---
type: concept
title: "Self-Reflection"
aliases: ["自我反思", "自我修正", "Self-Debugging"]
tags:
  - concept
  - agent/reasoning
  - feedback-loop
introduced_by: "[[Shinn2023-Reflexion]]"
last_updated: 2026-11-22
---

# Self-Reflection (自我反思)

## 1. 概念定義
**Self-Reflection** 是指大型語言模型代理人在執行任務受挫或面對環境負向反饋時，透過自我審查歷史執行步驟，產出診斷性語言分析並主動修正後續規劃的能力。

## 2. 演進脈絡
- **單輪內部自省**：早期提示（如 "Please review your previous answer"）。
- **外部驗證驅動之閉環自省**：[[Shinn2023-Reflexion]] 提出 Evaluator 觸發機制，將自省文字存儲為顯式先驗知識。
- **真實工程級自省**：在 [[Jimenez2024-SWEbench]] 評測中，優秀的程式代理人（如 SWE-agent）透過單元測試與 Git diff 進行多輪反思除錯。
