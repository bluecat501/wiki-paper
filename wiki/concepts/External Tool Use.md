---
type: concept
title: "External Tool Use"
aliases: ["Tool Use", "Tool Calling", "工具調用", "外部工具使用"]
tags:
  - concept
  - agent/tool-use
  - api-integration
introduced_by: "[[Yao2022-ReAct]]"
last_updated: 2026-11-22
---

# External Tool Use (外部工具使用)

## 1. 概念定義

**External Tool Use（外部工具使用）** 指大型語言模型透過特定語法格式（如 JSON、專用 Token 標籤或正規表示式指令）產生可以被外部直譯器解析的命令，調用外部環境（計算機、搜尋引擎、Python REPL、資料庫或第三方 API），並接收返回值作為後續推論依據的能力。

## 2. 演進脈絡
1. **Prompt 引導式調用**：[[Yao2022-ReAct]] 最早透過 Few-shot Prompt 示範如何調用搜尋引擎與維基百科，開啟 Thought-Action-Observation 循環。
2. **模型自主內化微調**：[[Schick2023-Toolformer]] 透過基於 Loss 增益的自監督微調，使 6.7B 小模型在無需大量人工標註的情況下學會自主呼叫計算機與搜尋引擎，表現超越 175B 的 GPT-3。
3. **原生 Function Calling**：現代前沿模型（GPT-4、Claude 3.5、Gemini 1.5）直接在模型預訓練與後訓練階段對齊結構化工具調用標準。
