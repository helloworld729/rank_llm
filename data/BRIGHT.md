# BRIGHT 数据集完整分析文档

## 📌 数据集概览

**BRIGHT** 是第一个需要**密集推理**的文本检索基准数据集。查询来自多个领域（StackExchange、LeetCode 和数学竞赛），数据均来自真实人工标注。

- **总大小**：~1.6 GB
- **任务类型**：文本检索（Text Retrieval）/ 信息检索（Information Retrieval）
- **许可证**：CC-BY-4.0
- **数据格式**：Parquet（列式存储）
- **官方论文**：https://brightbenchmark.github.io/

---

## 🗂️ 完整目录结构

```
/mnt/dolphinfs/ssd_pool/docker/user/hadoop-edi-training/huggingface.co/datasets/xlangai/BRIGHT/
├── README.md                          # 数据集元数据和文档
├── data_examples.pdf                  # 示例文档
├── statistics.png                     # 统计图表
├── .gitattributes                     # Git LFS 配置
├── .ipynb_checkpoints/                # Jupyter 检查点
│
├── examples/                          # 查询集（4.4M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   ├── economics-00000-of-00001.parquet
│   ├── psychology-00000-of-00001.parquet
│   ├── robotics-00000-of-00001.parquet
│   ├── stackoverflow-00000-of-00001.parquet
│   ├── sustainable_living-00000-of-00001.parquet
│   ├── leetcode-00000-of-00001.parquet
│   ├── pony-00000-of-00001.parquet
│   ├── aops-00000-of-00001.parquet
│   ├── theoremqa_questions-00000-of-00001.parquet
│   └── theoremqa_theorems-00000-of-00001.parquet
│
├── documents/                         # 短文档库（444M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   ├── economics-00000-of-00001.parquet
│   ├── psychology-00000-of-00001.parquet
│   ├── robotics-00000-of-00001.parquet
│   ├── stackoverflow-00000-of-00001.parquet
│   ├── sustainable_living-00000-of-00001.parquet
│   ├── leetcode-00000-of-00001.parquet
│   ├── pony-00000-of-00001.parquet
│   ├── aops-00000-of-00001.parquet
│   ├── theoremqa_theorems-00000-of-00001.parquet
│   └── theoremqa_questions-00000-of-00001.parquet
│
├── long_documents/                    # 长文档库（100M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   ├── economics-00000-of-00001.parquet
│   ├── psychology-00000-of-00001.parquet
│   ├── robotics-00000-of-00001.parquet
│   ├── stackoverflow-00000-of-00001.parquet
│   ├── sustainable_living-00000-of-00001.parquet
│   ├── pony-00000-of-00001.parquet
│   └── (others)
│
├── Gemini-1.0_reason/                 # Gemini 推理版本（5.7M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   └── (其他域...)
│
├── claude-3-opus_reason/              # Claude 推理版本（5.6M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   └── (其他域...)
│
├── gpt4_reason/                       # GPT-4 推理版本（6.2M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   └── (其他域...)
│
├── grit_reason/                       # GRIT 推理版本（5.0M）
│   ├── biology-00000-of-00001.parquet
│   ├── earth_science-00000-of-00001.parquet
│   └── (其他域...)
│
└── llama3-70b_reason/                 # Llama3 70B 推理版本（5.7M）
    ├── biology-00000-of-00001.parquet
    ├── earth_science-00000-of-00001.parquet
    └── (其他域...)
```

---

## 📄 顶级文件说明

### README.md（18K）
**作用**：数据集元数据、结构说明、加载方法、统计信息和论文引用
- 包含数据集概述
- 解释数据结构和字段含义
- 提供 Hugging Face 数据集加载示例
- 包含论文引用信息

### data_examples.pdf（195K）
**作用**：展示数据集中的示例数据
- 可视化显示数据格式
- 帮助用户理解数据内容

### statistics.png（357K）
**作用**：可视化显示 BRIGHT 统计信息
- 各领域样本分布
- 数据规模对比

### .gitattributes（2.3K）
**作用**：Git LFS 配置文件
- 指定 parquet、zip 等大文件使用 Git LFS 管理
- 避免仓库变得过大

### .ipynb_checkpoints/
**作用**：Jupyter Notebook 的自动备份文件夹
- 包含 Notebook 检查点数据

---

## 🔍 核心数据目录说明

### 1️⃣ examples/ 目录（4.4M）

**用途**：包含查询及其对应的标注信息，用于**训练和测试**

**数据结构**（7 列）：
```
{
  "query": string,              # 原始问题/提问内容
  "reasoning": string,          # 人工标注的推理步骤（辅助理解）
  "id": string,                 # 实例的唯一标识符
  "excluded_ids": [string],     # 评估时需要排除的文档 ID
  "gold_ids_long": [string],    # 长文档中的相关文档 ID 列表
  "gold_ids": [string],         # 短文档中的相关文档 ID 列表
  "gold_answer": string         # 正确答案文本
}
```

**包含的 12 个领域**：
- `biology` - 生物学
- `earth_science` - 地球科学
- `economics` - 经济学
- `psychology` - 心理学
- `robotics` - 机器人学
- `stackoverflow` - 技术问答
- `sustainable_living` - 可持续生活
- `leetcode` - 编程问题
- `pony` - Pony 编程语言讨论
- `aops` - 美国数学竞赛
- `theoremqa_questions` - 定理问题集
- `theoremqa_theorems` - 数学定理集

**样本统计**：
- 最小：pony (112 个)
- 最大：theoremqa_questions (194 个)
- 总计：~1,000+ 查询样本

---

### 2️⃣ documents/ 目录（444M）

**用途**：包含**短版本**的文档，是检索的候选文档库

**数据结构**（2 列）：
```
{
  "id": string,      # 文档唯一标识 (e.g., "neanderthals_vitamin_C_diet/Neanderthal_0_43.txt")
  "content": string  # 短文档内容（从完整网页或博客中提取的相关部分）
}
```

**样本数量示例**：
- biology: 57,359 个文档
- earth_science: 121,249 个文档
- stackoverflow: 107,081 个文档（最多）
- pony: 7,894 个文档（最少）

**用途**：标准的检索任务文档库，模型需要从中找到与查询相关的文档。

---

### 3️⃣ long_documents/ 目录（100M）

**用途**：包含**长版本**的文档（原始完整版本）

**数据结构**（2 列）：
```
{
  "id": string,      # 文档唯一标识
  "content": string  # 长文档内容（完整网页、博客等的原始版本）
}
```

**关键特征**：
- 文档数量较少（相同域的长文档数 << 短文档数）
  - 例：biology 的短文档 57,359 个，长文档仅 524 个
- 提供**完整上下文**用于更深层的推理和理解
- 用于评估模型在**长文本**上的检索能力

---

### 4️⃣ LLM 推理目录（各 5-6M）

**用途**：存储不同 LLM 对同一问题的理解和推理过程，用于**多参考评估**和**推理分析**

#### 包含的目录：

| 目录名 | 大小 | LLM 模型 |
|--------|------|---------|
| `Gemini-1.0_reason/` | 5.7M | Google Gemini 1.0 |
| `claude-3-opus_reason/` | 5.6M | Anthropic Claude 3 Opus |
| `gpt4_reason/` | 6.2M | OpenAI GPT-4 |
| `grit_reason/` | 5.0M | GRIT 模型 |
| `llama3-70b_reason/` | 5.7M | Meta Llama 3 70B |

#### 数据结构（7 列，与 examples 相同）：
```
{
  "query": string,              # LLM 对原始问题的理解/重新表述/扩展
  "reasoning": string,          # LLM 生成的推理步骤
  "id": string,                 # 实例 ID（与 examples 中相同）
  "excluded_ids": [string],     # 排除 ID（与 examples 中相同）
  "gold_ids_long": [string],    # 相关长文档 ID（与 examples 中相同）
  "gold_ids": [string],         # 相关短文档 ID（与 examples 中相同）
  "gold_answer": string         # 正确答案（与 examples 中相同）
}
```

#### 关键点：

**保持相同的内容**：
- `id` 完全相同
- `gold_ids` / `gold_ids_long` 完全相同
- `gold_answer` 完全相同

**不同的 LLM 推理内容**：
- `query`：LLM 对原始问题的**理解/重新表述**
- `reasoning`：LLM 的**推理步骤**

#### query vs reasoning 的区别：

| 维度 | query | reasoning |
|------|-------|-----------|
| **平均长度** | 3,300+ 字符 | 160 字符 |
| **平均词数** | 500 词 | 25 词 |
| **目的** | 问题深度理解与分析 | 文档相关性简短说明 |
| **内容** | LLM 对问题的完整理解 | LLM 对文档为何相关的解释 |

#### 具体示例：

**原始问题**：
```
为什么只用一个鼻孔呼吸？
```

**query（GPT-4 的深度分析）**：
```
你经历的本质问题是你注意到空气只从你的右鼻孔呼出...

1. 理解鼻腔循环：人体被设计为每次主要用一个鼻孔呼吸...
2. 鼻腔循环的原因：调节气流，帮助检测更广泛的气味范围...
3. 影响鼻腔呼吸的因素：身体位置、健康状况、时间...
4. 何时需要担心：一般来说，单鼻孔呼吸不是问题...
5. 观察和咨询：如果有任何不适，咨询耳鼻喉医生...

结论：单鼻孔呼吸是一个正常的有益过程...
（总计 ~400 词）
```

**reasoning（简短说明）**：
```
问题质疑单鼻孔呼吸是否正常。
所选文档解释了鼻腔循环，这是一个自然的生理过程，
直接回答并正常化了用户的观察。
（总计 ~25 词）
```

#### 用途：

1. **比较不同 LLM 的推理质量**
2. **进行多参考评估**（multi-reference evaluation）
3. **研究不同模型的理解差异**
4. **进行知识蒸馏的多教师学习**

---

## 🔗 字段关系说明

### gold_answer、gold_ids、gold_ids_long 的关系

```
原始问题（Query）
    ↓
    ├─→ gold_ids           ← 指向短文档中的相关段落
    │     (例: 6 个短文档 ID)
    │
    ├─→ gold_ids_long      ← 指向长文档中的完整内容  
    │     (例: 1 个长文档 ID)
    │
    └─→ gold_answer        ← 从这些文档中提取/汇总的最终答案
          (完整答案文本)
```

### 具体例子

**查询**："为什么我只用一个鼻孔呼吸？"

| 字段 | 值 | 数量 |
|------|---|------|
| **gold_ids** | `breathe_out_of_one_nostril/Nasal_cycle_0.txt` | 6 个 |
| | `breathe_out_of_one_nostril/Nasal_cycle_1.txt` | |
| | ... | |
| **gold_ids_long** | `breathe_out_of_one_nostril/Nasal_cycle.txt` | 1 个 |
| **gold_answer** | "在 1895 年，德国鼻科专家发现我们鼻子中有海绵体组织... 这种组织会在一个鼻孔中膨胀，在另一个中收缩..." | 完整答案文本 |

---

## 📊 数据集统计总结

| 指标 | 详情 |
|------|------|
| **总大小** | ~1.6 GB |
| **域数量** | 12 个（biology、earth_science 等） |
| **查询总数** | ~1,000+ (examples) |
| **短文档总数** | ~1.1M (documents) |
| **长文档总数** | ~3,400+ (long_documents) |
| **LLM 推理版本** | 5 个（Gemini、Claude、GPT-4、GRIT、Llama3） |
| **文件格式** | Parquet（列式存储，高效压缩） |
| **任务类型** | 文本检索 / 信息检索 |
| **数据标注** | 真实人工标注 |

---

## 💡 使用场景

### 1. **训练检索模型**
```python
from datasets import load_dataset

# 加载 examples 作为查询
data = load_dataset('xlangai/BRIGHT', 'examples')['biology']

# 使用 documents 作为文档库
docs = load_dataset('xlangai/BRIGHT', 'documents')['biology']

# 训练密集检索模型
model.train(queries=data['query'], 
           documents=docs['content'],
           gold_ids=data['gold_ids'])
```

### 2. **评估模型**
```python
# 对比模型检索结果与标准答案
for query_id, query in enumerate(data['query']):
    retrieved_docs = model.retrieve(query, top_k=10)
    gold_docs = data['gold_ids'][query_id]
    
    # 计算 nDCG、MRR 等指标
    metrics = evaluate(retrieved_docs, gold_docs)
```

### 3. **长文本理解**
```python
# 评估模型在长文档上的性能
long_docs = load_dataset('xlangai/BRIGHT', 'long_documents')['biology']

for query_id, query in enumerate(data['query']):
    retrieved_docs = model.retrieve(query, docs=long_docs, top_k=10)
    gold_long_docs = data['gold_ids_long'][query_id]
```

### 4. **推理分析**
```python
# 比较不同 LLM 的推理方式
gpt4_reasoning = load_dataset('xlangai/BRIGHT', 'gpt4_reason')['biology']
claude_reasoning = load_dataset('xlangai/BRIGHT', 'claude-3-opus_reason')['biology']

# 分析推理质量差异
for i in range(len(gpt4_reasoning)):
    gpt4_query = gpt4_reasoning['query'][i]
    claude_query = claude_reasoning['query'][i]
    
    # 比较两个 LLM 的理解方式
    similarity = calculate_similarity(gpt4_query, claude_query)
```

### 5. **多教师知识蒸馏**
```python
# 使用多个 LLM 的推理进行蒸馏
teacher_reasonings = [
    gpt4_reasoning['reasoning'],
    claude_reasoning['reasoning'],
    gemini_reasoning['reasoning'],
]

# 训练学生模型学习这些推理
student_model.train(queries=data['query'],
                   teacher_reasonings=teacher_reasonings,
                   gold_answers=data['gold_answer'])
```

---

## 🎯 关键特点

1. **真实数据**：所有查询来自真实的人工问题，而非人工生成
2. **多领域**：涵盖 12 个不同的知识领域
3. **密集推理**：需要模型进行复杂的推理来理解问题与文档的关系
4. **多视角**：提供 5 个不同 LLM 的推理方式，展示同一问题的多种理解
5. **多粒度**：提供短文档和长文档两个版本，支持不同粒度的评估
6. **全面标注**：提供人工标注的推理步骤和答案

---

## 🔗 相关资源

- **官方论文**：https://arxiv.org/abs/2407.12883
- **官方网站**：https://brightbenchmark.github.io/
- **Hugging Face 数据集**：https://huggingface.co/datasets/xlangai/BRIGHT

---

## 📝 引用

```bibtex
@misc{BRIGHT,
  title={BRIGHT: A Realistic and Challenging Benchmark for Reasoning-Intensive Retrieval},
  author={Su, Hongjin and Yen, Howard and Xia, Mengzhou and Shi, Weijia and Muennighoff, Niklas and Wang, Han-yu and Liu, Haisu and Shi, Quan and Siegel, Zachary S and Tang, Michael and Sun, Ruoxi and Yoon, Jinsung and Arik, Sercan O and Chen, Danqi and Yu, Tao},
  url={https://arxiv.org/abs/2407.12883},
  year={2024},
}
```

---

*文档生成时间：2026-03-10*
