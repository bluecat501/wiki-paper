---
type: benchmark
title: "SWE-bench"
aliases: ["SWE-bench Benchmark", "軟體工程評測基準"]
category: "Software Engineering / Autonomous Agents"
introduced_by: "[[Jimenez2024-SWEbench]]"
metrics: ["Resolved Rate (%)", "Patch Application Rate (%)"]
last_updated: 2026-11-22
---

# SWE-bench

## 1. 評測簡介
SWE-bench 是一個端到端評估 AI 代理人在真實世界開源 GitHub 倉庫中解決真實軟體工程 Issue 的評測基準。包含 2,294 個任務實例，必須透過 Docker 沙盒執行通過完整的 Fail-to-Pass 與 Pass-to-Pass 單元測試。

## 2. 橫向評比數據 (Leaderboard 演進史)

| 系統 / 模型 | 解決率 (Resolved Rate %) | 備註 |
| :--- | :---: | :--- |
| GPT-4 (Base Prompt) | 1.7% | 初始發表時（2023 年底） |
| Claude 2 (Base Prompt) | 4.8% | 初始發表時 |
| SWE-agent + GPT-4 | 12.5% | 引入 ACI 專用工具介面 |
| 現代前沿 Agent (2024+ SOTA) | **~40% - 50%+** | 結合樹狀搜尋、反思與多工具管線 |
