# Knowledge Base Operations Log

本檔案依時間軸嚴格記錄知識庫的所有變更（論文歸檔、概念更新、知識庫健檢與查詢沉澱）。

---

## [2026-11-22] init | 知識庫初始化建立與規格定稿
- **Action**: 初始化知識庫目錄結構，根據使用者偏好完成 `AGENTS.md` 規範修訂
- **Specifications Adopted**:
  - 語系慣例：繁體中文（台灣習慣用語）內文 + 專有名詞保留英文
  - 命名慣例：論文採用 `[Author][YYYY]-[ShortTitle].md`，概念與評測採用專有名詞英文/縮寫並配置 YAML `aliases`
  - 數學呈現：強制保留關鍵推導、Loss Function 與演算法虛擬碼（LaTeX `$ ... $` / `$$ ... $$`）
  - 文獻管理：接收 PDF 存入 `raw/papers/`；接收 arXiv 連結自動下載 PDF 歸檔
- **Files**:
  - `AGENTS.md` (知識庫規格手冊)
  - `index.md` (全域內容索引)
  - `log.md` (操作日誌)
- **Status**: 知識庫環境準備就緒。


## [2026-11-22] ingest | Yao2022-ReAct
- **Paper Page**: [[Yao2022-ReAct]]
- **Raw PDF**: raw/papers/Yao2022-ReAct.pdf
- **Updated Concepts**: [[ReAct]], [[Chain-of-Thought]], [[External Tool Use]]
- **Updated Benchmarks**: [[HotpotQA]], [[ALFWorld]]
- **Status**: 第 1 篇核心論文歸檔完成。


## [2026-11-22] ingest | Schick2023-Toolformer
- **Paper Page**: [[Schick2023-Toolformer]]
- **Raw PDF**: raw/papers/Schick2023-Toolformer.pdf
- **Updated Concepts**: [[Toolformer]], [[External Tool Use]]
- **Updated Benchmarks**: [[SVAMP]]
- **Status**: 第 2 篇核心論文歸檔完成。


## [2026-11-22] ingest | Shinn2023-Reflexion
- **Paper Page**: [[Shinn2023-Reflexion]]
- **Raw PDF**: raw/papers/Shinn2023-Reflexion.pdf
- **Updated Concepts**: [[Reflexion]], [[Self-Reflection]], [[Agent Memory Systems]]
- **Updated Benchmarks**: [[HumanEval]]
- **Status**: 第 3 篇核心論文歸檔完成。

## [2026-11-22] ingest | Park2023-GenerativeAgents
- **Paper Page**: [[Park2023-GenerativeAgents]]
- **Raw PDF**: raw/papers/Park2023-GenerativeAgents.pdf
- **Updated Concepts**: [[Generative Agents]], [[Multi-Agent Systems]], [[Agent Memory Systems]]
- **Status**: 第 4 篇核心論文歸檔完成。

## [2026-11-22] ingest | Jimenez2024-SWEbench
- **Paper Page**: [[Jimenez2024-SWEbench]]
- **Raw PDF**: raw/papers/Jimenez2024-SWEbench.pdf
- **Updated Concepts**: [[Software Engineering Agents]]
- **Updated Benchmarks**: [[SWE-bench]]
- **Status**: 第 5 篇核心論文歸檔完成。

## [2026-11-22] compound | AI-Agents-Architecture-Evolution
- **Synthesis Page**: [[2024-11-AI-Agents-Architecture-Evolution]]
- **Papers Synthesized**: [[Yao2022-ReAct]], [[Schick2023-Toolformer]], [[Shinn2023-Reflexion]], [[Park2023-GenerativeAgents]], [[Jimenez2024-SWEbench]]
- **Status**: 5 篇經典論文全數入庫，跨論文演進知識沉澱完成。
