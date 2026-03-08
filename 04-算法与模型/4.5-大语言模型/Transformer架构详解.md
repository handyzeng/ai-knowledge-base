# Transformer 架构详解

> 从 Attention Is All You Need 到现代大模型

---

## 🏗️ 整体架构

```
Input Embedding
    ↓
Positional Encoding
    ↓
[Encoder × N]  ← 仅BERT类使用
    ↓
[Decoder × N]  ← GPT类仅使用Decoder
    ↓
Output
```

---

## 🔑 核心组件

### 1. Multi-Head Self-Attention

#### Scaled Dot-Product Attention

```python
Attention(Q, K, V) = softmax(QK^T / √d_k)V
```

**为什么要除以 √d_k？**
- 防止点积过大导致 softmax 梯度消失
- d_k 越大，点积值越大

#### Multi-Head 机制

```python
# 8个头，每个头64维 (d_model=512)
head_1 = Attention(QW_1^Q, KW_1^K, VW_1^V)
head_2 = Attention(QW_2^Q, KW_2^K, VW_2^V)
...
head_8 = Attention(QW_8^Q, KW_8^K, VW_8^V)

MultiHead(Q,K,V) = Concat(head_1,...,head_8)W^O
```

**优势**:
- 不同头关注不同子空间信息
- 增强表达能力

### 2. Feed-Forward Network

```python
FFN(x) = max(0, xW_1 + b_1)W_2 + b_2
```

- **内层维度**: 4 × d_model (2048 for d_model=512)
- **激活函数**: ReLU (现代变体使用 GELU/SwiGLU)

### 3. Layer Normalization

```python
LayerNorm(x) = γ * (x - μ) / √(σ² + ε) + β
```

**Post-LN vs Pre-LN**:
- **原始**: LayerNorm 在残差之后
- **现代**: LayerNorm 在残差之前 (更稳定)

---

## 🚀 现代优化技术

### 1. RoPE (Rotary Position Embedding)

```python
# 旋转位置编码
# 将位置信息编码到Query/Key中
# 支持外推，长上下文友好
```

**优势**:
- 相对位置感知
- 外推能力强
- 被 LLaMA、PaLM 等采用

### 2. GQA (Grouped Query Attention)

```python
# MHA: 8 Q heads, 8 K/V heads
# MQA: 8 Q heads, 1 K/V head  (节省显存，质量下降)
# GQA: 8 Q heads, 2 K/V heads (平衡方案)
```

**应用**: LLaMA-2、Mixtral

### 3. Flash Attention

```python
# IO-Aware 精确注意力算法
# 分块计算，减少HBM访问
```

**优势**:
- 内存高效
- 计算更快
- 支持更长序列

### 4. MoE (Mixture of Experts)

```
Input
  ↓
[Router] → 选择 Top-k Experts
  ↓
[Expert 1] [Expert 2] ... [Expert N]
  ↓
加权聚合
  ↓
Output
```

**代表模型**: GPT-4、Mixtral 8x7B

---

## 📊 架构演进

| 模型 | 年份 | 参数量 | 创新点 |
|------|------|--------|--------|
| Transformer | 2017 | - | Attention机制 |
| GPT-1 | 2018 | 117M | 生成式预训练 |
| BERT | 2018 | 340M | 双向编码 |
| GPT-2 | 2019 | 1.5B | Zero-shot能力 |
| GPT-3 | 2020 | 175B | Few-shot能力 |
| PaLM | 2022 | 540B | 规模效应 |
| GPT-4 | 2023 | - | 多模态、推理 |
| LLaMA-3 | 2024 | 405B | 开源最强 |

---

## 📚 必读论文

1. [Attention Is All You Need](https://arxiv.org/abs/1706.03762) (Transformer)
2. [RoFormer](https://arxiv.org/abs/2104.09864) (RoPE)
3. [Flash Attention](https://arxiv.org/abs/2205.14135)
4. [GQA](https://arxiv.org/abs/2305.13245)
5. [Switch Transformers](https://arxiv.org/abs/2101.03961) (MoE)

---

*最后更新: 2026-03-08*
