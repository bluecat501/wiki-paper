---
type: concept
title: "Reflexion"
aliases: ["Reflexion Framework", "Verbal Reinforcement Learning", "語言強化學習", "自省架構"]
tags:
  - concept
  - agent/architecture
  - memory
  - self-reflection
introduced_by: "[[Shinn2023-Reflexion]]"
last_updated: 2026-11-22
---

# Reflexion

## 1. 概念定義

**Reflexion** 是一種透過**語言反思（Verbal Self-Reflection）**來增強自主代理人推理與行動決策能力的架構。由 MIT 與普林斯頓大學於 2023 年提出。

其核心洞見是：傳統強化學習（RL）依賴數值獎勵更新神經網絡權重，過程昂貴且缺乏可解釋性；Reflexion 則將失敗反饋轉化為具體的**自然語言文字診斷**，將其寫入工作記憶，並在下一次嘗試時直接注入 Context，達成「在不更新模型參數的情況下快速自我修正」。

## 2. 系統核心三元組
1. **Actor**：生成執行動作與思考（多基於 [[ReAct]] 範式）。
2. **Evaluator**：驗證任務成果（如測試案例、環境分數）。
3. **Self-Reflection**：當失敗時撰寫具體檢討日記，存入 Memory Buffer。
