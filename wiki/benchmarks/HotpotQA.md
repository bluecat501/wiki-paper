---
type: benchmark
title: "HotpotQA"
aliases: ["HotpotQA Benchmark", "多跳問答評測"]
category: "Multi-hop Question Answering"
introduced_by: "Yang et al., 2018"
metrics: ["Exact Match (EM)", "F1 Score"]
last_updated: 2026-11-22
---

# HotpotQA

## 1. 評測簡介
HotpotQA 是衡量模型**多跳推理（Multi-hop Reasoning）**能力的經典問答數據集。模型無法僅憑單一維基百科頁面回答問題，必須檢索跨越 2 個以上的互聯文檔並整合線索。

## 2. 橫向評比數據 (Leaderboard)

| 模型 / 方法 | Exact Match (EM %) | F1 (%) | 論文來源 | 備註 |
| :--- | :---: | :---: | :--- | :--- |
| PaLM-540B (Standard Prompt) | 29.4 | 38.6 | [[Yao2022-ReAct]] | 無推理步驟 |
| PaLM-540B (CoT) | 33.7 | 44.1 | [[Yao2022-ReAct]] | 內部推導，易有幻覺 |
| PaLM-540B (Act-only) | 25.7 | 34.2 | [[Yao2022-ReAct]] | 純搜尋無規劃 |
| **PaLM-540B (ReAct)** | **27.4** | **36.2** | [[Yao2022-ReAct]] | 低幻覺率 |
| **PaLM-540B (ReAct + CoT)** | **35.1** | **45.6** | [[Yao2022-ReAct]] | 混合機制 SOTA |
