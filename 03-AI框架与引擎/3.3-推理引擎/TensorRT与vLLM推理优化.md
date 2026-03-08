# TensorRT 与 vLLM 推理优化

> 生产级 LLM 推理加速方案

---

## 🎯 推理优化核心挑战

| 挑战 | 影响 | 解决方案 |
|------|------|----------|
| **高延迟** | 用户体验差 | 量化、算子融合 |
| **低吞吐** | 成本高 | 批处理、PagedAttention |
| **显存瓶颈** | 无法部署大模型 | KV Cache 优化 |
| **动态形状** | 序列长度不一 | 连续批处理 |

---

## 🚀 vLLM - 高吞吐推理

### PagedAttention 核心创新

**传统问题**:
```
预分配连续内存 → 内部碎片严重 → 显存浪费 60%+
```

**PagedAttention 方案**:
```
借鉴操作系统虚拟内存
┌─────────────────────────────────────┐
│  逻辑块 (Logical Blocks)            │
│  [Block 0] [Block 1] [Block 2] ...  │
└─────────────────────────────────────┘
           ↓ 映射
┌─────────────────────────────────────┐
│  物理块 (Physical Blocks)           │
│  [P-3] [P-1] [P-5] ... 非连续      │
└─────────────────────────────────────┘
```

**优势**:
- ✅ 消除内部碎片
- ✅ 显存利用率 > 90%
- ✅ 吞吐量提升 2-4 倍

### 连续批处理 (Continuous Batching)

**传统静态批处理**:
```
Batch: [Req A(100tokens)] [Req B(10tokens)] [Req C(50tokens)]
→ 等待最慢的 A 完成才能返回
```

**连续批处理**:
```
时间 0:  [A] [B] [C] 一起处理
时间 1:  A完成 → 新请求D加入 → [B] [C] [D]
时间 2:  B完成 → 新请求E加入 → [C] [D] [E]
```

### vLLM 快速开始

```bash
# 安装
pip install vllm

# 启动服务
python -m vllm.entrypoints.openai.api_server \
    --model meta-llama/Llama-2-7b-hf \
    --tensor-parallel-size 1 \
    --max-num-batched-tokens 4096

# 调用 API
curl http://localhost:8000/v1/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "meta-llama/Llama-2-7b-hf",
    "prompt": "San Francisco is a",
    "max_tokens": 7,
    "temperature": 0
  }'
```

---

## ⚡ TensorRT-LLM - NVIDIA 高性能推理

### 核心优化技术

#### 1. 算子融合 (Layer Fusion)

```python
# 融合前: 多个独立算子
LayerNorm → MatMul → Add → Activation

# 融合后: 单个融合算子
FusedLayerNormMatMulAddActivation
```

**效果**: 减少 kernel 启动开销，提升 20-30%

#### 2. FP8 量化推理

```python
# Hopper GPU 原生支持
import tensorrt_llm

config = tensorrt_llm.BuilderConfig(
    precision='fp8',  # FP8 量化
    quantization=QuantMode.FP8_KV_CACHE
)
```

| 精度 | 速度 | 精度损失 |
|------|------|----------|
| FP16 | 1x | 0% |
| FP8 | 2x | <1% |
| INT8 | 1.5x | 2-3% |

#### 3. In-Flight Batching

类似 vLLM 的连续批处理，TensorRT-LLM 原生支持

### TensorRT-LLM 构建流程

```python
# 1. 准备模型
from tensorrt_llm import LLM

llm = LLM(
    model="meta-llama/Llama-2-7b",
    dtype="float16",
    tensor_parallel_size=1
)

# 2. 编译优化
llm.save("./trt_engines/llama-7b")

# 3. 推理
output = llm.generate(
    ["The future of AI is"],
    max_new_tokens=128
)
```

---

## 📊 性能对比

### 吞吐量 (Llama-2 7B, A100)

| 方案 | 吞吐量 (req/s) | 延迟 (ms) |
|------|----------------|-----------|
| HuggingFace | 2.5 | 400 |
| ONNX Runtime | 4.0 | 250 |
| TensorRT-LLM | 8.5 | 120 |
| **vLLM** | **12.0** | **85** |

### 显存效率 (Llama-2 70B)

| 方案 | 显存占用 | 支持并发 |
|------|----------|----------|
| 标准 HF | 140GB | 1 |
| DeepSpeed | 80GB | 2 |
| vLLM | 75GB | 4 |
| vLLM + FP8 | 40GB | 8 |

---

## 💡 生产环境选型建议

### 场景一：高吞吐云端服务

```yaml
推荐: vLLM + A100/H100
配置:
  - 连续批处理: 开启
  - PagedAttention: 开启
  - 量化: FP8 (Hopper)
  - 张量并行: 根据GPU数量
```

### 场景二：低延迟实时应用

```yaml
推荐: TensorRT-LLM + H100
配置:
  - 算子融合: 全部开启
  - 量化: FP8
  - In-Flight Batching: 开启
  - KV Cache 管理: 优化
```

### 场景三：成本敏感场景

```yaml
推荐: vLLM + INT4量化
配置:
  - 模型量化: GPTQ/AWQ
  - KV Cache 量化: 开启
  - 批处理大小: 动态调整
```

---

## 🔗 相关资源

- [vLLM GitHub](https://github.com/vllm-project/vllm)
- [TensorRT-LLM Documentation](https://nvidia.github.io/TensorRT-LLM/)
- [PagedAttention Paper](https://arxiv.org/abs/2309.06180)

---

*最后更新: 2026-03-08*
