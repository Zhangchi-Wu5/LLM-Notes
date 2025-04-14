# 从局部到整体：GraphRAG 查询聚焦摘要方法

**参考文献：**  
Edge, D., Trinh, H., Cheng, N., 等. (2024). *From Local to Global: A GraphRAG Approach to Query‑Focused Summarization*. arXiv:2404.16130.  [oai_citation_attribution:0‡arxiv.org](https://arxiv.org/html/2404.16130)

---

## 🎯 问题陈述

- **朴素（向量）RAG** 通过检索与查询语义相似的文本块来生成答案，适合“局部”事实型问题，但无法有效回答需要对整个语料库进行**全局理解**的问题（例如：“这个数据集的主要主题是什么？”）。  [oai_citation_attribution:1‡arxiv.org](https://arxiv.org/abs/2404.16130) [oai_citation_attribution:2‡falkordb.com](https://www.falkordb.com/blog/what-is-graphrag/)

- **GraphRAG** 专为大规模、查询聚焦的摘要（Query‑Focused Summarization, QFS）设计，使用图结构索引替代简单检索，以支持对百万级 Token 语料的全局感知。

---

## 🛠️ GraphRAG 工作流程

| 阶段                                  | 输入 → 输出                    | 目的                                        |
| :------------------------------------ | :----------------------------- | :------------------------------------------ |
| **1**                                 | 文档 → 文本切片                | 将语料拆分为适配 LLM 上下文窗口的小块       |
| **2**                                 | 文本切片 → 实体 & 关系         | 通过 LLM 提示提取命名实体、关系和事实性陈述 |
| **3**                                 | 实体 & 关系 → 知识图谱         | 构建节点—边缘图谱，并合并重复实体           |
| **4**                                 | 知识图谱 → 社区划分            | 使用 Leiden 算法进行层级社区检测            |
| **5**                                 | 社区 → 社区摘要                | 并行生成每个社区的概览报告（自下而上聚合）  |
| **6**                                 | 社区摘要 → 中间答案 → 全局答案 | **Map：** 并行生成部分答案并评分            |
| **Reduce：** 合并高分答案生成最终摘要 |                                |                                             |

> 此管道既能保留**局部细节**（叶级社区），又能汇总**全局主题**（根级社区），高效支持百万级 Token 的查询聚焦摘要。  [oai_citation_attribution:3‡arxiv.org](https://arxiv.org/html/2404.16130)

---

## 🤝 GraphRAG vs 朴素 RAG 对比

| 特性          | 朴素（向量）RAG           | GraphRAG                                                     |
| :------------ | :------------------------ | :----------------------------------------------------------- |
| 索引结构      | 扁平向量存储              | 分层知识图谱（实体＋关系）                                   |
| 检索方式      | 语义相似度 → top‑k 文本块 | 社区摘要 → Map‑Reduce 聚合                                   |
| 查询类型      | 局部事实检索              | 全局主题感知与摘要                                           |
| 可扩展性      | 难以处理 >1M Token        | 并行社区处理，高度可扩展                                     |
| 可解释性      | 不透明，仅返回文本        | 图结构＋社区报告，可追溯推理路径                             |
| 全局 QFS 性能 | 综合度 & 多样性较低       | 综合度 & 多样性显著提升  [oai_citation_attribution:4‡falkordb.com](https://www.falkordb.com/blog/what-is-graphrag/) |

---

## 📈 关键收获

1. **GraphRAG** 通过知识图谱与社区检测，实现了对大型语料库的模块化、分层式摘要。  
2. 相较于向量 RAG，GraphRAG 在全局感知问题上提供了更全面、更丰富、更可解释的答案。  
3. 层级社区结构在保留细节与汇总全局信息间取得平衡，支持高效查询聚焦摘要。

---

## ⚙️ 进一步阅读 & 开源实现

- **官方代码（微软）**：https://github.com/microsoft/graphrag  
- **实践教程（Stephen Collins）**：Implementing GraphRAG for Query‑Focused Summarization  [oai_citation_attribution:5‡dev.to](https://dev.to/stephenc222/implementing-graphrag-for-query-focused-summarization-47ib)  