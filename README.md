# 🤖 AI Knowledge Base | 人工智能知识库

[![GitHub stars](https://img.shields.io/github/stars/handyzeng/ai-knowledge-base?style=social)](https://github.com/handyzeng/ai-knowledge-base/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/handyzeng/ai-knowledge-base?style=social)](https://github.com/handyzeng/ai-knowledge-base/network)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Last Updated](https://img.shields.io/badge/Last%20Updated-2026--03--08-brightgreen)](https://github.com/handyzeng/ai-knowledge-base/commits/main)

> 系统化的AI技术知识体系，涵盖从底层硬件到上层应用的全栈技术栈
> 
> Systematic AI technology knowledge base, covering the full stack from hardware to applications

[English Version](#english-version) | [中文版本](#中文版本)

---

## 📚 知识库概览 | Knowledge Base Overview

本知识库致力于构建最全面的AI技术知识体系，包含：

- 🏗️ **完整技术栈**: 从芯片架构到应用落地的8大技术领域
- 📄 **核心文档**: 40+ 技术文档，3500+ 行深度内容
- 🏆 **顶会论文**: 18篇 NeurIPS/ICML/ICLR Best Paper 精读
- 🚀 **前沿趋势**: 2024-2025 最新技术突破与行业动态

This knowledge base aims to build the most comprehensive AI technology knowledge system:

- 🏗️ **Full Stack**: 8 major technical domains from chip architecture to applications
- 📄 **Core Documents**: 40+ technical documents, 3500+ lines of in-depth content
- 🏆 **Top Conference Papers**: 18 NeurIPS/ICML/ICLR Best Paper readings
- 🚀 **Cutting-edge Trends**: 2024-2025 latest tech breakthroughs and industry dynamics

---

## 📂 知识库结构 | Repository Structure

```
ai-knowledge-base/
├── 📁 01-硬件基础设施/          # Hardware Infrastructure
│   ├── 📁 1.1-AI芯片架构/       # AI Chip Architecture
│   ├── 📁 1.2-服务器与集群/     # Servers & Clusters
│   └── 📁 1.3-边缘计算设备/     # Edge Computing
├── 📁 02-系统软件层/            # System Software
│   ├── 📁 2.1-操作系统与虚拟化/ # OS & Virtualization
│   ├── 📁 2.2-编译器与优化/     # Compilers & Optimization
│   └── 📁 2.3-分布式系统/       # Distributed Systems
├── 📁 03-AI框架与引擎/          # AI Frameworks & Engines
│   ├── 📁 3.1-深度学习框架/     # Deep Learning Frameworks
│   ├── 📁 3.2-大模型训练框架/   # LLM Training Frameworks
│   └── 📁 3.3-推理引擎/         # Inference Engines
├── 📁 04-算法与模型/            # Algorithms & Models
│   ├── 📁 4.1-基础算法/         # Fundamental Algorithms
│   ├── 📁 4.2-计算机视觉/       # Computer Vision
│   ├── 📁 4.3-自然语言处理/     # Natural Language Processing
│   ├── 📁 4.4-多模态与跨模态/   # Multimodal & Cross-modal
│   └── 📁 4.5-大语言模型/       # Large Language Models
├── 📁 05-开发工具链/            # Development Toolchain
│   ├── 📁 5.1-数据处理/         # Data Processing
│   ├── 📁 5.2-模型开发/         # Model Development
│   └── 📁 5.3-部署与监控/       # Deployment & Monitoring
├── 📁 06-应用与解决方案/        # Applications & Solutions
│   ├── 📁 6.1-行业应用/         # Industry Applications
│   ├── 📁 6.2-垂直领域/         # Vertical Domains
│   └── 📁 6.3-生成式AI应用/     # Generative AI Applications
├── 📁 07-数据与资源/            # Data & Resources
│   ├── 📁 7.1-数据集资源/       # Dataset Resources
│   ├── 📁 7.2-开源项目/         # Open Source Projects
│   └── 📁 7.3-学习资源/         # Learning Resources
├── 📁 08-前沿研究/              # Frontier Research
│   ├── 📁 8.1-新兴技术方向/     # Emerging Technologies
│   └── 📁 8.2-技术趋势/         # Technology Trends
└── 📁 09-论文精读/              # Paper Readings
    ├── 📁 NeurIPS-2024/
    ├── 📁 ICML-2024/
    ├── 📁 ICLR-2024/
    ├── 📁 NeurIPS-2023/
    ├── 📁 ICML-2023/
    └── 📁 NeurIPS-2022/
```

---

## 🎯 核心内容 | Core Contents

### 🔥 热门技术详解 | Hot Tech Deep Dives

| 技术 | 文档 | 关键词 |
|------|------|--------|
| **GPU架构** | [GPU架构与原理](01-硬件基础设施/1.1-AI芯片架构/GPU架构与原理.md) | NVIDIA, Tensor Core, HBM |
| **DeepSpeed** | [DeepSpeed实战](03-AI框架与引擎/3.2-大模型训练框架/DeepSpeed.md) | ZeRO, 3D并行, 训练优化 |
| **Transformer** | [Transformer详解](04-算法与模型/4.5-大语言模型/Transformer架构详解.md) | Attention, RoPE, Flash Attention |
| **推理优化** | [TensorRT与vLLM](03-AI框架与引擎/3.3-推理引擎/TensorRT与vLLM推理优化.md) | PagedAttention, FP8, 吞吐量 |
| **RAG** | [RAG实战](04-算法与模型/4.5-大语言模型/RAG检索增强生成.md) | 向量检索, 混合检索, 企业级架构 |
| **AI Agent** | [AI Agent](04-算法与模型/4.5-大语言模型/AI-Agent智能体.md) | ReAct, 工具使用, 多Agent协作 |

### 🏆 顶会论文精读 | Top Conference Papers

#### NeurIPS 2024
- [Scaling Laws for Reward Model Overoptimization](03-论文精读/NeurIPS-2024/NeurIPS-2024-Best-Papers.md)
- [Vision Transformers Need Registers](03-论文精读/NeurIPS-2024/NeurIPS-2024-Best-Papers.md)
- [Mechanistic Understanding of LLMs](03-论文精读/NeurIPS-2024/NeurIPS-2024-Best-Papers.md)

#### ICML 2024
- [DPO: Direct Preference Optimization](03-论文精读/ICML-2024/ICML-2024-Best-Papers.md)
- [Mamba: Linear-Time Sequence Modeling](03-论文精读/ICML-2024/ICML-2024-Best-Papers.md)
- [Consistency Models](03-论文精读/ICML-2024/ICML-2024-Best-Papers.md)

#### ICLR 2024
- [Generalization in Deep Learning](03-论文精读/ICLR-2024/ICLR-2024-Outstanding-Papers.md)
- [Understanding Diffusion Models](03-论文精读/ICLR-2024/ICLR-2024-Outstanding-Papers.md)
- [In-context Learning and Induction Heads](03-论文精读/ICLR-2024/ICLR-2024-Outstanding-Papers.md)

[查看更多论文...](09-论文精读/)

---

## 🚀 前沿趋势 2024-2025 | Frontier Trends

### 芯片硬件突破
- **NVIDIA Blackwell B200**: 20 petaflops FP4, 训练成本降低25倍
- **AMD MI300X**: 192GB HBM3, 性价比超H100 40-60%
- **边缘AI**: Apple A17 Pro (35 TOPS), Snapdragon 8 Gen 3 (45 TOPS)

### 推理引擎革新
- **vLLM PagedAttention**: 吞吐量提升2-4倍
- **推测解码**: 速度提升2-3倍，质量无损
- **FP8量化**: 精度损失<1%，速度提升2倍

### 长上下文技术
- **Gemini 1.5 Pro**: 200万token上下文
- **Ring Attention**: 无限上下文扩展
- **StreamingLLM**: 400万token处理

[查看完整趋势报告...](08-前沿研究/8.2-技术趋势/)

---

## 🗺️ 学习路径 | Learning Paths

### 路径一：AI系统工程师
```
硬件基础设施 → 系统软件层 → AI框架与引擎 → 部署与监控
```

### 路径二：算法研究员
```
基础算法 → 专业方向(CV/NLP/多模态) → 前沿研究
```

### 路径三：AI应用开发者
```
AI框架 → 开发工具链 → 应用与解决方案 → 行业实践
```

### 路径四：全栈AI工程师
```
硬件基础 → 系统软件 → 框架引擎 → 算法模型 → 应用开发
```

---

## 📊 知识库统计 | Statistics

| 指标 | 数据 |
|------|------|
| **总文档数** | 40+ |
| **总目录数** | 33个 |
| **核心论文** | 18篇 Best Papers |
| **代码示例** | 50+ |
| **总内容量** | 5000+ 行 |
| **覆盖领域** | 8大技术领域 |

---

## 🛠️ 使用方法 | How to Use

### 快速导航

**按技术栈查阅**:
1. 从[硬件基础设施](01-硬件基础设施/)开始，逐层深入
2. 每个目录都有README提供导航

**按角色查阅**:
- 👨‍💻 **AI工程师**: 关注[AI框架](03-AI框架与引擎/)和[部署](05-开发工具链/5.3-部署与监控/)
- 🔬 **研究员**: 深入[算法模型](04-算法与模型/)和[论文精读](09-论文精读/)
- 🏗️ **架构师**: 查看[硬件](01-硬件基础设施/)和[系统软件](02-系统软件层/)
- 👨‍🎓 **学习者**: 从[基础算法](04-算法与模型/4.1-基础算法/)开始

**按问题查阅**:
- 搜索关键词，找到相关技术文档
- 查看论文精读获取理论基础

### 内容标准

每个技术文档包含：
- ✅ **概念定义**: 清晰的技术概念解释
- ✅ **原理说明**: 深入的工作原理分析
- ✅ **实践案例**: 可运行的代码示例
- ✅ **资源链接**: 相关论文和工具链接

---

## 🔗 相关链接 | Related Links

- [飞书知识库](https://xhlv6wtom9.feishu.cn/wiki/RSKCwxLEsiwvQekUZsIcvP4Vnxd) - 同步更新的飞书文档
- [Notion知识库](https://www.notion.so/) - 个人知识管理
- [Papers with Code](https://paperswithcode.com/) - 论文与代码
- [arXiv](https://arxiv.org/) - 最新论文预印本

---

## 🤝 贡献指南 | Contributing

欢迎贡献！您可以通过以下方式参与：

1. **提交Issue**: 报告错误或建议新内容
2. **Pull Request**: 添加新的技术文档或改进现有内容
3. **分享**: 将这个知识库分享给更多人

### 贡献规范

- 保持文档结构清晰
- 添加必要的代码示例
- 引用可靠的来源
- 使用中文或双语编写

---

## 📜 许可证 | License

本项目采用 [MIT License](LICENSE) 开源许可证。

---

## 🙏 致谢 | Acknowledgments

- 感谢所有开源社区的贡献者
- 感谢顶级会议(NeurIPS/ICML/ICLR)的作者们
- 感谢AI领域的先驱研究者们

---

## 📧 联系 | Contact

如有问题或建议，欢迎通过以下方式联系：

- GitHub Issues: [提交问题](https://github.com/handyzeng/ai-knowledge-base/issues)
- Email: zeng.handy@gmail.com

---

<p align="center">
  <b>⭐ Star this repo if you find it helpful!</b><br>
  <b>如果觉得有帮助，请给这个仓库点个星！</b>
</p>

<p align="center">
  <a href="https://github.com/handyzeng/ai-knowledge-base/stargazers">
    <img src="https://img.shields.io/github/stars/handyzeng/ai-knowledge-base?style=social" alt="GitHub Stars">
  </a>
</p>

---

*本知识库由 AI 助手协助整理维护*<br>
*Last Updated: 2026-03-08*<br>
*Version: 2.0*
