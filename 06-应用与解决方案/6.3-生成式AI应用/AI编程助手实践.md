# AI 编程助手实践

> Copilot、Cursor 等 AI 编程工具深度使用指南

---

## 🛠️ 主流工具对比

| 工具 | 厂商 | 特点 | 价格 |
|------|------|------|------|
| **GitHub Copilot** | GitHub/OpenAI | 代码补全最强 | $10/月 |
| **Cursor** | Anysphere | 聊天式编程 | $20/月 |
| **Codeium** | Codeium | 免费版功能全 | 免费 |
| **Amazon CodeWhisperer** | AWS | 与 AWS 集成 | 免费 |
| **JetBrains AI** | JetBrains | IDE 深度集成 | 订阅制 |

---

## 🚀 Copilot 深度使用

### 核心功能

#### 1. 代码补全

```python
# 输入注释，自动生成代码
# 计算斐波那契数列
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

#### 2. 整块生成

```python
# 输入: "实现一个 LRU Cache"
class LRUCache:
    def __init__(self, capacity: int):
        self.capacity = capacity
        self.cache = {}
        self.order = []
    
    def get(self, key: int) -> int:
        if key in self.cache:
            self.order.remove(key)
            self.order.append(key)
            return self.cache[key]
        return -1
    
    def put(self, key: int, value: int) -> None:
        if key in self.cache:
            self.order.remove(key)
        elif len(self.cache) >= self.capacity:
            oldest = self.order.pop(0)
            del self.cache[oldest]
        self.cache[key] = value
        self.order.append(key)
```

#### 3. 测试生成

```python
# 输入: "为上面的 LRUCache 写单元测试"
import unittest

class TestLRUCache(unittest.TestCase):
    def test_basic(self):
        cache = LRUCache(2)
        cache.put(1, 1)
        self.assertEqual(cache.get(1), 1)
```

### 提示技巧

| 技巧 | 示例 | 效果 |
|------|------|------|
| **详细注释** | `# 使用快速排序算法，时间复杂度O(nlogn)` | 更准确的代码 |
| **示例驱动** | `# 例如: input=[3,1,2], output=[1,2,3]` | 明确期望 |
| **逐步引导** | `# 第一步: 读取文件` | 分块生成 |

---

## 💬 Cursor 聊天式编程

### 核心特性

#### 1. Cmd+K 快速编辑

```python
# 选中代码，按 Cmd+K
# 输入: "添加错误处理"

def divide(a, b):
    # Cursor 自动生成:
    try:
        return a / b
    except ZeroDivisionError:
        raise ValueError("Cannot divide by zero")
```

#### 2. Cmd+L 聊天问答

```
用户: 解释这段代码的作用
Cursor: 这段代码实现了...

用户: 如何优化性能？
Cursor: 建议使用...
```

#### 3. 代码库理解

```
用户: @index 这个项目的架构是什么？
Cursor: 分析整个代码库后，这是一个...
```

### 实际案例

#### 案例 1: 重构代码

```python
# 原始代码（混乱）
def process(data):
    r = []
    for i in data:
        if i > 0:
            r.append(i*2)
    return r

# Cursor 重构后
def filter_and_double(positive_numbers: list[int]) -> list[int]:
    """过滤正数并翻倍"""
    return [num * 2 for num in positive_numbers if num > 0]
```

#### 案例 2: Debug

```
错误信息: IndexError: list index out of range

Cursor 分析:
1. 问题在第 23 行
2. 原因是列表为空时访问了索引 0
3. 建议添加边界检查
```

---

## 📊 效率提升数据

| 指标 | 提升 |
|------|------|
| 代码编写速度 | 55% faster |
| 重复代码减少 | 40% less |
| 文档编写时间 | 50% less |
| Bug 修复速度 | 30% faster |

---

## 💡 最佳实践

### 1. 渐进式采用

```
Week 1: 代码补全
Week 2: 函数生成
Week 3: 测试生成
Week 4: 代码审查
```

### 2. 安全注意事项

⚠️ **永远不要**:
- 直接提交 AI 生成的代码不审查
- 让 AI 处理敏感信息
- 完全依赖 AI 做架构设计

✅ **应该**:
- 始终审查生成的代码
- 理解代码后再使用
- 结合领域知识判断

### 3. 提示工程

```python
# 好的提示
"""
实现一个线程安全的单例模式
要求:
1. 使用双检锁
2. 支持延迟初始化
3. 添加类型注解
"""

# 不好的提示
"写个单例"
```

---

## 🔧 自定义与集成

### Copilot 自定义提示

```json
// .github/copilot-instructions.md
## 项目规范

### 代码风格
- 使用 TypeScript 严格模式
- 函数长度不超过 50 行
- 必须添加 JSDoc 注释

### 命名规范
- 组件: PascalCase
- 函数: camelCase
- 常量: UPPER_SNAKE_CASE
```

### 与 CI/CD 集成

```yaml
# .github/workflows/ai-review.yml
name: AI Code Review
on: [pull_request]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: AI Review
        run: |
          # 调用 AI 进行代码审查
```

---

## 🔗 相关资源

- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Cursor Documentation](https://cursor.sh/docs)
- [Codeium](https://www.codeium.com/)

---

*最后更新: 2026-03-08*
