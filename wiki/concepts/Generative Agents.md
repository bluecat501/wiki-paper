---
type: concept
title: "Generative Agents"
aliases: ["生成式代理人", "虛擬小鎮", "Smallville"]
tags:
  - concept
  - agent/architecture
  - multi-agent
  - simulation
introduced_by: "[[Park2023-GenerativeAgents]]"
last_updated: 2026-11-22
---

# Generative Agents (生成式代理人)

## 1. 概念定義

**Generative Agents** 是由史丹佛大學與 Google 於 2023 年提出的計算代理人架構。它將大型語言模型與計算認知架構相結合，模擬具備擬真人類行為（如形成記憶、規劃日程、社交聊天、形成人際關係）的虛擬個體。

## 2. 三大核心機制
1. **Memory Stream（記憶流）**：綜合時效性（Recency）、重要性（Importance）與相關性（Relevance）的加權檢索評分：
   $$\text{Score} = \alpha_r s_r + \alpha_i s_i + \alpha_{rel} s_{rel}$$
2. **Reflection（反思）**：定期從底層海量記憶中歸納出高階抽象概念（Insights），形成層級化的知識體系。
3. **Planning & Reacting（規劃與動態反應）**：將抽象日程層層遞歸分解至具體動作，並對環境突發事件即時調整。
