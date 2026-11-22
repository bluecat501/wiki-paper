---
type: concept
title: "Chain-of-Thought"
aliases: ["CoT", "思維鏈", "逐步思考"]
tags:
  - concept
  - reasoning
  - prompt-engineering
introduced_by: "[[Wei2022-ChainOfThought]]"
last_updated: 2026-11-22
---

# Chain-of-Thought (CoT)

## 1. 概念定義

**Chain-of-Thought (CoT)** 是引導大型語言模型產出一連串中間自然語言推理步驟（Rationale）的提示與微調技術。透過「分步驟思考」，模型能顯著提升在多步驟算術、符號推理與邏輯分析任務上的表現。

## 2. 與 Agent 的關係與演進
- **純 CoT 的局限**：傳統 CoT 是**靜態封閉式**的推理，模型只能根據自身參數權重中的既有知識進行聯想，無法即時調用外部 API 或確認世界事實，極易產生事實幻覺。
- **進入 Agent 時代**：[[Yao2022-ReAct]] 將 CoT 的內部思維機制整合至環境互動中，轉化為 `Thought-Action-Observation` 循環中的 `Thought` 模組，成為現代 AI Agent 進行目標拆解與長程規劃的核心引擎。
