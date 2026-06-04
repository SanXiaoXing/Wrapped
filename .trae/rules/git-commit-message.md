---
alwaysApply: true
scene: git_message
---

### Git提交规范

本项目遵循 **Conventional Commits** 规范，确保提交历史清晰、可追溯，并支持自动生成变更日志（Changelog）。

#### 1. 提交消息格式

提交消息应包含以下三个部分：

<type>(<scope>): <subject>

<body>

<footer>

- **Header（必填）**: 包含类型、范围（可选）和简短描述。
- **Body（可选）**: 详细描述修改动机、实现细节及与之前行为的对比。
- **Footer（可选）**: 包含不兼容变动（Breaking Changes）或关闭的Issue引用。

#### 2. Type 类型说明

| 类型 | 说明 | 示例 |
|------|------|------|
| `feat` | 新功能（Feature） | `feat(driver): 添加CHR34205驱动支持` |
| `fix` | 修复Bug | `fix(ui): 修复主界面崩溃问题` |
| `docs` | 文档更新 | `docs: 更新开发指南中的驱动注册步骤` |
| `style` | 代码格式调整（不影响逻辑） | `style: 统一使用Black格式化代码` |
| `refactor` | 代码重构（非新功能也非修复Bug） | `refactor(config): 优化配置加载逻辑` |
| `perf` | 性能优化 | `perf(read): 提高数据读取速度` |
| `test` | 测试相关（新增或修改测试） | `test(driver): 添加NEW_DRIVER单元测试` |
| `chore` | 构建过程或辅助工具的变动 | `chore(deps): 升级PyQt5版本` |
| `ci` | CI配置文件或脚本的修改 | `ci: 更新GitHub Actions工作流` |

#### 3. Scope 范围说明（可选）

Scope 用于说明 commit 影响的范围，建议根据项目模块划分：

- `driver`: 驱动相关
- `component`: UI组件相关
- `config`: 配置文件相关
- `test`: 测试用例相关
- `core`: 核心逻辑相关
- `ui`: 界面交互相关

#### 4. Subject 主题说明

- 使用祈使句、现在时态（如 "add" 而非 "added" 或 "adds"）
- 首字母小写
- 结尾不加句号（.）
- 长度控制在 50 个字符以内

#### 5. Body 正文说明（可选）

- 详细说明为什么做这个修改，以及修改前后的行为对比
- 可以分点陈述，每行不超过 72 个字符
- 如果修改复杂，务必在此处解释设计思路

#### 6. Footer 脚注说明（可选）

- **不兼容变动**: 以 `BREAKING CHANGE:` 开头，后跟空格或两个换行符，描述变动内容及迁移方法
- **关闭Issue**: 使用 `Closes #123` 或 `Fixes #456` 引用相关的Issue

#### 7. 完整示例

**示例1：简单功能添加**

feat(driver): 添加NEW_DRIVER驱动支持

- 实现Driver_NEW_DRIVER类
- 添加setup, read, close接口
- 注册到drivers/__init__.py

Closes #101

**示例2：Bug修复**

fix(ui): 修复长时间运行后界面卡顿问题

- 由于信号槽连接未正确断开，导致内存泄漏。
- 在组件销毁时显式断开所有信号连接。

BREAKING CHANGE: 无

**示例3：文档更新**

docs: 更新开发指南中的Git提交规范

- 补充Scope范围说明和Body正文编写建议
