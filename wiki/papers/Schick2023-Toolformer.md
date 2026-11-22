---
type: paper
title: "Toolformer: Language Models Can Teach Themselves to Use Tools"
authors: ["Timo Schick", "Jane Dwivedi-Yu", "Roberto Dessì", "Roberta Raileanu", "Maria Lomeli", "Luke Zettlemoyer", "Nicola Cancedda", "Thomas Scialom"]
year: 2023
venue: "NeurIPS 2023"
arxiv_id: "2302.04761"
doi: "10.48550/arXiv.2302.04761"
code_url: "https://github.com/conceptofmind/toolformer"
raw_file: "raw/papers/Schick2023-Toolformer.pdf"
tags:
  - paper
  - agent/tool-use
  - self-supervised
  - fine-tuning
concepts:
  - "[[Toolformer]]"
  - "[[External Tool Use]]"
  - "[[Self-Supervised Learning]]"
benchmarks:
  - "[[ASDiv]]"
  - "[[SVAMP]]"
  - "[[LAMA]]"
ingested_date: 2026-11-22
---

# Toolformer: Language Models Can Teach Themselves to Use Tools

## 1. 核心動機與欲解決痛點 (Motivation & Problem)

儘管大型語言模型（LLM）展現出驚人的語言生成能力，但在以下幾類任務上天生存在結構性缺陷：
1. **精確數學運算能力薄弱**（缺乏算術單元，容易算錯數字）。
2. **缺乏即時世界知識與時間感知**（訓練完後知識即凍結在特定時間點）。
3. **事實檢索幻覺問題**（在冷門長尾實體上產生虛構內容）。

在 [[Yao2022-ReAct]] 等早期工作中，模型使用工具主要依賴 **In-Context Few-shot Prompting**。然而這種方法存在嚴重痛點：
- **Context 成本昂貴**：需要塞入大量冗長範例示範如何使用工具。
- **欠缺自主決策邊界**：模型無法根據問題難度自主權衡「何時該用工具」、「何時自己回答即可」，常造成非必要的外部 API 浪費或反而在簡單問題上呼叫錯誤。
- **依賴大量人工標註**：傳統為工具調用準備監督式微調（SFT）資料集需要耗費大量人工編寫 API 呼叫樣本。

**Toolformer 的核心動機**：提出一種**完全自監督（Self-Supervised）**的學習架構，讓語言模型透過自採樣、真實 API 執行與 Loss 增益過濾，**自己教會自己使用各種外部工具**，並將工具調用能力無縫融合至自回歸語言生成的底層權重中。

---

## 2. 核心方法與數學架構 (Method & Mathematical Formulation)

Toolformer 的自監督流程可拆解為四個嚴謹步驟：

```mermaid
flowchart LR
    A[無標註純文本庫] --> B[1. 啟發式採樣 API 調用候選]
    B --> C[2. 外部 API 實際執行取得結果]
    C --> D[3. 基於 Loss 增益之過濾判準]
    D --> E[4. 標準語言模型微調 (Fine-tuning)]
```

### 2.1 候選 API 調用採樣 (Sampling API Calls)
給定文本序列 $x = (x_1, \dots, x_n)$，模型首先透過少量 Prompt 範例，計算在位置 $i$ 插入特定 API 調用候選 $c_i = (a_i, r_i)$ 的機率，其中 $a_i$ 為 API 呼叫指令，格式為 `<API>api_name(input)</API>`，由外部執行後得到結果 $r_i$。

### 2.2 核心過濾準則：Loss 增益判準 (Loss-based Filtering)
模型是否應該在位置 $i$ 呼叫 API，取決於**該 API 呼叫是否能幫助模型更好地預測後續文本**。

定義加權交叉熵損失（Cross-Entropy Loss）：
$$L_i(\mathbf{z}) = -\sum_{j=i}^n w_{j-i} \cdot \log p(x_j \mid x_{1:j-1}, \mathbf{z})$$
其中 $w_{j-i}$ 為隨距離衰減的權重。

作者計算兩種情況的 Loss 差異：
1. **$L_i(e(c_i))$**：提供完整的 API 呼叫與執行結果 $r_i$（其中 $e(c_i)$ 表示將 `<API>a_i \rightarrow r_i</API>` 插入至文本）。
2. **$L_i(\varepsilon)$ 與 $L_i(e(a_i, \varepsilon))$**：不提供 API 或只提供呼叫但不給結果。

**過濾閾值（Core Criterion）**：
只有當 API 的結果能夠顯著降低後續文本的預測困惑度時，才保留此樣本：
$$L_i(\varepsilon) - \min\left(L_i(e(c_i)), L_i(e(a_i, \varepsilon))\right) \ge \tau_f$$
此條件保證了：只有「真正有助於解答後續內容」的 API 呼叫才會被納入訓練集。

### 2.3 微調與推論 (Fine-tuning & Inference)
將篩選過後帶有 API 調用標籤的高品質合成數據與原始文本混合，在 GPT-J (6.7B) 上進行因果語言模型（CLM）微調。
在推論時期，當模型自主輸出 `<API>` Token 時，推論引擎暫停生成，調用外部程式取得結果填入 `</API>` 後，再繼續解碼。

---

## 3. 實驗設計與關鍵結果 (Experiments & Results)

Toolformer 在多個需要特定工具的領域進行評測：

### 3.1 數學推理評測 (ASDiv & SVAMP)
評測調用**計算機（Calculator）**的成效：

| 模型 | 參數規模 | ASDiv (Accuracy %) | SVAMP (Accuracy %) |
| :--- | :---: | :---: | :---: |
| GPT-3 (text-davinci-002) | 175B | 40.3 | 37.8 |
| GPT-J (Base) | 6.7B | 18.2 | 19.3 |
| **Toolformer (GPT-J + Tools)** | **6.7B** | **40.4** | **42.1** |
| Toolformer (禁用 API 調用) | 6.7B | 18.0 | 19.5 |

> **驚人成果**：僅僅 **6.7B** 的 Toolformer，在算術評測中不僅將自身基準線翻倍，更全面超越了參數量大其近 30 倍的 **GPT-3 (175B)**！

### 3.2 事實問答評測 (LAMA Benchmark)
調用**維基百科搜尋 API**：
- 在 LAMA (Date) 任務上，Toolformer 正確率從 baseline 的 26.3% 暴升至 **53.5%**。

---

## 4. 消融實驗與關鍵洞察 (Ablation Study & Insights)

1. **自監督過濾是成功的靈魂**：
   - 若不經過 Loss 增益過濾，盲目微調模型調用 API，模型會學會在完全不需要計算機的地方胡亂調用（如將「西元 1990 年」調用計算機計算），導致常規語言建模能力大幅崩塌。
2. **通用語言能力未受損 (No Alignment Tax)**：
   - 在標準 WikiText 困惑度評測中，微調後的 Toolformer 與原始 GPT-J 保持相當的語言建模水準，說明自主工具學習未損害基礎語言能力。

---

## 5. 局限性與未來方向 (Limitations & Future Work)

1. **缺乏多工具鏈式調用 (Tool Chaining)**：
   - Toolformer 只能在單一生成點調用單一工具，無法像 [[Yao2022-ReAct]] 那樣根據工具 A 的輸出決定調用工具 B。
2. **無法處理互動式環境**：
   - 僅適用於「請求-即時回應」型 API，無法直接應對需要維持多輪狀態的環境（如模擬遊戲、資料庫交易）。

---

## 6. 相關文獻與關聯節點 (Connections & Graph)

- **前驅與對比**：
  - [[Yao2022-ReAct]]：Prompt 階段的工具調用框架；Toolformer 則將工具意識寫入模型參數本身。
- **後續演進**：
  - **Gorilla** (Patil et al., 2023)：專門微調百萬規模 API 調用的模型。
  - **Function Calling**：OpenAI / Anthropic 將 Toolformer 的精神轉化為工業級標準 API 功能。
- **核心概念關聯**：
  - [[External Tool Use]]
  - [[Toolformer]]
