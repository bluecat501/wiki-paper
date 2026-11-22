---
type: paper
title: "Reflexion: Language Agents with Verbal Reinforcement Learning"
authors: ["Noah Shinn", "Federico Cassano", "Edward Berman", "Ashwin Gopinath", "Karthik Narasimhan", "Shunyu Yao"]
year: 2023
venue: "NeurIPS 2023"
arxiv_id: "2303.11366"
doi: "10.48550/arXiv.2303.11366"
code_url: "https://github.com/noahshinn/reflexion"
raw_file: "raw/papers/Shinn2023-Reflexion.pdf"
tags:
  - paper
  - agent/architecture
  - memory
  - self-reflection
  - reinforcement-learning
concepts:
  - "[[Reflexion]]"
  - "[[ReAct]]"
  - "[[Self-Reflection]]"
  - "[[Agent Memory Systems]]"
benchmarks:
  - "[[HumanEval]]"
  - "[[ALFWorld]]"
  - "[[HotpotQA]]"
ingested_date: 2026-11-22
---

# Reflexion: Language Agents with Verbal Reinforcement Learning

## 1. 核心動機與欲解決痛點 (Motivation & Problem)

儘管基於提示工程的代理人架構（如 [[Yao2022-ReAct]]）能夠透過「思考-行動-觀察」解決複雜問題，但仍存在兩大致命痛點：
1. **執行死循環與重複犯錯 (Execution Looping)**：
   - 當代理人在某個步驟失敗（例如查詢無效關鍵詞、產出語法錯誤的程式碼），它往往會在當前上下文內重複相同的無效嘗試，無法自我跳脫思維盲區。
2. **傳統強化學習（RL）的權重更新代價過高**：
   - 傳統 Policy Gradient 或 Q-Learning 透過純標量獎勵（Scalar Reward, 如 +1/-1）更新模型梯度。這需要數以萬計的昂貴互動樣本，且純標量回饋「缺乏語義資訊」，無法明確告訴模型「到底錯在哪裡、該如何修改」。

**Reflexion 的核心動機**：借鑑人類在試錯中的「反省與寫日記」機制，提出**語言強化學習（Verbal Reinforcement Learning）**。透過將純數值回饋轉化為豐富的「自然語言自我反思」，並將其存儲於滑動記憶視窗中，讓模型在**完全不更新神經網絡權重**的前提下，透過迭代 Trial 實現快速自我進化與除錯。

---

## 2. 核心方法與數學架構 (Method & Mathematical Formulation)

Reflexion 系統架構由三大核心組件與一個記憶緩存區構成：

```mermaid
flowchart TD
    Env[外部環境 / 程式直譯器] -->|環境回饋 $o_t$| Actor[Actor 行動者 (基於 ReAct)]
    Actor -->|執行動作 $a_t$ / 軌跡 $\tau$| Env
    Actor -->|完整軌跡 $\tau_t$| Evaluator[Evaluator 評估者]
    Evaluator -->|標量獎勵 $r_t$ (Pass/Fail)| SelfReflect[Self-Reflection 自我反思模型]
    SelfReflect -->|生成語言反思回饋 $sr_t$| MemBuffer[(Memory Buffer 記憶緩存區)]
    MemBuffer -->|在下一次 Trial 注入上下文| Actor
```

### 2.1 系統三大組件形式化定義
1. **Actor（行動者 $\pi_\theta$）**：
   - 生成行為軌跡 $\tau_t = (s_0, a_0, o_0, \dots, a_n, o_n)$。
   - Actor 採用 ReAct 範式，在給定任務輸入 $x$ 與反思記憶 $\mathcal{M}$ 下採樣動作：
     $$a_t \sim \pi_\theta(a_t \mid h_t, \mathcal{M})$$
2. **Evaluator（評估者 $\mathcal{E}$）**：
   - 審查 Actor 的完整輸出軌跡 $\tau_t$，計算標量績效指標 $r_t = \mathcal{E}(\tau_t) \in [0, 1]$。
   - 例如在程式設計任務中，執行單元測試；在問答任務中，檢查答案精確度。
3. **Self-Reflection（自我反思模型 $\mathcal{R}$）**：
   - 當 $r_t < 1$（任務未成功）時觸發。$\mathcal{R}$ 接收輸入 $x$、失敗軌跡 $\tau_t$ 與評估報告，生成一段口語化、指涉明確的自省文字：
     $$sr_t = \mathcal{R}(x, \tau_t, r_t)$$
   - *反思文字範例*：「在第 3 步中，我假設了輸入列表是非空的，導致 ZeroDivisionError。在下一次嘗試中，我必須在第 1 步先加入邊界條件檢查 `if not arr: return 0`。」

### 2.2 記憶緩存與試驗推進機制 (Memory Buffer & Trial Loop)
記憶緩存 $\mathcal{M}$ 儲存最近 $k$ 次失敗的自省文字：
$$\mathcal{M}_{i+1} = \mathcal{M}_i \cup \{sr_i\}$$
在進入第 $i+1$ 次 Trial 時，歷史自省經驗直接拼接在 Prompt 最前緣，成為強約束先驗。若當前嘗試成功（$r_t = 1$）或達到最大 Trial 上限，迴圈終止。

---

## 3. 實驗設計與關鍵結果 (Experiments & Results)

Reflexion 在多個高難度決策與推理基準上展現出驚人的躍升：

### 3.1 程式碼生成：[[HumanEval]] (Python Benchmark)
評測使用 GPT-4 進行自我反思循環的代碼修復表現：

| 模型 / 嘗試輪次 | pass@1 (%) | 備註 |
| :--- | :---: | :--- |
| GPT-4 (Zero-shot Baseline) | 67.0% | 單次直接生成 |
| Reflexion Trial 1 (初次反思) | 79.3% | 第一次單元測試反思除錯 |
| Reflexion Trial 2 (二次反思) | 88.4% | 第二次反思修正邊界條件 |
| **Reflexion Trial 4 (終局完成)** | **91.0%** | **比 Zero-shot 提升 24 個百分點** |

### 3.2 具身家庭互動：[[ALFWorld]]
- **Baseline ReAct** 成功率：**71.0%**
- **Reflexion (ReAct + Self-Reflection)** 成功率：**97.0%**（在家庭環境中幾乎能克服所有卡關狀態）

### 3.3 知識問答：[[HotpotQA]]
- 正確率從 ReAct 的 34% 提升至 **54%**。

---

## 4. 消融實驗與關鍵洞察 (Ablation Study & Insights)

1. **語言回饋遠勝純標量回饋**：
   - 若僅向模型回傳「錯誤 (Fail)」，模型只能瞎猜；而具體的自然語言回饋（告訴它在哪個變數上犯錯）為後續推理提供了即時的梯度方向。
2. **短期記憶 vs 長期記憶**：
   - 記憶容量 $k=1$ 到 $k=3$ 提升顯著；超過 3 次反思後邊際效應遞減，並可能因上下文過長帶來資訊干擾。

---

## 5. 局限性與未來方向 (Limitations & Future Work)

1. **幻覺反思 (Hallucinatory Reflection)**：
   - 當底層 LLM 能力不足時，模型可能對錯誤原因做出錯誤的「偽自省」（例如本來程式沒錯卻改壞其他部分）。
2. **跨 Session 記憶持久化不足**：
   - Reflexion 的記憶仍是臨時性的 Working Memory，缺乏像 [[Park2023-GenerativeAgents]] 那樣的分層檢索式長期記憶體系。

---

## 6. 相關文獻與關聯節點 (Connections & Graph)

- **直接繼承**：
  - [[Yao2022-ReAct]]：Reflexion 的 Actor 模組直接採用 ReAct 範式。
- **後續演化**：
  - [[Park2023-GenerativeAgents]]：進一步將記憶深化為 Memory Stream，結合檢索（Retrieval）、重要性衰減與歸納思考。
  - **Self-Refine** (Madaan et al., 2023)：無外部評估者的自省式文本改進。
- **核心概念關聯**：
  - [[Reflexion]]
  - [[Self-Reflection]]
  - [[Agent Memory Systems]]
