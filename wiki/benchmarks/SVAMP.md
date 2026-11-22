---
type: benchmark
title: "SVAMP"
aliases: ["SVAMP Benchmark", "挑戰型小學數學應用題評測"]
category: "Mathematical Reasoning"
introduced_by: "Patel et al., 2021"
metrics: ["Accuracy (%)"]
last_updated: 2026-11-22
---

# SVAMP (Simple Variations on Arithmetic Word Problems)

## 1. 評測簡介
SVAMP 是一個測試模型對小學數學應用題強健性（Robustness）的挑戰性基準測試。透過對詞彙結構進行微調干擾，測試模型是否真正理解題目運算邏輯，而非單純背誦題目模式。

## 2. 橫向評比數據 (Leaderboard)

| 模型 / 架構 | 參數規模 | 準確率 (Accuracy %) | 論文來源 | 備註 |
| :--- | :---: | :---: | :--- | :--- |
| GPT-J (Base) | 6.7B | 19.3% | [[Schick2023-Toolformer]] | 基礎語言模型算術極差 |
| GPT-3 (text-davinci-002) | 175B | 37.8% | [[Schick2023-Toolformer]] | 大模型仍存在計算失誤 |
| **Toolformer (GPT-J + Tools)** | **6.7B** | **42.1%** | [[Schick2023-Toolformer]] | **超越 175B 模型**，自主調用計算機效果顯著 |
