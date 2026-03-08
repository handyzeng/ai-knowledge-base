# ICML 2023 Best Papers

> ICML 2023 获奖论文精读

---

## 🏆 Test of Time Award

### Adam: A Method for Stochastic Optimization

**作者**: Kingma & Ba (2014)

**贡献**: 深度学习优化器的事实标准，引用10万+

---

## 🏆 Best Paper Awards

### 1. Symbolic Discovery of Optimization Algorithms

**作者**: Chen et al. (Google DeepMind)

#### 核心贡献

使用符号回归自动发现优化算法，提出Lion优化器：

#### 方法

```python
# 符号搜索空间
update = f(sign(gradient), sign(momentum), ...)

# 搜索最优符号表达式
# 发现: sign(momentum) * lr
```

#### Lion优化器

```python
# 比Adam更省内存
# 只使用符号，不存储二阶矩

m = β1 * m + (1-β1) * g  # 动量
θ = θ - lr * sign(m)      # 符号更新
```

#### 性能

| 任务 | Adam | Lion | 提升 |
|------|------|------|------|
| ImageNet | 76.5% | 77.0% | +0.5% |
 | GPT训练 | 基线 | - | 2x内存节省 |

---

### 2. Diffusion Models Beat GANs on Image Synthesis

**作者**: Dhariwal & Nichol (OpenAI)

#### 核心贡献

证明扩散模型可以超越GAN的图像质量：

#### 关键技术

1. **自适应组归一化**
2. **分类器引导**: 利用类别信息
3. **渐进式增长**: 从低分辨率到高分辨率

#### 结果

- ImageNet FID: 4.59 (超越StyleGAN-XL的5.22)
- 首次证明扩散模型可以SOTA
- 为DALL-E 2、Stable Diffusion铺路

---

## 🔗 相关资源

- [ICML 2023 Proceedings](https://proceedings.mlr.press/v202/)

---

*整理日期: 2026-03-08*
