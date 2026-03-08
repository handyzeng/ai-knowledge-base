# DeepSpeed 大模型训练实战

> 微软开源的大模型训练框架

---

## 🎯 核心特性

### ZeRO (Zero Redundancy Optimizer)

| 阶段 | 优化内容 | 显存节省 |
|------|----------|----------|
| **ZeRO-1** | 优化器状态分片 | 4x |
| **ZeRO-2** | + 梯度分片 | 8x |
| **ZeRO-3** | + 参数分片 | 与数据并行度线性相关 |
| **ZeRO-Offload** | CPU/NVMe 卸载 | 单卡训练百亿模型 |

### 3D 并行

```
数据并行 (DP)
    ↓
张量并行 (TP) - 层内切分
    ↓
流水线并行 (PP) - 层间切分
```

---

## 🚀 快速开始

### 安装

```bash
pip install deepspeed
```

### 配置文件 (ds_config.json)

```json
{
  "fp16": {
    "enabled": true,
    "loss_scale": 0,
    "loss_scale_window": 1000
  },
  "zero_optimization": {
    "stage": 2,
    "offload_optimizer": {
      "device": "cpu",
      "pin_memory": true
    },
    "allgather_partitions": true,
    "allgather_bucket_size": 2e8,
    "overlap_comm": true,
    "reduce_scatter": true
  },
  "train_batch_size": "auto",
  "train_micro_batch_size_per_gpu": "auto",
  "gradient_accumulation_steps": "auto"
}
```

### 训练代码

```python
import deepspeed
import torch

# 初始化模型
model = MyLargeModel()

# 初始化 DeepSpeed
deepspeed.init_distributed()
model_engine, optimizer, _, _ = deepspeed.initialize(
    model=model,
    model_parameters=model.parameters(),
    config="ds_config.json"
)

# 训练循环
for batch in data_loader:
    loss = model_engine(batch)
    model_engine.backward(loss)
    model_engine.step()
```

---

## 📊 性能对比

### 训练速度 (GPT-3 175B)

| 配置 | 时间/iter | 加速比 |
|------|-----------|--------|
| 无优化 | 120s | 1x |
| ZeRO-2 | 45s | 2.7x |
| ZeRO-3 | 38s | 3.2x |
| + Offload | 52s | 2.3x (单卡) |

### 显存占用 (GPT-2 1.5B)

| 方法 | 显存 | 可训练模型大小 |
|------|------|----------------|
| 标准 | 28GB | 1.5B |
| ZeRO-2 | 14GB | 3B |
| ZeRO-3 | 6GB | 10B+ |

---

## 💡 最佳实践

### 1. 选择 ZeRO Stage

- **Stage 0**: 标准数据并行
- **Stage 1**: 大模型优化器状态大
- **Stage 2**: 大模型梯度占用大
- **Stage 3**: 超大模型，参数都存不下

### 2. 通信优化

```json
{
  "zero_optimization": {
    "stage": 2,
    "overlap_comm": true,
    "contiguous_gradients": true,
    "sub_group_size": 1e9
  }
}
```

### 3. 混合精度

```json
{
  "bf16": {
    "enabled": true
  }
}
```

---

## 🔗 相关资源

- [DeepSpeed GitHub](https://github.com/microsoft/DeepSpeed)
- [DeepSpeed Documentation](https://www.deepspeed.ai/)
- [ZeRO Paper](https://arxiv.org/abs/1910.02054)

---

*最后更新: 2026-03-08*
