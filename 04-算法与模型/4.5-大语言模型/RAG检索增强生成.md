# RAG 检索增强生成实战

> 解决大模型幻觉和知识时效性问题

---

## 🤔 为什么需要 RAG？

### 大模型的局限性

| 问题 | 表现 | RAG 解决方案 |
|------|------|--------------|
| **幻觉** | 生成虚假内容 | 基于检索事实生成 |
| **知识时效** | 训练数据有截止日期 | 实时检索最新信息 |
| **领域知识** | 专业领域表现差 | 检索领域文档 |
| **可解释性** | 黑盒输出 | 引用来源可追溯 |

---

## 🏗️ RAG 架构

```
┌─────────────────────────────────────────┐
│              用户查询                    │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  查询理解 (Query Understanding)          │
│  - 意图识别                             │
│  - 查询扩展                             │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  检索模块 (Retrieval)                    │
│  - 向量检索 (Vector Search)              │
│  - 关键词检索 (BM25)                     │
│  - 混合检索 (Hybrid)                     │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  重排序 (Reranking)                      │
│  - 粗排: 向量相似度                       │
│  - 精排: Cross-Encoder                   │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  上下文组装 (Context Assembly)           │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│  生成模块 (Generation)                   │
│  - Prompt 工程                           │
│  - 大模型生成                            │
└─────────────┬───────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│              最终回答                    │
└─────────────────────────────────────────┘
```

---

## 🛠️ 核心组件实现

### 1. 文档处理流程

```python
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

# 加载文档
loader = PyPDFLoader("document.pdf")
docs = loader.load()

# 文本分割
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=500,      # 每块大小
    chunk_overlap=50,    # 重叠部分
    separators=["\n\n", "\n", ".", " "]
)
chunks = text_splitter.split_documents(docs)
```

### 2. 向量化与存储

```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

# 嵌入模型
embeddings = OpenAIEmbeddings(
    model="text-embedding-ada-002"
)

# 创建向量数据库
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="./chroma_db"
)

# 相似度检索
retriever = vectorstore.as_retriever(
    search_type="similarity",
    search_kwargs={"k": 5}
)
```

### 3. 检索与生成

```python
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI

# 构建 RAG Chain
qa_chain = RetrievalQA.from_chain_type(
    llm=OpenAI(),
    chain_type="stuff",  # 或 "map_reduce", "refine"
    retriever=retriever,
    return_source_documents=True
)

# 查询
result = qa_chain({"query": "什么是RAG？"})
print(result["result"])
print(result["source_documents"])  # 查看来源
```

---

## 📈 优化技巧

### 1. 查询重写 (Query Rewriting)

```python
# 原始查询可能不够清晰
query = "那个技术"

# 重写为更具体的查询
rewritten_query = "RAG检索增强生成技术原理"
```

### 2. 混合检索

```python
# 向量检索 + 关键词检索
from langchain.retrievers import BM25Retriever, EnsembleRetriever

bm25_retriever = BM25Retriever.from_documents(docs)
bm25_retriever.k = 5

vector_retriever = vectorstore.as_retriever(search_kwargs={"k": 5})

# 集成检索
ensemble_retriever = EnsembleRetriever(
    retrievers=[bm25_retriever, vector_retriever],
    weights=[0.5, 0.5]
)
```

### 3. 重排序 (Reranking)

```python
from sentence_transformers import CrossEncoder

# 使用 Cross-Encoder 精排
reranker = CrossEncoder('cross-encoder/ms-marco-MiniLM-L-6-v2')

candidates = retriever.get_relevant_documents(query)
pairs = [[query, doc.page_content] for doc in candidates]
scores = reranker.predict(pairs)

# 按分数排序
sorted_docs = [doc for _, doc in sorted(
    zip(scores, candidates), 
    key=lambda x: x[0], 
    reverse=True
)]
```

### 4. 上下文压缩

```python
from langchain.retrievers import ContextualCompressionRetriever
from langchain.retrievers.document_compressors import LLMChainExtractor

compressor = LLMChainExtractor.from_llm(llm)
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor,
    base_retriever=retriever
)
```

---

## 🏢 企业级 RAG 架构

```
┌─────────────────────────────────────────┐
│           数据源层                       │
│  [数据库] [文档系统] [API] [网页]        │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           数据处理层                     │
│  文档解析 → 清洗 → 分块 → 向量化        │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           向量数据库                     │
│  [Milvus] [Pinecone] [Weaviate]         │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           检索服务层                     │
│  混合检索 → 重排序 → 上下文组装         │
└─────────────────────────────────────────┘
              ↓
┌─────────────────────────────────────────┐
│           生成服务层                     │
│  Prompt工程 → LLM → 后处理              │
└─────────────────────────────────────────┘
```

---

## 🔗 相关资源

- [LangChain RAG Tutorial](https://python.langchain.com/docs/use_cases/question_answering/)
- [LlamaIndex Documentation](https://docs.llamaindex.ai/)
- [RAG Survey Paper](https://arxiv.org/abs/2312.10997)

---

*最后更新: 2026-03-08*
