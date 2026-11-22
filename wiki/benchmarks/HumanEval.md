---
type: benchmark
title: "HumanEval"
aliases: ["HumanEval Benchmark", "OpenAI 程式碼評測集"]
category: "Code Generation"
introduced_by: "Chen et al., 2021 (Codex)"
metrics: ["pass@1", "pass@10", "pass@100"]
last_updated: 2026-11-22
---

# HumanEval

## 1. 評測簡介
HumanEval 由 OpenAI 釋出，包含 164 道原創 Python 程式設計題目，附帶嚴格的單元測試，是評估 LLM 程式碼合成與邏輯推理能力的黃金標準。

## 2. 橫向評比數據 (Leaderboard)

| 模型 / 架構 | pass@1 (%) | 論文來源 | 備註 |
| :--- | :---: | :--- | :--- |
| GPT-4 (Zero-shot Baseline) | 67.0% | [[Shinn2023-Reflexion]] | 無自省單次生成 |
| **Reflexion (GPT-4 + Self-Reflection)** | **91.0%** | [[Shinn2023-Reflexion]] | 多輪自我除錯與反思 |
