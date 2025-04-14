
---

## **1. LLMFriSPG 的核心概念**

![[file-20250331105058628.png]]

**LLMFriSPG（LLM Friendly Structured Prompt Generation）** 是一种适配 LLM 的知识表示框架，通过将 **实例（Entity Instances）** 和 **概念（Concepts）** 分离，从而实现更高效的知识对齐。其目标是：

• **提升知识检索的准确性：** 通过静态知识（Knowledge）与动态信息（Information）双向索引，提高语义匹配能力。

• **增强 LLM 推理能力：** 通过概念引导 LLM 进行实例分类，并支持推理与信息检索任务。

用公式表的LLMFriSPG的话：
$$
\begin{equation}
\mathcal{M} = \{\mathcal{T}, \rho, \mathcal{C}, \mathcal{L}\}
\end{equation}
$$

> [!NOTE] 公式详解
> 其中：
>   - $\mathcal{M}$ 表示LLMFriSPG
>   - $\mathcal{T}$ 表示实体类型，类似于面向对象的定义（如：Person, Event, Organization等)
>   - $\rho$ 代表从实例到概念的归纳关系
>   - $\mathcal{C}$ 表示概念类，可以理解为实体的上层更为抽象的一个层级
>   - $\mathcal{L}$ 表示在逻辑关系和逻辑概念上定义的所有可执行规则。

假设用$\rho_t$来表示类型$t$的所有关系，则$\rho_t$的表达如下
$$
\rho_t = {\rho_t^c, \rho_t^f, \rho_t^b}
$$
> [!NOTE] 公式详解
> 其中：
>   - $\rho_t$ 表示类型$t$的所有关系
>   - $\rho_t^c$ 代表着领域专家预先定义的部分
>   - $\rho_t^f$ 临时方式添加的部分
>   - $\rho_t^b$ 代表系统内置属性;
> 	  - 如支持的分块（supporting_chunks）、描述（description）、摘要（summary）和所属关系（belongTo）; 
> 	  - 例如：在$\rho_t^b$ 中，有一个实例$e$，支持的分块（supporting_chunks）表示包含实例$e$的的所有文本块的集合，描述（description）表$e$的类型一般描述性信息；摘要（summary）则表示$e$实例摘要信息；所属关系（belongTo）表示从$e$实例到概念的归纳语义。


---

## **2.核心模块与流程**


**2.1 知识与信息区域的划分**

• **Knowledge Area（静态区域）**

- 预定义的、结构化的知识，由领域专家提供。

- 通过 r1 和 r2 等关系将实体与相关属性进行连接，支持复杂的逻辑推理。

• **Information Area（动态区域）**

- 动态、即时获取的文本块（Chunks），包含开放信息。
 
- 通过 supporting_chunks 机制，将信息块与相关实例建立关联，用于补充实体信息。

---

**2.2 SPG 属性的分类**

  所有的属性和关系由集合 $p_t$ 表示，其中：

$$
p_t = \{p_t^c, p_t^f, p_t^b\}
$$
 $p_t^c$**：领域专家预定义的属性**

-  预定义静态属性，通常应用于专业决策场景，如 Person 的 name、age 等静态特性。

-  这些属性通过面向对象原则匹配 LPG（Labeled Property Graph）的表示。

 $p_t^f$**：实时添加的动态属性**

- 在 ad-hoc 过程中添加的动态属性，适用于信息检索。

- 动态属性可根据输入进行实时更新，提供灵活的数据扩展能力。

 $p_t^b$**：系统内置属性**

- 例如 supporting_chunks、description、summary 和 belongTo 等。

- supporting_chunks：与实例相关联的所有文本块集合。

- description：通用的描述信息，可与$t_k$（类型）或 $e_i$（实例）关联。

- summary：从原始文本中提取的摘要信息。

- belongTo：表示实例到概念的归纳关系。

---

**2.3 Supporting_chunks 的核心作用**

• **supporting_chunks** 负责将实例和动态信息块进行关联，实现知识的动态补充。

• 用户可以在 KAG Builder 阶段定义 chunk 生成策略，并设置 chunk 的最大长度，以保证 LLM 的上下文理解能力。

• 通过 Chunk 机制增强检索的准确性，为后续生成提供上下文信息。

---

**2.4 Description 的双重含义**

• **当 description 关联到 $t_k$（类型）时：**
	
	• 表示 $t_k$ 的全局描述信息，为概念提供通用定义。
	
	• 例如：TaxoOfPerson 描述 Person 的通用特性。

• **当 description 关联到 $e_i$（实例）时：**

	• 表示与实例相关的具体描述，提供关于$e_i$的详细信息。
	
	• 例如：description 可以提供某个 Person 的背景信息，有助于 LLM 理解实例的确切含义。

---

**2.5  $\mathcal{T}$  和 $\mathcal{C}$   的不同功能**  

**$\mathcal{T}$（Entity Type）**

- **面向对象表示（Object-Oriented Representation）**

- 适配 LPG（Labeled Property Graph）的实例类型，将实体信息结构化存储。

- 例如：Person、Event、Organization 等。

**$\mathcal{C}$（Concept Type）**

-  **基于文本的概念树（Text-based Concept Tree）**

-  管理更高层次的语义分类，用于指导 LLM 对实例进行分类。

• 例如：TaxoOfPerson 是 Person 的概念类别。

---

**2.6 $p_t^c$ 和 $p_f^t$ 的独立实例化机制**

**$p_t^c$ 和 $p_f^t$ 可以独立实例化：**

- 它们共享相同的类声明（Class Declaration），但在 **实例存储空间（Instance Storage Space）** 中：

- **预定义静态属性（Static Properties）** 与

- **实时动态属性（Dynamic Properties）** 可同时存在，也可以只实例化其中之一。

**应用场景：**

- **专业决策场景：** 主要实例化静态属性（ptc），保证一致性和稳定性。

- **信息检索场景：** 主要实例化动态属性（p_f_t），增加灵活性和开放性。

---

**2.7 . 概念共享机制：$p_t^c$ 和 $p_f^t$ 共享相同的概念术语**

• **共享概念术语：** 概念是独立于具体文档或实例的通用常识知识。

• **实例与概念的链接：**

- 通过 belongTo 将不同实例与相同概念节点关联，实现 **实例分类** 和 **语义对齐**。

- 例如：Person → TaxoOfPerson → isA 关系。

• **概念的双重功能：**

1. **导航：** 作为知识检索时的导航路径，引导 LLM 检索最相关的实例或知识块。

2. **语义对齐：** 通过概念图实现 LLM 与实例的语义对齐。

---

**🔎 2.8. 数学原理解析**

  

**✅ 实体与属性的表示**

• 实体 e_i 与属性的关系可以表示为：

p_t = \{p_t^c, p_t^f, p_t^b\}

• 其中：

• p_t^c：静态预定义属性

• p_t^f：动态添加的属性

• p_t^b：系统内置属性

  

**✅ Instance-Concept 归纳关系**

• 通过 belongTo 关系实现从实例到概念的映射：

 $$ 

[

e_i \xrightarrow{belongTo} C_j

]
$$

• 其中：

• e_i：具体实例

• C_j：概念节点

---

**🎯 2.9 关键点总结**


✅ **实例（T）与概念（C）分离：** 提高语义匹配和检索效率。

✅ **静态与动态属性独立实例化：** 适配不同业务场景的需求。

✅ **共享概念术语与语义对齐：** 通过 belongTo 关联实例与概念，确保 LLM 的语义一致性。

✅ **支持信息检索和专业决策：** 通过 Chunk 机制提升信息获取能力，并保证在不同场景下的灵活性。



## **3. KAG 分层知识体系解析**

**KAG（Knowledge Augmented Generation）** 通过三层知识表示来优化 LLM 在专业领域的推理能力，分别为：
![[file-20250331122554130.png]]
##### 3.1 **$KG_{CS}$**（Knowledge Graph Concepts）— 专业知识层****
**${KG}_{Cs}$** 代表 **知识层**，存储经过 **领域模式约束（Schema Constraints）** 的专业知识。
 **特点：**

- 具有高度的 **逻辑严谨性（Logical Rigor）** 和 **准确性（Accuracy）**。
- 由领域专家手动注释，符合 SPG 语义规范和严格的知识结构要求。
- 构建成本较高，信息覆盖率不完全，易受信息更新滞后的影响。 

**应用：** 适合高要求的专业决策任务，需要更高比例的 **R(KGCs)** 以提升知识的专业性。

##### 3.2 **${KG}_{fr}$**（Knowledge Graph Facts and Relations）— 图信息层
- 通过信息抽取从原始文本中获取的实体和关系数据。
- 与 KGCs 共享相同的 EntityType、EventType 和 ConceptType。
- 通过 supporting_chunks 机制与原始数据建立倒排索引。

##### 3.3 **$RC$ (Raw Chunks）— 原始文本块层

**RC** 代表 **原始文本块层**，包含通过语义分块（semantic segmentation）提取的原始文档内容，例如摘要、描述等非结构化数据。

**特点：**
- 提供对 ${KG}_{fr}$的信息补充，弥补结构化数据的缺陷。
- 覆盖率高但准确性较低，易受噪声和信息冗余影响。
- 适用于开放领域的信息检索任务，可通过 R(RC) 提高信息量覆盖率。

#####  3.4 **KAG 层级关联**
- `RC` → `KGfr`：补充图信息层的上下文信息。
- `KGfr` → `KGCs`：提供 KGCs 语义增强支持。
- `supporting_chunks` 机制实现 `RC` 和 `KGfr` 的动态信息关联。

#####  3.5 **Coverage Ratio**
**Coverage Ratio（覆盖率比率）** 是指在 LLMFriSPG 和 KAG 框架中，不同层级数据（KGCs、KGfr 和 RC）在知识增强生成（KAG）任务中所占的权重比例。

这个比率决定了在生成答案时，各个知识层的数据对 LLM（大语言模型）产生的影响程度，从而控制 **专业知识的严谨性** 和 **信息的覆盖率** 之间的平衡。
- `R(KGCs)` 提高逻辑推理准确性。
- `R(KGfr)` 提升信息覆盖率。
- `R(RC)` 补充语义背景，提高检索效率。

---

## 🎯 **9. 关键点总结**

-  **KAG 三层知识体系：** 结合 `KGCs`、`KGfr` 和 `RC` 提供多层次信息支持，提高推理能力。
- **supporting_chunks 机制：** 通过倒排索引实现 `RC` 与 `KGfr`、`KGCs` 之间的信息联动。
- **ptc 和 p_f_t 的关联性：** 在不同 KAG 层级中实现独立实例化，适应不同业务场景。
-  **Coverage Ratio 动态调整：** 结合 `R(KGCs)`、`R(KGfr)` 和 `R(RC)` 优化推理效果。

---
