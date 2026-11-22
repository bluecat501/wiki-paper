---
type: paper
title: "ReAct: Synergizing Reasoning and Acting in Language Models"
authors: ["Shunyu Yao", "Jeffrey Zhao", "Dian Yu", "Nan Du", "Izhak Shafran", "Karthik Narasimhan", "Yuan Cao"]
year: 2022
venue: "ICLR 2023"
arxiv_id: "2210.03629"
doi: "10.48550/arXiv.2210.03629"
code_url: "https://github.com/ysymyx/ReAct"
raw_file: "raw/papers/Yao2022-ReAct.pdf"
tags:
  - paper
  - agent/architecture
  - reasoning
  - tool-use
concepts:
  - "[[ReAct]]"
  - "[[Chain-of-Thought]]"
  - "[[External Tool Use]]"
benchmarks:
  - "[[HotpotQA]]"
  - "[[Fever]]"
  - "[[ALFWorld]]"
ingested_date: 2026-11-22
---

# ReAct: Synergizing Reasoning and Acting in Language Models

## 1. 核心動機與欲解決痛點 (Motivation & Problem)

傳統大型語言模型（LLM）在解決複雜現實世界問題時，主要面臨兩種孤立的範式局限：
1. **純推理範式（Reasoning-only, 如 Chain-of-Thought, CoT）**：
   - 模型依賴內部參數記憶進行靜態邏輯推演。
   - **痛點**：模型缺乏與真實物理或數位環境的感知通道，在需要事實性或即時資訊的任務中極易產生嚴重「幻覺 (Hallucination)」，且一旦前期推理步驟發生偏差，錯誤將在後續步驟中被放大（Error Propagation）。
2. **純行動範式（Acting-only, 如 WebGPT 或純 API 行動調用）**：
   - 模型直接產出環境動作指令（如搜尋、翻頁、點擊）。
   - **痛點**：缺乏內省式推理與目標分解機制，無法進行長程規劃（Long-horizon Planning）。代理人在面對多步驟決策時容易淪為「盲目試錯」，難以處理需要多來源資訊整合的綜合問題。

**ReAct 的核心動機**：結合人類在面對任務時「邊思考、邊行動、邊觀察」的認知機制，在同一個執行軌跡（Trajectory）中將內部推理（Reasoning）與外部環境行動（Acting）有機結合，形成自我調節的封閉迴圈。

---

## 2. 核心方法與數學架構 (Method & Mathematical Formulation)

### 2.1 動作空間擴展 (Action Space Augmentation)
在傳統強化學習或決策環境中，環境在時間步 $t$ 給予觀察值 $o_t \in \mathcal{O}$，代理人選擇執行動作 $a_t \in \mathcal{A}$。

ReAct 將原始動作空間 $\mathcal{A}$ 擴展為包含「思考 (Thoughts)」的聯合動作空間：
$$\mathcal{A}_{ReAct} = \mathcal{A} \cup \mathcal{T}$$
其中 $\mathcal{T}$ 為語言思考空間（Thought Space）。
- 若 $a_t \in \mathcal{T}$，該動作是一個思考標記（Mental Act），**不改變外部環境狀態**，也不觸發外部環境反饋，其觀察值定義為空或內部緩存：$o_t = \emptyset$。思考的目的是更新代理人的內部上下文狀態，進行子目標分解、萃取已知資訊或規劃下一步。
- 若 $a_t \in \mathcal{A}$，該動作為外部物理/數位行動（Physical Act），會作用於外部環境，並產生真實的環境觀察值 $o_t \in \mathcal{O}$。

### 2.2 軌跡生成公式 (Trajectory Formulation)
給定任務輸入（上下文）$c$，ReAct 在時間步 $t$ 的軌跡定義為歷史序列：
$$h_t = (c, a_1, o_1, a_2, o_2, \dots, a_{t-1}, o_{t-1})$$
語言模型 $\pi_\theta$ 根據歷史軌跡條件機率分佈預測下一個 Token 序列：
$$a_t \sim \pi_\theta(a_t \mid h_t)$$

### 2.3 核心交替機制：Thought-Action-Observation
ReAct 透過 Few-shot In-context Learning 提示工程，引導模型按照嚴格的三階段交替執行：
```text
Question: [使用者輸入目標]
Thought 1: [分解問題，分析第一步需要尋找什麼關鍵實體]
Action 1: Search[實體名稱]
Observation 1: [外部環境/維基百科搜尋 API 返回之文字片段]
Thought 2: [分析 Observation 1，發現新線索，決定下一步查驗對象]
Action 2: Lookup[關鍵字]
Observation 2: [對應段落內容]
...
Thought k: [所有資訊齊備，進行邏輯整合得出結論]
Action k: Finish[最終答案]
```

---

## 3. 實驗設計與關鍵結果 (Experiments & Results)

作者在**知識密集型推理 (Knowledge-intensive Reasoning)** 與 **具身決策互動 (Decision-making Interaction)** 兩大維度進行嚴格評測：

### 3.1 評測基準與對比數據

| 任務領域 | 評測基準 (Benchmark) | 模型架構 | Standard Prompt | CoT (純推理) | Act-only (純行動) | **ReAct** | **ReAct + CoT (混合)** |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| 多跳問答 | **[[HotpotQA]]** (Exact Match %) | PaLM-540B | 29.4 | 33.7 | 25.7 | 27.4 | **35.1** |
| 事實查核 | **[[Fever]]** (Accuracy %) | PaLM-540B | 56.3 | 56.3 | 58.9 | 60.9 | **65.3** |
| 具身互動 | **[[ALFWorld]]** (Success Rate %) | PaLM-540B | - | - | 45.0 | **71.0** | - |
| 網頁導航 | **WebShop** (Success Rate %) | PaLM-540B | - | - | 35.5 | **40.0** | - |

- 在 **HotpotQA** 上，ReAct 單獨執行時雖然略有召回率折損，但當與 CoT 結合（CoT 失敗時後備切換至 ReAct，或互相投票）時，創下 **35.1%** 的新紀錄。
- 在 **ALFWorld** 具身互動中，ReAct 成功率從純行動的 45% 暴增至 **71%**，證明「動腦想過再動手」在具身決策環境中具有決定性優勢。

---

## 4. 消融實驗與關鍵洞察 (Ablation Study & Insights)

1. **錯誤型態對比 (Error Pattern Analysis)**：
   - **CoT 的致命傷是「虛構事實 (Fact Hallucination)」**：CoT 在問答任務中有高達 56% 的錯誤來自於無中生有的資訊。
   - **ReAct 的優勢是「自我糾正性 (Groundedness)」**：ReAct 絕大部分事實皆錨定在環境 Observation 中，幻覺率降至極低。
2. **ReAct 錯誤的主因**：
   - ReAct 的失敗主要來自於搜尋檢索不到目標資訊導致的「推理死循環（Looping/Search Fatigue）」，而非模型邏輯崩潰。
3. **可解釋性與人機協同 (Interpretability & Human-in-the-loop)**：
   - 由於所有 Thought 均以人類自然語言寫出，使用者可即時觀察 AI「當前在哪一步思考受阻」，並直接在 Observation 處進行人工干預（Human Steering），這是純端到端 RL 或黑箱神經網絡無法企及的特性。

---

## 5. 局限性與未來方向 (Limitations & Future Work)

1. **脈絡長度與推論成本 (Context Length & Cost)**：
   - 每一次 `Thought` 與 `Observation` 都會累積到上下文視窗中，長程任務極易耗盡 Context Window，並產生昂貴的 API 呼叫費用。
2. **缺乏長期跨情境記憶 (Lack of Long-term Memory)**：
   - ReAct 僅具備工作記憶（In-context Working Memory），任務結束後即遺失經驗，無法跨 Session 記住失敗教訓（後續由 [[Reflexion]] 解決）。
3. **工具依賴性與檢索彈性**：
   - 嚴重依賴外部環境返回資料的精準度，若環境 API 響應格式有誤或雜訊過高，推理鏈條易被誤導。

---

## 6. 相關文獻與關聯節點 (Connections & Graph)

- **前驅基礎**：
  - [[Chain-of-Thought]] (Wei et al., 2022)：提供了思維鏈提示的基礎框架，ReAct 將其從封閉空間拓展至開放環境。
- **後續演進與改進**：
  - [[Reflexion]] (Shinn et al., 2023)：在 ReAct 的基礎上引入語言回饋反思（Evaluator 與 Memory Buffer），解決了 ReAct 陷入重複錯誤的痛點。
  - [[Toolformer]] (Schick et al., 2023)：將外部 API 調用能力透過微調直接內化至模型權重中。
- **核心概念關聯**：
  - [[ReAct]] (核心概念節點)
  - [[External Tool Use]] (工具調用)
