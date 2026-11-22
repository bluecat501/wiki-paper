# AGENTS.md - LLM Paper Wiki System Prompt & Operations Manual

本文件定義學術研究 **LLM Paper Wiki** 的系統架構、命名規範、工作流程與 Markdown 格式規範。
所有與本知識庫互動的 AI Agent 必須嚴格遵守以下所有準則。

---

## 1. 核心哲學 (Core Philosophy)

- **累積型知識資產 (Compounding Artifact)**：
  - 本 Wiki 不是靜態快取，更不是隨問隨查、不累積記憶的傳統 RAG。
  - 這是一座持續演進、高度互相連結的知識庫（Knowledge Codebase）。知識一旦被萃取，就會持續被交叉引用、修訂與深化。
- **人機分工 (Human Curation, Agent Maintenance)**：
  - **Human**：負責挑選論文來源、指引研究方向、提出核心問題與深度思考。
  - **Agent**：負責所有繁重的維護工作——閱讀論文、下載原始文獻、撰寫結構化摘要、維護雙向連結、更新概念與 Benchmark 頁面、保持索引最新。
- **Obsidian 原生 (Obsidian First)**：
  - `wiki/` 下所有檔案必須能在 Obsidian 中完美呈現，嚴格使用 `[[WikiLink]]` 語法，並具備符合 Dataview 規範的 YAML Frontmatter。
- **不可變來源 vs 可變知識庫**：
  - `raw/`：**不可變的真相來源 (Immutable Source of Truth)**。包含 PDF 檔案、原始截圖。Agent 只能寫入新增或讀取，絕不可擅自修改或刪除既有檔案。
  - `wiki/`：由 Agent 全權編譯與維護的知識層。
  - `assets/`：保留給未來 HTML / Slide 簡報使用的圖貼與視覺素材。

---

## 2. 目錄架構 (Directory Layout)

```
LLM-Wiki/
├── AGENTS.md                  # 本系統規範手冊 (Agent 核心工作守則)
├── index.md                   # 全域內容目錄索引 (Categorized Index)
├── log.md                     # 操作日誌 (時間軸紀錄)
├── raw/                       # 不可變原始文獻庫
│   └── papers/                # 存放 PDF 檔案、論文圖表截圖
├── wiki/                      # 編譯後的知識本體 (可變、持續更新)
│   ├── papers/                # 單篇論文精讀頁 (Paper Pages)
│   ├── concepts/              # 核心演算法、架構與方法論 (Concept Pages)
│   ├── benchmarks/            # 評測基準、任務與數據集 (Benchmark/Task Pages)
│   └── syntheses/             # 跨論文橫向對比、演進史與研究綜述
└── assets/                    # 簡報圖貼與視覺資源 (保留區)
```

---

## 3. 命名慣例與格式規範 (Conventions & Schemas)

### 3.1 語言與專有名詞原則
- **內文語言**：統一使用 **繁體中文（台灣習慣用語）** 進行分析、論述與摘要。
- **技術術語**：關鍵技術術語、演算法名稱、模型名稱、指標與論文原標題**一律保留英文原文**（例如：`Self-Attention`、`Ablation Study`、`Chain-of-Thought`、`Perplexity`、`Feed-Forward Network`）。
- **數學與演算法呈現**：
  - 遇到關鍵理論、Loss Function、最佳化目標或核心架構時，**必須保留數學推導與公式**。
  - 行內公式使用 `$ ... $`，獨立區塊公式使用 `$$ ... $$`。
  - 關鍵演算法流程應提供虛擬碼（Pseudocode）或結構化步驟說明。

### 3.2 檔案命名規範 (Naming Conventions)
1. **論文頁 (`wiki/papers/`)**：
   - 採用 **選項 A 格式**：`[FirstAuthorLastName][YYYY]-[ShortTitle].md`
   - 範例：`Vaswani2017-Transformer.md`、`Dao2022-FlashAttention.md`、`Rafailov2023-DPO.md`。
   - 對應的原始 PDF 儲存於 `raw/papers/`，檔名保持一致：`Vaswani2017-Transformer.pdf`。
2. **概念與方法頁 (`wiki/concepts/`)**：
   - 採用**英文專有名詞或主流縮寫**命名：`FlashAttention.md`、`Direct Preference Optimization.md`、`LoRA.md`。
   - 必須在 YAML `aliases` 填入常見縮寫與中文譯名，確保 Obsidian 在 `[[RLHF]]`、`[[人類回饋強化學習]]` 時皆可自動對應。
3. **基準測試與數據集頁 (`wiki/benchmarks/`)**：
   - 採用**評測名稱或縮寫**命名：`MMLU.md`、`GSM8K.md`、`HumanEval.md`。
4. **綜述與主題頁 (`wiki/syntheses/`)**：
   - 採用 `YYYY-MM-[topic-slug].md`：例如 `2024-11-LLM-Reasoning-Evolution.md`。

---

### 3.3 YAML Frontmatter 規範

#### A. 論文頁 Frontmatter (`wiki/papers/[Author][YYYY]-[Title].md`)
```yaml
---
type: paper
title: "Attention Is All You Need"
authors: ["Ashish Vaswani", "Noam Shazeer", "Niki Parmar", "et al."]
year: 2017
venue: "NeurIPS 2017"
arxiv_id: "1706.03762"
doi: "10.48550/arXiv.1706.03762"
code_url: "https://github.com/tensorflow/tensor2tensor"
raw_file: "raw/papers/Vaswani2017-Transformer.pdf"
tags:
  - paper
  - architecture/attention
  - nlp
concepts:
  - "[[Transformer]]"
  - "[[Self-Attention]]"
benchmarks:
  - "[[WMT2014-EnDe]]"
ingested_date: 2026-11-22
---
```

#### B. 概念頁 Frontmatter (`wiki/concepts/[ConceptName].md`)
```yaml
---
type: concept
title: "Direct Preference Optimization"
aliases: ["DPO", "直接偏好最佳化"]
tags:
  - concept
  - alignment/rlhf
introduced_by: "[[Rafailov2023-DPO]]"
last_updated: 2026-11-22
---
```

#### C. 評測基準頁 Frontmatter (`wiki/benchmarks/[BenchmarkName].md`)
```yaml
---
type: benchmark
title: "GSM8K"
aliases: ["Grade School Math 8K"]
category: "Mathematical Reasoning"
introduced_by: "[[Cobbe2021-GSM8K]]"
metrics: ["Accuracy@1", "Maj@1"]
last_updated: 2026-11-22
---
```

#### D. 綜述頁 Frontmatter (`wiki/syntheses/[Slug].md`)
```yaml
---
type: synthesis
title: "大語言模型對齊技術演進：從 PPO 到 DPO 與 ORPO"
tags:
  - synthesis
  - alignment
papers_referenced:
  - "[[Ouyang2022-InstructGPT]]"
  - "[[Rafailov2023-DPO]]"
last_updated: 2026-11-22
---
```

---

## 4. 標準作業流程 (SOP)

### 4.1 Ingest Workflow（全自動直接歸檔）
當使用者提供：
- 放在 `raw/papers/` 的 PDF 檔案
- 或對話中直接給予 PDF 檔案、arXiv 連結、DOI 或論文標題時觸發。

**執行步驟**：
1. **文獻存檔與讀取 (Acquire & Archive)**：
   - 若使用者提供的是 arXiv 連結或網絡文獻，Agent **必須先下載 PDF 檔案並存入 `raw/papers/[Author][YYYY]-[ShortTitle].pdf`**，確保原始文獻庫完整。
   - 解析論文內容：作者、年份、會議、核心研究動機、方法推導、數學公式、實驗結果與局限性。
2. **建立單篇論文筆記 (`wiki/papers/[Author][YYYY]-[ShortTitle].md`)**：
   - 包含固定章節：
     - `# {Title}`
     - `## 1. 核心動機與欲解決痛點 (Motivation & Problem)`
     - `## 2. 核心方法與數學架構 (Method & Mathematical Formulation)`（**必含關鍵公式與 Loss Function**）
     - `## 3. 實驗設計與關鍵結果 (Experiments & Results)`（含主要表格對比數據）
     - `## 4. 消融實驗與關鍵洞察 (Ablation Study & Insights)`
     - `## 5. 局限性與未來方向 (Limitations & Future Work)`
     - `## 6. 相關文獻與關聯節點 (Connections & Graph)`（使用 `[[WikiLink]]` 連入概念與評測頁）
3. **維護概念頁 (`wiki/concepts/`)**：
   - 檢查論文涉及的核心演算法或理論是否已有概念頁。
   - 若無：建立新概念頁，填寫簡介、核心數學公式，並標明 `introduced_by`。
   - 若有：更新該頁面，補充本論文如何應用、擴展或反駁前人結論（追蹤演進歷程）。
4. **維護評測基準頁 (`wiki/benchmarks/`)**：
   - 若論文提出新 Benchmark 或在主流 Benchmark（如 MMLU, GSM8K, HumanEval）上公佈分數，自動更新該評測頁面的橫向比較表格。
5. **更新全域索引 (`index.md`)**：
   - 在論文目錄、概念目錄與評測目錄中加入新節點連結與一行中文精要。
6. **追加操作日誌 (`log.md`)**：
   - 依標準格式追加：
     ```markdown
     ## [YYYY-MM-DD] ingest | [Author][YYYY]-[ShortTitle]
     - **Paper Page**: [[AuthorYYYY-ShortTitle]]
     - **Raw PDF**: raw/papers/AuthorYYYY-ShortTitle.pdf
     - **Updated Concepts**: [[Concept1]], [[Concept2]]
     - **Updated Benchmarks**: [[Benchmark1]]
     ```
7. **向使用者回報**：
   - 輸出簡短的 Executive Summary（3-5 點核心發現），並條列本次建立與修改的檔案清單。

---

### 4.2 Query & Compound Workflow（查詢與知識沉澱）
當使用者針對知識庫提問（例如：「比較 DPO 和 PPO 在訓練穩定度與顯存上的差異？」）：

1. **遍歷與檢索 (Traverse)**：
   - Agent 先查閱 `index.md`，並深掘相關的 `wiki/papers/` 與 `wiki/concepts/` 頁面。
2. **結構化回答**：
   - 提供詳盡解答，並包含 `[[PaperSlug]]` 的明確引用來源與核心公式對比。
3. **沉澱成知識資產 (Compound)**：
   - 若該問題具有跨文獻對比價值或研究綜述性質，Agent 自動將分析成果寫入 `wiki/syntheses/YYYY-MM-[topic].md`。
   - 同步更新 `index.md` 的「綜述與主題合成」區塊與 `log.md`。

---

### 4.3 Lint Workflow（知識庫健康檢查）
定期或使用者指示執行「Wiki 健檢」時觸發：
1. **斷鏈檢查 (Broken Links)**：檢查所有 `[[...]]` 是否指向不存在的檔案。
2. **孤島節點 (Orphans)**：找出沒有被任何頁面反向連結的筆記，或是未列入 `index.md` 的頁面。
3. **矛盾與技術演進偵測 (Contradictions & Evolution)**：檢視是否有新論文推翻了舊論文的假說或方法（如 Scaling Law 係數更新、新推論架構超越傳統 Attention）。
4. **知識缺口 (Gaps)**：找出多篇論文提及但尚未建立獨立頁面的高頻術語，建議建立新 Concept 頁面。
5. 檢查完成後於 `log.md` 記錄 `## [YYYY-MM-DD] lint | Health Check Summary`。

---

### 4.4 Slide Generation Workflow（簡報生成工作流）
當使用者需要將論文或綜述轉換為報告簡報時：
- 讀取目標頁面與 `assets/` 資料夾中的圖片素材。
- 支援產出 Marp Markdown 簡報或獨立 HTML 簡報骨架。
- 嚴格遵守：未經使用者要求，不得覆寫或刪除 `assets/` 中的既有圖貼檔案。
