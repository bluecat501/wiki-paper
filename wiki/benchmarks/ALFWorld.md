---
type: benchmark
title: "ALFWorld"
aliases: ["ALFWorld Benchmark", "家庭具身互動評測"]
category: "Embodied Decision Making"
introduced_by: "Shridhar et al., 2020"
metrics: ["Success Rate (%)"]
last_updated: 2026-11-22
---

# ALFWorld

## 1. 評測簡介
ALFWorld 是一個基於文字的虛擬具身環境（基於 TextWorld 與 ALFRED），模擬在家庭環境中執行高階指令（如「把洗乾淨的蘋果放到冰箱裡」）。模型必須在模擬空間中進行移動、拾取、清洗、加熱等多步驟決策。

## 2. 橫向評比數據 (Leaderboard)

| 模型 / 架構 | 任務成功率 (Success Rate %) | 論文來源 | 備註 |
| :--- | :---: | :--- | :--- |
| PaLM-540B (Act-only) | 45.0% | [[Yao2022-ReAct]] | 容易卡在重複動作或找不到物品 |
| **PaLM-540B (ReAct)** | **71.0%** | [[Yao2022-ReAct]] | 透過 Thought 進行長程規劃，大幅超越 Act-only |
| Reflexion (GPT-4 + Self-Reflection) | **97.0%** | [[Shinn2023-Reflexion]] | 引入記憶反思後逼近完美達成率 |
