# HuggingFace

1. ### <u>HuggingFace 概述</u>

   ### Hugging Face

   Hugging Face 是一个旨在推动自然语言处理（NLP）技术和工具发展的开源社区和公司。他们致力于提供各种NLP任务中的最新技术、模型和工具，以及为开发者提供便捷的方式来使用、微调和部署这些技术。Hugging Face 在NLP领域中的贡献得到了广泛认可，成为了许多开发者和研究者的重要资源。除了自然语言处理，还支持处理图像和音频等多模态任务，社区还提供海量的预训练模型和数据集。

   主要亮点包括：

   1. **预训练模型：** Hugging Face 提供了一系列优秀的预训练NLP模型，如BERT、GPT、RoBERTa等，这些模型在多个任务上取得了卓越的表现。
   2. **transformers库：** Hugging Face 开发了名为 "transformers" 的Python库，这个库提供了各种预训练模型的实现，支持多个深度学习框架，如PyTorch和TensorFlow。它还提供了用于加载、微调、转换和使用这些模型的方便工具。
   3. **NLP工具：** Hugging Face 提供了多个NLP相关的工具，包括文本生成、文本分类、命名实体识别等。这些工具为开发者提供了快速构建NLP[ 应用](https://aitutor.liduos.com/04-huggingface/04-1.html#)的能力。
   4. **模型社区：** Hugging Face 建立了一个强大的开发者社区，让开发者可以分享自己的NLP模型、经验和教程，共同推动NLP技术的发展。

   ### transformers库

   transformers库专注于提供各种自然语言处理（NLP）任务中使用的预训练模型和相关工具。这个库的目标是使开发者能够轻松地使用和微调预训练的NLP模型，以解决各种文本处理任务，如文本分类、命名实体识别、文本生成等。

   主要特点和功能包括：

   1. **预训练模型：** "transformers"库提供了一系列预训练的NLP模型，包括BERT、GPT、RoBERTa、XLNet等。这些模型在大规模文本数据上进行预训练，学习了丰富的语言知识和表示。这些模型可以被加载并用于不同的NLP任务。
   2. **模型微调：** 你可以使用"transformers"库微调预训练模型以适应特定的任务，如情感分析、问答系统等。这使得你可以在相对较少的标注数据上训练出性能优秀的模型。
   3. **多框架支持：** "transformers"库支持多种深度学习框架，包括PyTorch和TensorFlow，因此你可以根据自己的偏好选择合适的框架进行开发。
   4. **模型架构封装：** 该库提供了易于使用的[ API](https://aitutor.liduos.com/04-huggingface/04-1.html#)，使得加载、使用和微调预训练模型变得简单和一致。
   5. **模型转换：** 你可以在不同的预训练模型之间进行转换，从而在不同模型之间进行比较和选择。
   6. **预训练令牌嵌入：** "transformers"库提供了预训练模型的词嵌入，你可以将它们用于自己的模型中，从而充分利用预训练的语言知识。

   #### transformers库及相关

   - ﻿Transformers：核心库，模型加载、模型训练、流水线等
   - ﻿Tokenizer：分词器，对数据进行预处理，文本到token序列的互相转换
   - ﻿﻿Datasets：数据集库，提供了数据集的加载、处理等方法
   - ﻿Evaluate：评估函数，提供各种评价指标的计算函数
   - ﻿PEFT：高效微调模型的库，提供了几种高效微调的方法，小参数量撬动大模型
   - ﻿﻿Accelerate：分布式训练，提供了分布式训练解决方案，包括大模型的加载与推理解决方案
   - ﻿Optimum：优化加速库，支持多种后端，如Onnxruntime、Openvino等
   - ﻿﻿Gradio：可视化部署库，几行代码快速实现基于Web交互的算法演示系统

   ### 常见自然语言处理任务

   - 情感分析 (sentiment-analysis)：对给定的文本分析其情感极性
   - 文本生成 (text-generation)：根据给定的文本进行生成
   - 命名实体识别 （ner）：标记句子中的实体
   - 阅读理解 （question-answering）：给定上下文与问题，从上下文中抽取答案
   - 掩码填充 （fill-mask）：填充给定文本中的掩码词
   - 文本摘要 （summarization）：生成一段长文本的摘要
   - 机器翻译 （translation）：将文本翻译成另一种语言
   - 特征提取 （feature-extraction）：生成给定文本的张量表示
   - 对话机器人 （conversional）：根据用户输入文本，产生回应，与用户对话

   ### 自然语言处理的几个阶段

   - ﻿第一阶段：统计模型＋数据（特征工程）

     决策树、SVM、 HIMM、CRF、TF-IDF、BOW

   - ﻿第二阶段：神经网络＋数据

     Linear、CNN. RNN、 GRU、 LSTM.Transformer、 Word2vec. Glove

   - ﻿第三阶段：神经网络＋预训练模型＋（少量）数据

     ﻿﻿GPT,BERT, ROBERTa, T5

   - 第四阶段：神经网络＋ 更大的预训练模型 + Prompt

     ChatGPT、 LLaMA、文心一言

   HuggingFace地址：https://huggingface.co/

2. ### <u>HuggingFace 配置</u>