# NeurIPS 2022 Best Papers

> NeurIPS 2022 获奖论文精读

---

## 🏆 Best Paper Awards

### 1. Training language models to follow instructions with human feedback (InstructGPT)

**作者**: Ouyang et al. (OpenAI)

#### 核心贡献

提出RLHF方法，使GPT-3能够遵循人类指令：

#### 方法流程

```
1. 收集演示数据，监督微调(SFT)
2. 收集比较数据，训练奖励模型
3. 使用PPO优化策略
```

#### 关键发现

- 1.3B InstructGPT > 175B GPT-3 (人类评价)
- 对齐可以牺牲部分能力换取可用性
- 成为ChatGPT的基础

#### 影响

- 开启对齐研究热潮
- RLHF成为标准做法
- 引发AI安全讨论

---

### 2. A Generalist Agent (Gato)

**作者**: Reed et al. (DeepMind)

#### 核心贡献

单一Transformer处理多种任务：

#### 能力范围

- 游戏（Atari）
- 机器人控制
- 图像描述
- 自然语言

#### 架构

```
单一Transformer
  ↓
游戏帧 + 机器人状态 + 文本
  ↓
统一token化
  ↓
自回归预测
```

#### 意义

- 通用人工智能的探索
- 多模态统一
- 数据规模的重要性

---

### 3. Solving (most) of a set of benchmark problems in satisfiability

**作者**: Selsam et al. (Microsoft Research)

#### 核心贡献

使用神经网络解决SAT问题：

#### 方法

- 图神经网络处理CNF公式
- 神经引导的搜索
- 结合符号和神经方法

---

## 📊 2022年回顾

### 关键进展

1. **对齐研究起步**: InstructGPT、RLHF
2. **通用Agent探索**: Gato、多模态统一
3. **扩散模型崛起**: 开始挑战GAN
4. **神经符号结合**: SAT求解、程序合成

---

*整理日期: 2026-03-08*
