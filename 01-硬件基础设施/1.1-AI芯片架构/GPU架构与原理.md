# GPU 架构与原理

> NVIDIA GPU 架构演进与技术详解

---

## 📈 NVIDIA GPU 架构演进

### 架构时间线

| 架构 | 时间 | 代表产品 | 核心特性 |
|------|------|----------|----------|
| **Kepler** | 2012 | GTX 680 | 首次支持 GPU Direct |
| **Maxwell** | 2014 | GTX 980 | 能效比提升 |
| **Pascal** | 2016 | GTX 1080, P100 | NVLink, HBM2 |
| **Volta** | 2017 | V100 | **第一代 Tensor Core** |
| **Turing** | 2018 | RTX 2080, T4 | RT Core, 第二代 Tensor Core |
| **Ampere** | 2020 | A100, RTX 3090 | 第三代 Tensor Core, MIG |
| **Hopper** | 2022 | H100 | **Transformer Engine**, FP8 |
| **Blackwell** | 2024 | B200 | **第二代 Transformer Engine**, 20 petaflops FP4 |

---

## 🧠 核心组件

### CUDA Core
- **功能**: 通用计算单元
- **特点**: 支持 FP32/FP64/INT 运算
- **演进**: 从 Kepler 到 Blackwell 持续优化

### Tensor Core
- **Volta (V100)**: 第一代，支持 FP16
- **Turing**: 第二代，支持 INT8/INT4
- **Ampere**: 第三代，支持 TF32/BF16
- **Hopper**: 第四代，支持 FP8，Transformer Engine
- **Blackwell**: 第五代，FP4 精度，AI 性能提升 4 倍

### 显存架构

| 类型 | 带宽 | 应用 |
|------|------|------|
| GDDR5/X | 336-512 GB/s | 消费级显卡 |
| HBM2 | 732-900 GB/s | P100/V100 |
| HBM2e | 1.2-1.6 TB/s | A100 |
| HBM3 | 3.0 TB/s | H100 |
| HBM3e | 5.0+ TB/s | B200 |

---

## 🔗 互联技术

### NVLink
- **NVLink 3.0**: 50 GB/s (Ampere)
- **NVLink 4.0**: 100 GB/s (Hopper)
- **应用**: GPU 间高速通信

### NVSwitch
- **功能**: 全互联拓扑
- **DGX A100**: 6 个 NVSwitch
- **DGX H100**: 4 个 NVSwitch

### InfiniBand
- **NDR**: 400 Gb/s
- **XDR**: 800 Gb/s (即将推出)

---

## 💡 选型指南

### 训练场景

| 模型规模 | 推荐 GPU | 配置 |
|----------|----------|------|
| 7B-13B | A100 40GB | 8x A100 |
| 70B | A100 80GB | 8x A100 |
| 175B+ | H100 80GB | 64x H100 |
| 超大模型 | B200 | DGX B200 |

### 推理场景

| 场景 | 推荐 GPU | 特点 |
|------|----------|------|
| 高吞吐 | A10G / L4 | 性价比 |
| 低延迟 | H100 | FP8 加速 |
| 边缘部署 | Jetson | 低功耗 |

---

## 📚 参考资料

- [NVIDIA GPU 架构白皮书](https://www.nvidia.com/en-us/data-center/technologies/)
- [CUDA Programming Guide](https://docs.nvidia.com/cuda/)
- [Deep Learning Performance Guide](https://docs.nvidia.com/deeplearning/performance/)

---

*最后更新: 2026-03-08*
