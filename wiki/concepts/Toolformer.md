---
type: concept
title: "Toolformer"
aliases: ["Tool-Augmented Language Model", "自主工具學習模型"]
tags:
  - concept
  - agent/tool-use
  - self-supervised
introduced_by: "[[Schick2023-Toolformer]]"
last_updated: 2026-11-22
---

# Toolformer

## 1. 概念定義

**Toolformer** 是由 Meta AI 於 2023 年提出的「讓大型語言模型自主學習使用外部工具」的模型訓練範式。不同於傳統以 Few-shot Prompt 引導工具調用的方式（如 [[ReAct]]），Toolformer 透過**自監督學習（Self-Supervised Learning）**，使語言模型能夠自主判斷：
1. **何時該呼叫工具（When to call）**
2. **呼叫哪種工具（Which tool to call）**
3. **如何傳遞精確參數（What parameters to pass）**
4. **如何將工具回傳結果無縫整合進文本生成中（How to integrate outputs）**

## 2. 核心機制：Loss 增益判準
Toolformer 的本質突破在於設計了一套數學判準：只有當某個 API 呼叫的結果能**顯著降低後續文本生成的交叉熵損失（Cross-Entropy Loss）**時，模型才將該調用視為有益經驗並寫入訓練集：
$$L_{without\_API} - L_{with\_API} \ge \tau$$

## 3. 與 ReAct 的本質差異

| 維度 | ReAct ([[Yao2022-ReAct]]) | Toolformer ([[Schick2023-Toolformer]]) |
| :--- | :--- | :--- |
| **實現層級** | Prompt 提示工程（無需微調權重） | 模型參數層（微調神經網絡權重） |
| **上下文開銷** | 需保留多輪 Thought/Observation，開銷大 | 原生生成特殊 Token `<API>`，推論極輕量 |
| **工具組合性** | 支援多步長程鏈式調用（Chaining） | 單點即時調用，不擅長多工具複雜管線 |
| **互動模式** | 多輪互動迴圈 | 自回歸生成中的即時中斷與恢復 |
