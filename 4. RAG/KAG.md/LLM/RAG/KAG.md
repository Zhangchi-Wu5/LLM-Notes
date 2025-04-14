# KAG

OpenSPG

> * 论文：[KAG:Boosting LLMs in Professional Domains via Knowledge Augmented Generation](https://arxiv.org/pdf/2409.13731)
>
> * 开源地址：[OpenSPG/KAG: KAG is a logical form-guided reasoning and retrieval framework based on OpenSPG engine and LLMs. It is used to build logical reasoning and factual Q&amp;A solutions for professional domain knowledge bases. It can effectively overcome the shortcomings of the traditional RAG vector similarity calculation model.](https://github.com/OpenSPG/KAG)
>
> * 文档：[用户手册](https://openspg.yuque.com/ndx6g9/docs)
>
> * [蚂蚁开源新RAG框架KAG，可达91%准确率](https://mp.weixin.qq.com/s/KtFMh0QcKaLK4Nm8bAks-A)
>
> * [OpenKG大模型增强系统评测](http://leaderboard.openkg.cn/leaderboards/)
>
> * [KAG 技术与实践分享｜基于 KAG 框架自主完成领域图谱构建和知识问答本文来自社区用户的投稿，如果你也在使用 KAG， - 掘金](https://juejin.cn/post/7443645814507798564)
> * ‍

## 介绍

* KAG(Knowledge Augmented Generation) 是蚂蚁基于 [OpenSPG](https://github.com/OpenSPG/openspg) 引擎和大型语言模型的逻辑推理问答框架，用于构建**垂直领域知识库**的逻辑推理问答解决方案。
* KAG 可以有效克服传统 RAG 向量相似度计算的歧义性和 OpenIE 引入的 GraphRAG 的噪声问题。

  > OpenIE: 神经开放域信息抽取(Open Information Extraction)，也被称为开放信息抽取，是**一种从非结构化文本中提取信息的强大技术**。 不同于传统的信息抽取方法，OpenIE 不依赖于预定义的领域知识或本体模式，使其具有更广泛的适用性和灵活性。
  >
* KAG 支持逻辑推理、多跳事实问答等，并且明显优于目前的 SOTA 方法。
* KAG 的目标是在专业领域构建知识增强的 LLM 服务框架，支持逻辑推理、事实问答等。KAG 充分融合了 KG 的逻辑性和事实性特点，其核心功能包括：

  * 知识与 Chunk 互索引结构，以整合更丰富的上下文文本信息
  * 利用概念语义推理进行知识对齐，缓解 OpenIE 引入的噪音问题
  * 支持 Schema-Constraint 知识构建，支持领域专家知识的表示与构建
  * 逻辑符号引导的混合推理与检索，实现逻辑推理和多跳推理问答

‍

## 更新日志

* [OpenSPG/KAG v0.6 发布，兼顾事实推理与摘要生成，支持用户自定义 Schema](https://mp.weixin.qq.com/s/vcwufsp6fwyp0C6gyvbqsw)

  [用户手册v0.6](https://openspg.yuque.com/ndx6g9/0.6)

  2025 年 1 月 7 日，OpenSPG/KAG 正式发布 v0.6 版本，此次发布带来多个功能更新，包括摘要生成类任务支持、垂域 Schema 管理、可视化知识探查等；用户体验上，提供知识库任务的断点续跑机制，新增用户登录与权限体系、优化构建任务调度；开发者模式下支持不同阶段配置不同模型、支持 schema-constraint 模式抽取等，极大地提升了系统的灵活性、易用性、性能和安全性，为用户提供一个更加强大，且适应多样化应用场景的知识管理平台。

---

## KAG论文

​![image](assets/image-20250102141131-iec8i36.png)​

### 主要贡献

* 提出了一个对大型语言模型（LLM）友好的知识表示框架LLMFriSPG
* 提出了一个逻辑形式引导的混合求解和推理引擎
* 提出了一个基于语义推理的知识对齐方法
* 提出了一个适用于KAG的底层模型

### <span data-type="text" style="white-space-collapse: break-spaces;">框架</span>

总的来看，分为3个部分：KAG-Builder、KAG-Solver、KAG-Model（**未开源**）

​![image](assets/image-20250116145220-z6eq7dm.png)​

### KAG中常用的概念语义

​![image](assets/image-20250102141902-ddo0zof.png)​

‍

### Logical Form Solver

​![image](assets/image-20250113155451-ld0tsi2.png)​

​![image](assets/image-20250113155624-h2n4nnj.png)​

#### Planning

* 将父问题拆解成若干个子问题$lf_{subquery} $
* 选择子问题需要用到的执行函数$lf_{func}$，分类如下

​![image](assets/image-20250113160003-bplg1to.png)​

#### Reasoning

* 用GraphRetrieval在kg里搜索
* 用HybridRetrieval在doc中搜索

​![image](assets/image-20250113160909-1b2tyxp.png)​

‍

#### Retrieval

‍

## [KAG：一种知识增强的私域知识库可信问答框架](https://www.bilibili.com/video/BV1eMUVYRE6D/?spm_id_from=333.337.search-card.all.click&amp;vd_source=32744b5402d5a34aab61dad8fdd68ebd) 2024.11.07 桂正科

> [KAG-.-.pdf](assets/KAG-.--20250102102228-nvm5g7t.pdf)

​![image](assets/image-20250102102547-elpcypt.png)​

​![image](assets/image-20250102103138-lftmrdx.png)​

* **多跳问题 (Multi-hop Questions)**  指的是那些需要知识图谱  **「多跳推理」**  才能回答的问题。例如，若要回答 ”成龙主演电影的导演是哪些人？“ 这一问题，则需要多个三元组所形成的多跳推理路径 <成龙，主演，新警察故事>, <新警察故事，导演，陈木胜> 才能够回答。

### Kag-schema & indexing

​![image](assets/image-20250102103539-uxsg756.png)​

​![image](assets/image-20250102103726-xndwbcf.png)​

​![image](assets/image-20250102103852-wgkvnpp.png)​

* 是不是可以结合cdss将规则转换为schema

​![image](assets/image-20250102104024-pv63nne.png)​

* RC: raw content
* KGfr: KG free
* KGcs：KG constraint
* 底层知识的完整性更高，上层知识的精准性和逻辑严密性更高

​![image](assets/image-20250102104551-x1gs6ot.png)​

### Kag-builder

​![image](assets/image-20250102104850-8gd8cfk.png)​

​![image](assets/image-20250102105041-rj3iq8m.png)​

### Kag-solver

​![image](assets/image-20250102105324-v5nmhqe.png)​

* Reasoner ➡️ Generator ➡️ Reflector

​![image](assets/image-20250102105533-uols8gq.png)​

* LogicForm

‍

‍

​![image](assets/image-20250102110046-unm163s.png)​

‍

## OpenSPG-KAG框架及垂域应用 梁磊

> [KAG.pdf](assets/KAG-20250102101215-ddg0zu6.pdf)

### 研究现状

#### 1、以文档检索为基础

​![image](assets/image-20250102140117-61o8268.png)​

​![image](assets/image-20250102140206-h16tp03.png)​

#### 2、KG增强文档索引

​![image](assets/image-20250102140234-pde1qjw.png)​

* GraphRAG：全局摘要
* LightRAG
* HippoRAG：多跳问答

​![image](assets/image-20250102140609-g8dq6sk.png)​

#### 3、以KBQA为原型演进

​![image](assets/image-20250102140638-9s8yjrd.png)​

​![image](assets/image-20250102140722-302nnqe.png)​

‍

## 垂直知识服务的典型要求

* 知识精准
* 知识完备
* 逻辑严谨
* 时间敏感
* 数值敏感

​![image](assets/image-20250102140856-15gnzfz.png)​

​![image](assets/image-20250102141013-gc338a0.png)​

‍
