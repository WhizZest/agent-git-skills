---
name: code-review
description: 结构化代码审查框架，覆盖代码质量、安全性、性能和测试标准。适用于 PR 审查、代码质量检查、安全审计、性能分析等场景。
---

# 代码审查

## 适用场景

- 审查 Pull Request
- 检查代码质量
- 对实现提供反馈
- 识别潜在 bug
- 提出改进建议
- 安全审计
- 性能分析

## 审查流程

### 第 1 步：理解上下文

**阅读 PR 描述**：

- 这次改动的目标是什么？
- 解决了哪些 issue？
- 有没有特殊注意事项？

**确认改动范围**：

- 改动了多少个文件？
- 改动类型是什么？（新功能、bug 修复、重构）
- 是否包含测试？

**检查 CI/CD 或本地运行测试**：

- 如果有 CI/CD 状态（GitHub Actions、GitLab CI 等），确认所有检查通过
- 如果是本地审查，在开始前运行项目的测试套件：
  - Node.js：`npm test` 或 `yarn test`
  - Python：`pytest` 或 `python -m unittest`
  - Rust：`cargo test`
  - Go：`go test ./...`
  - Java：`mvn test` 或 `gradle test`
- 如果测试失败，先报告失败信息 —— 测试不通过则不继续审查

### 第 2 步：高层审视

**架构与设计**：

- 整体方案是否合理？
- 是否与现有模式保持一致？
- 有没有更简单的替代方案？
- 代码是否放在了正确的位置？

**代码组织**：

- 关注点是否清晰分离？
- 抽象层级是否恰当？
- 文件/目录结构是否合理？

### 第 3 步：详细代码审查

**命名**：

- 变量：描述性强、有意义
- 函数：动词开头、目的明确
- 类：名词为主、单一职责
- 常量：真正的常量用 UPPER_CASE
- 避免缩写，除非广为人知

**函数**：

- 单一职责
- 长度合理（理想情况 < 50 行）
- 输入输出清晰
- 尽量减少副作用
- 正确的错误处理

**类与对象**：

- 单一职责原则
- 开闭原则
- 里氏替换原则
- 接口隔离原则
- 依赖倒置原则

**错误处理**：

- 所有错误都被捕获和处理
- 错误信息有意义
- 正确记录日志
- 不允许静默失败
- UI 层给出用户友好的错误提示

**代码质量**：

- 无重复代码（DRY）
- 无死代码
- 无被注释掉的代码
- 无魔法数字
- 格式一致

### 第 4 步：安全审查

**输入校验**：

- 所有用户输入都经过校验
- 类型检查
- 范围检查
- 格式校验

**认证与授权**：

- 正确的认证检查
- 敏感操作有授权控制
- 会话管理
- 密码处理（哈希、加盐）

**数据保护**：

- 无硬编码密钥
- 敏感数据加密
- SQL 注入防护
- XSS 防护
- CSRF 防护

**依赖检查**：

- 无已知漏洞的依赖包
- 依赖版本保持更新
- 依赖使用最小化

### 第 5 步：性能审查

**算法**：

- 算法选择恰当
- 时间复杂度合理
- 空间复杂度合理
- 无不必要的循环

**数据库**：

- 查询高效
- 索引合理
- 防止 N+1 查询
- 连接池使用

**缓存**：

- 缓存策略恰当
- 缓存失效已处理
- 内存使用合理

**资源管理**：

- 文件正确关闭
- 连接正确释放
- 防止内存泄漏

### 第 6 步：测试审查

**测试覆盖**：

- 新代码有单元测试
- 必要时有集成测试
- 边界情况已覆盖
- 错误情况已测试

**测试质量**：

- 测试可读
- 测试可维护
- 测试确定性（不依赖随机/外部状态）
- 测试之间无相互依赖
- 测试数据正确 setup/teardown

**测试命名**：

```
# 好的命名
def test_user_creation_with_valid_data_succeeds():
    pass

# 差的命名
def test1():
    pass
```

### 第 7 步：文档审查

**代码注释**：

- 复杂逻辑有解释
- 没有废话注释（不注释显而易见的代码）
- TODO 有关联的 ticket
- 注释内容准确

**函数文档**：

```
def calculate_total(items: List[Item], tax_rate: float) -> Decimal:
    """
    Calculate the total price including tax.

    Args:
        items: List of items to calculate total for
        tax_rate: Tax rate as decimal (e.g., 0.1 for 10%)

    Returns:
        Total price including tax

    Raises:
        ValueError: If tax_rate is negative
    """
    pass
```

**README/文档**：

- README 已按需更新
- API 文档已更新
- 如有破坏性变更，提供迁移指南

**外部引用与工具推荐**：

- 推荐的工具是否仍在维护、未被废弃？（不确定时主动搜索确认）
- 工具分类是否准确（如 Linter vs Formatter、安全工具 vs 代码质量平台）？
- 外部链接是否有效、引用的版本是否最新？

### 第 8 步：提供反馈

**建设性反馈**：

```
✅ 好的反馈：
"建议将这段逻辑提取为独立函数，以提高可测试性和可复用性：

def validate_email(email: str) -> bool:
    return '@' in email and '.' in email.split('@')[1]

这样更容易测试，也能在整个代码库中复用。"

❌ 差的反馈：
"这是错的，重写。"
```

**具体明确**：

```
✅ 好的反馈：
"第 45 行的查询可能导致 N+1 问题。建议使用
.select_related('author') 在单次查询中获取关联对象。"

❌ 差的反馈：
"这里有性能问题。"
```

**问题分级**：

- 🔴 严重（Critical）：安全问题、数据丢失、重大 bug
- 🟡 重要（Important）：性能问题、可维护性问题
- 🟢 锦上添花（Nice-to-have）：代码风格、小改进

**肯定好的工作**：

```
"策略模式用得很好！这样以后添加新的支付方式会非常方便。"
```

## 审查检查清单

### 功能正确性

- 代码实现了预期功能
- 边界情况已处理
- 错误情况已处理
- 无明显 bug

### 代码质量

- 命名清晰、有描述性
- 函数短小精悍、职责聚焦
- 无重复代码
- 与代码库风格一致
- 无代码坏味道

### 安全性

- 输入校验
- 无硬编码密钥
- 认证/授权
- 无 SQL 注入漏洞
- 无 XSS 漏洞

### 性能

- 无明显瓶颈
- 算法高效
- 数据库查询合理
- 资源管理正确

### 测试

- 包含测试
- 测试覆盖良好
- 测试可维护
- 边界情况已测试

### 文档

- 代码自文档化
- 必要处有注释
- 文档已更新
- 破坏性变更已记录
- 推荐工具和外部引用准确、未过时

## 常见问题

### 反模式

**上帝类**：

```
# 差：一个类包揽一切
class UserManager:
    def create_user(self): pass
    def send_email(self): pass
    def process_payment(self): pass
    def generate_report(self): pass
```

**魔法数字**：

```
# 差
if user.age > 18:
    pass

# 好
MINIMUM_AGE = 18
if user.age > MINIMUM_AGE:
    pass
```

**深层嵌套**：

```
# 差
if condition1:
    if condition2:
        if condition3:
            if condition4:
                # 嵌套过深的代码

# 好（提前返回）
if not condition1:
    return
if not condition2:
    return
if not condition3:
    return
if not condition4:
    return
# 扁平的代码
```

### 安全漏洞

**SQL 注入**：

```
# 差
query = f"SELECT * FROM users WHERE id = {user_id}"

# 好
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))
```

**XSS**：

```
// 差
element.innerHTML = userInput;

// 好
element.textContent = userInput;
```

**硬编码密钥**：

```
# 差
API_KEY = "sk-1234567890abcdef"

# 好
API_KEY = os.environ.get("API_KEY")
```

## 最佳实践

1. **及时审查**：不要让提交者等待太久
2. **保持尊重**：关注代码，而非个人
3. **解释原因**：不仅指出问题，还要说明为什么是问题
4. **给出替代方案**：展示更好的做法
5. **用示例说话**：代码示例能让反馈更清晰
6. **抓大放小**：聚焦重要问题
7. **肯定好的工作**：正面反馈同样重要
8. **先自审**：提交前自己先过一遍，发现明显问题
9. **善用自动化工具**：让工具处理代码风格问题
10. **标准一致**：对所有代码应用相同的审查标准

## 推荐工具

**Linter & Formatter**：

- Python：pylint, flake8 (Linter), black (Formatter)
- Node.js：eslint (Linter), prettier (Formatter)
- Go：golangci-lint (Linter), gofmt (Formatter)
- Rust：clippy (Linter), rustfmt (Formatter)

**安全工具**：

- Bandit（Python）
- npm audit（Node.js）
- OWASP Dependency-Check

**代码质量平台**：

- SonarQube
- CodeClimate
- Codacy

## 参考资料

- [Google 代码审查规范](https://google.github.io/eng-practices/review/)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)
- [《代码整洁之道》Robert C. Martin](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)
