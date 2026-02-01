# agent-browser Skill 实施计划

## 目标

创建一个完整的 agent-browser skill，让 Claude Code 用户能够快速使用 agent-browser 替代 Playwright MCP，实现：
- **更快的执行速度**（守护进程持久化）
- **更低的 token 消耗**（节省 93%）
- **AI 友好的语义定位器系统**

---

## 架构确认

agent-browser 采用 **Rust CLI + Node.js Daemon** 原生架构：
- CLI 通过 Unix socket 与守护进程通信
- 浏览器实例持久化，无需额外包装器脚本
- **状态**: ✅ 已验证

---

## 架构决策：混合方案（推荐）

### 选择理由

1. **现有投资利用**：项目已有完整的插件结构（agents/、skills/、commands/、rules/、hooks/）
2. **渐进增强路径**：可逐步添加功能，无需大规模重构
3. **用户体验优先**：`/plugin install` 比手动复制文件更简单
4. **模块化灵活性**：用户可选择安装需要的部分

### 文件结构

```
agent-browser-claude-plugin/
├── skills/
│   └── agent-browser/              # 新增核心 skill
│       ├── SKILL.md                # 核心文档 (~300 行)
│       └── references/             # Progressive disclosure
│           ├── semantic-locators.md
│           ├── ref-system.md
│           ├── advanced-waiting.md
│           ├── network-storage.md
│           ├── debugging.md
│           ├── sessions-profiles.md
│           └── api-complete.md
├── commands/
│   ├── find.md                     # 新增
│   └── browse.md                   # 更新
├── agents/
│   └── browser-automation.md        # 更新
├── rules/
│   └── browser-automation.md        # 更新
└── CLAUDE.md                        # 更新
```

---

## 功能覆盖对比

### agent-browser 核心优势（vs Playwright MCP）

| 特性 | agent-browser | Playwright MCP |
|------|--------------|----------------|
| Token 节省 | **93%** (守护进程持久化) | 每次重启浏览器 |
| AI 友好性 | **语义定位器** (find role/text/label) | CSS 选择器 |
| 启动速度 | 首次慢，后续**即时** | 每次慢 |
| Ref 系统 | **原生支持** | 无 |
| JSON 输出 | `--json` 减少输出 token | 通常输出详细文本 |
| 可视化操作 | **`--headed` 实时可见** | 通常无头模式 |
| Cookie 持久化 | **`--profile` 自动保存** | 需手动处理 |

### 当前插件覆盖分析

```
已覆盖：导航、基础交互、数据提取、截图
核心缺失：语义定位器系统、Ref 系统、高级等待机制
未覆盖：网络拦截、存储管理、调试功能、多标签页、设备模拟
```

---

## 实施步骤

### Phase 1: 核心 Skill（必须）

1. **✅ 创建 `skills/agent-browser/SKILL.md`**
   - YAML frontmatter: 清晰描述 skill 用途
   - **前置条件检查**：检测 agent-browser 是否安装，未安装时引导安装
   - Quick Start: 核心 workflow（snapshot + ref）
   - 语义定位器快速参考
   - **最佳实践**：
     - 强制使用 `--profile <session_id>` 防止状态污染
     - 使用 `--json` 标志减少输出 token
     - **可视化操作**：默认使用 `--headed` 让用户看到浏览器操作
     - **Cookie 持久化**：通过 `--profile` 自动保存登录状态
     - Ref ID 失效后重新 snapshot 的错误恢复策略
   - 安全警告：file:/// 路径访问需用户明确授权
   - 链接到 references

2. **✅ 创建 `references/semantic-locators.md`**
   - find role/text/label/placeholder/alt/testid 完整指南
   - 所有动作类型（click, fill, hover, check）
   - 修饰符（first, last, nth）

3. **✅ 创建 `references/ref-system.md`**
   - snapshot 工作流
   - ref 使用方法
   - ref 持久性说明
   - **错误恢复**：Ref ID 失效时的重新 snapshot 策略

4. **✅ 创建 `references/api-complete.md`**
   - 完整命令速查表
   - 所有选项说明
   - **JSON 输出**：标注哪些命令支持 `--json`

5. **✅ 更新 `.claude-plugin/plugin.json`**
   - 添加新 skill 引用

### Phase 2: 高级功能（推荐）

6. **✅ 创建 `references/visual-mode.md`** ⭐ 新增
   - `--headed` 模式完整指南
   - 可视化操作的优势和场景
   - 用户实时监控 AI 操作
   - 调试和演示用途

7. **✅ 创建 `references/sessions-profiles.md`** ⭐ 重点更新
   - 多会话管理
   - 持久化配置
   - **Cookie 持久化完整指南**
   - **登录状态保存和恢复**
   - **Profile 数据结构说明**
   - 会话隔离最佳实践

8. **✅ 创建 `references/advanced-waiting.md`**
   - wait --text/--url/--fn 用法
   - 加载状态说明

9. **✅ 创建 `references/network-storage.md`**
   - 网络拦截和模拟
   - Cookies 和存储管理

10. **✅ 创建 `references/debugging.md`**
    - console、errors、trace
    - 调试工作流

### Phase 3: Commands 更新（可选）

10. **✅ 创建 `commands/find.md`**
    - 语义查找命令文档

11. **✅ 更新 `commands/browse.md`**
    - 添加语义定位器示例

### Phase 4: 现有组件更新（可选）

12. **✅ 更新 `agents/browser-automation.md`**
    - 添加 profile 隔离最佳实践
    - 添加 JSON 输出使用说明
13. **✅ 更新 `rules/browser-automation.md`**
    - 添加会话隔离规则
    - 添加错误恢复策略
14. **更新现有 skills**（web-scraping, form-automation 等）
15. **更新 `CLAUDE.md`**

---

## 核心 SKILL.md 结构

```yaml
---
name: agent-browser
description: Complete browser automation using agent-browser CLI with AI-friendly semantic locators, ref system, and advanced waiting. Use when Claude needs to: (1) Navigate and interact with web pages, (2) Extract structured data, (3) Fill forms, (4) Take screenshots, (5) Run E2E tests, or (6) Debug web applications. Covers semantic locators (role/text/label/placeholder), snapshot+ref workflow, session isolation with --profile, JSON output for token efficiency, and error recovery strategies.
---
```

### 内容大纲

```markdown
# agent-browser

AI-first browser automation CLI with semantic locators and ref system.

## Prerequisites Check

First, verify agent-browser is installed:

```bash
# Check if agent-browser is available
if ! command -v agent-browser &> /dev/null; then
    echo "agent-browser not found. Installing..."
    npm install -g agent-browser
    agent-browser install  # Download Chromium
else
    echo "agent-browser is installed: $(agent-browser --version)"
fi
```

## Quick Start

### Core Workflow (Recommended - Visual Mode)
1. Navigate with visible browser: `agent-browser --profile mysession --headed open <url>`
2. Snapshot: `agent-browser --profile mysession snapshot -i`
3. Use refs: `agent-browser --profile mysession click @e2`
4. Re-snapshot after page changes

### Alternative: Semantic Locators
```bash
agent-browser --profile mysession --headed find role button click --name "Submit"
agent-browser --profile mysession --headed find label "Email" fill "test@test.com"
```

## Best Practices

### Visual Mode (Recommended for Users)
Use `--headed` to show browser window - users can see AI operating in real-time:
```bash
# GOOD - Visible browser (user sees what's happening)
agent-browser --profile mysession --headed open https://example.com

# Acceptable - Headless mode (background, faster)
agent-browser --profile mysession open https://example.com
```

### Session Isolation & Cookie Persistence (REQUIRED)
Always use `--profile <session_id>` for state isolation and automatic cookie saving:

```bash
# GOOD - Isolated session with automatic cookie persistence
agent-browser --profile github_session --headed open https://github.com/login
# ... login process ...
# Cookies and login state are automatically saved to github_session profile

# Next time - no need to login again!
agent-browser --profile github_session --headed open https://github.com/dashboard
# Automatically logged in, cookies restored

# BAD - Shared state (cookies may mix between tasks)
agent-browser open https://example.com
agent-browser open https://another.com
```

**Profile Persistence Benefits:**
- **Login sessions** are saved across runs
- **Cookies** persist automatically
- **localStorage** and **sessionStorage** are preserved
- **Multiple isolated sessions** can run simultaneously

### Token Efficiency
Use `--json` flag where available to reduce output tokens:
```bash
agent-browser --json get title
agent-browser --json get text .content
```

### Error Recovery
If Ref ID fails ("Node not found" or "Detach"), re-snapshot:
```bash
# Ref fails
agent-browser click @e2  # Error: Node not found

# Recovery: Re-snapshot and get new ref
agent-browser snapshot -i  # Now the button is @e5
agent-browser click @e5
```

### Security Warning
Access to `file:///` local paths requires explicit user authorization.

## Core Commands

[精简的命令参考表，标注 --json 支持]

## Semantic Locators

[AI 友好的定位器快速参考]

## Ref System

[snapshot + ref 工作流，包含错误恢复]

## Troubleshooting

[常见问题解决方案]

## Advanced Features

[链接到 references/ 目录下的详细文档]
```

---

## Critical Files

| 文件 | 操作 | 描述 |
|------|------|------|
| `skills/agent-browser/SKILL.md` | 创建 | 核心 skill 文件（含可视化、Cookie 持久化、安装检测） |
| `skills/agent-browser/references/semantic-locators.md` | 创建 | 语义定位器完整指南 |
| `skills/agent-browser/references/ref-system.md` | 创建 | Ref 系统完整指南（含错误恢复） |
| `skills/agent-browser/references/api-complete.md` | 创建 | 完整 API 参考（标注 --json、--headed） |
| `skills/agent-browser/references/advanced-waiting.md` | 创建 | 高级等待机制 |
| `skills/agent-browser/references/network-storage.md` | 创建 | 网络/存储管理 |
| `skills/agent-browser/references/debugging.md` | 创建 | 调试和监控 |
| `skills/agent-browser/references/sessions-profiles.md` | 创建 | 会话和持久化（**Cookie 持久化重点**） |
| `skills/agent-browser/references/visual-mode.md` | 创建 | **可视化操作指南（--headed）** |
| `.claude-plugin/plugin.json` | 更新 | 添加新 skill |
| `commands/find.md` | 创建 | 语义查找命令 |
| `commands/browse.md` | 更新 | 添加语义定位器和可视化示例 |
| `agents/browser-automation.md` | 更新 | 添加最佳实践（可视化、持久化） |
| `CLAUDE.md` | 更新 | 反映新增功能 |

---

## 验证测试方案

### 1. 功能测试

```bash
# 测试基础功能（可视化模式 + profile 隔离）
agent-browser --profile test1 --headed open https://example.com
agent-browser --profile test1 screenshot test.png
agent-browser --profile test1 find text "More information" click

# 测试 Ref 系统和错误恢复
agent-browser --profile test1 snapshot -i
agent-browser --profile test1 click @e1
# 模拟页面变化后重新 snapshot
agent-browser --profile test1 snapshot -i

# 测试 JSON 输出
agent-browser --json get title

# 测试会话隔离
agent-browser --profile test1 open https://site-a.com
agent-browser --profile test2 open https://site-b.com
# 验证两个会话的 cookies 互不影响
```

### 2. 可视化操作测试

```bash
# 测试 --headed 模式（用户应该看到浏览器窗口打开）
agent-browser --profile visual_test --headed open https://example.com
# 验证：浏览器窗口应该可见，用户能实时看到操作

# 测试无头模式（后台运行）
agent-browser --profile headless_test open https://example.com
# 验证：没有浏览器窗口，后台运行
```

### 3. Cookie 持久化测试

```bash
# 测试 Cookie 保存
echo "=== 首次登录 ==="
agent-browser --profile cookie_test --headed open https://github.com
# 手动登录...

# 保存状态后退出
echo "=== 退出 ==="
# 关闭浏览器

# 验证 Cookie 持久化
echo "=== 重新打开，验证是否保持登录状态 ==="
agent-browser --profile cookie_test --headed open https://github.com
# 验证：应该自动登录，无需重新输入凭证

# 验证 Profile 数据
ls -la ~/.agent-browser/profiles/cookie_test/
# 应该看到 cookies、localStorage 等持久化数据
```

### 4. 多网站验证

| 网站类型 | 测试 URL | 测试内容 | 验证点 |
|----------|----------|----------|--------|
| 静态页面 | example.com | 基础导航和截图 | --headed 可视化 |
| SPA 应用 | react.dev | 等待和动态内容 | --wait --text |
| 表单填写 | github.com/search | 表单填写 | find label |
| **登录持久化** | github.com/login | **认证流程 + Cookie 保存** | **下次无需登录** |
| 多任务隔离 | site-a.com + site-b.com | 不同 profile | cookies 互不影响 |

### 5. Token 消耗验证

```bash
# 验证 progressive disclosure
wc -l skills/agent-browser/SKILL.md  # 应该 < 500 行

# 验证 JSON 输出节省
agent-browser get title | wc -c      # 文本输出
agent-browser --json get title | wc -c  # JSON 输出（应更小）
```

### 6. 完整用户流程测试

```bash
# 模拟真实用户场景：登录 GitHub 并创建仓库

# 1. 首次登录（可视化）
SESSION="github_demo"
agent-browser --profile $SESSION --headed open https://github.com/login
agent-browser --profile $SESSION find label "Username" fill "test_user"
agent-browser --profile $SESSION find label "Password" fill "password"
agent-browser --profile $SESSION find role button click --name "Sign in"
agent-browser --profile $SESSION wait --url "**/session"
# Cookies 自动保存

# 2. 关闭浏览器，模拟用户离开
# (用户关闭浏览器或一段时间后)

# 3. 重新打开，验证登录状态
agent-browser --profile $SESSION --headed open https://github.com/new
# 验证：自动登录，直接到创建仓库页面！

# 4. 清理测试 profile
rm -rf ~/.agent-browser/profiles/$SESSION
```

---

## 安装使用流程

### 用户安装

```bash
# 1. 安装插件
/plugin marketplace add ZenoWangzy/agent-browser-claude-plugin
/plugin install agent-browser@agent-browser-claude-plugin

# 2. 安装 agent-browser CLI
npm install -g agent-browser
agent-browser install
```

### 使用示例

#### 示例 1：可视化截图（用户可见浏览器）

```bash
# 用户: 截取 https://example.com 的屏幕截图，我想要看到浏览器操作
# Claude 自动执行（可视化模式 + profile 隔离）:
SESSION_ID="screenshot_$(date +%s)"
agent-browser --profile $SESSION_ID --headed goto https://example.com
agent-browser --profile $SESSION_ID waitFor body
agent-browser --profile $SESSION_ID screenshot screenshot.png
```

#### 示例 2：登录并保存状态（Cookie 持久化）

```bash
# 用户: 帮我登录 GitHub，以后就不需要再登录了
# Claude 自动执行:
SESSION_ID="github_persistent"

# 首次登录 - 浏览器可见
agent-browser --profile $SESSION_ID --headed open https://github.com/login
agent-browser --profile $SESSION_ID find label "Username" fill "your_username"
agent-browser --profile $SESSION_ID find label "Password" fill "your_password"
agent-browser --profile $SESSION_ID find role button click --name "Sign in"
agent-browser --profile $SESSION_ID wait --url "**/session"

# 登录成功！Cookies 自动保存到 github_persistent profile
# 下次直接使用，无需重新登录：
agent-browser --profile $SESSION_ID --headed open https://github.com/dashboard
# 自动恢复登录状态！
```

#### 示例 3：电商购物流程（多步骤 + Cookie 保存）

```bash
# 用户: 帮我在 Amazon 上搜索商品并加入购物车
# Claude 自动执行:
SESSION_ID="shopping_$(date +%s)"

# 打开 Amazon（可见浏览器）
agent-browser --profile $SESSION_ID --headed open https://amazon.com

# 搜索商品
agent-browser --profile $SESSION_ID find textbox search fill "wireless headphones"
agent-browser --profile $SESSION_ID press Enter
agent-browser --profile $SESSION_ID wait --text "Results"

# 点击第一个商品
agent-browser --profile $SESSION_ID snapshot -i  # 获取元素引用
agent-browser --profile $SESSION_ID click @e3  # 第一个商品

# 加入购物车
agent-browser --profile $SESSION_ID find role button click --name "Add to Cart"

# Cookies 和购物车状态自动保存到 profile
# 用户可以稍后继续：agent-browser --profile $SESSION_ID --headed open https://amazon.com/gp/cart
```

#### 示例 4：多任务并行（不同 Profile 隔离）

```bash
# 用户: 同时在 GitHub 和 Google 上搜索
# Claude 执行两个独立任务:

# 任务 1: GitHub 搜索（有自己的 cookies）
agent-browser --profile github_task --headed open https://github.com/search
agent-browser --profile github_task find label "Search" fill "agent-browser"

# 任务 2: Google 搜索（完全隔离的 cookies）
agent-browser --profile google_task --headed open https://google.com
agent-browser --profile google_task find name "q" fill "agent-browser CLI"

# 两个任务的 cookies 和状态完全隔离！
```

---

## 总结

这个实施计划将创建一个完整的 agent-browser skill，覆盖：

1. **核心 AI 友好功能**：语义定位器 + Ref 系统
2. **可视化操作**：`--headed` 模式让用户实时看到浏览器操作
3. **Cookie 持久化**：`--profile` 自动保存登录状态和 cookies
4. **高级等待机制**：wait --text/--url/--fn
5. **网络和存储管理**：拦截、mock、cookies
6. **调试工具**：console、errors、trace
7. **多会话支持**：强制 `--profile` 隔离
8. **Token 优化**：`--json` 输出
9. **错误恢复**：Ref ID 失效后的重新 snapshot 策略
10. **自动安装检测**：检测 agent-browser 是否安装，未安装时引导安装
11. **安全考量**：file:/// 访问警告

### 用户体验核心特性

| 特性 | 实现方式 | 用户收益 |
|------|----------|----------|
| **可视化操作** | `--headed` 模式 | 实时看到 AI 操作浏览器，像人一样 |
| **Cookie 持久化** | `--profile` 自动保存 | 登录一次，永久有效 |
| **多任务隔离** | 不同 `--profile` | 多个任务互不干扰 |
| **免重复登录** | Profile 持久化 | 下次直接使用，无需重新登录 |

通过 progressive disclosure 设计，保持 SKILL.md 精简（~300 行），详细内容按需加载到 references/，实现最佳的 token 效率。
