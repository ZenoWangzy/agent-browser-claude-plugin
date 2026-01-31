# agent-browser Claude Code 插件

[![Stars](https://img.shields.io/github/stars/ZenoWangzy/agent-browser-claude-plugin?style=flat)](https://github.com/ZenoWangzy/agent-browser-claude-plugin/stargazers)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Rust](https://img.shields.io/badge/-Rust-000000?logo=rust&logoColor=white)
![Node.js](https://img.shields.io/badge/-Node.js-339933?logo=node.js&logoColor=white)
![Playwright](https://img.shields.io/badge/-Playwright-2EAD33?logo=playwright&logoColor=white)

<p align="left">
  <a href="README.md">English</a> |
  <span>简体中文</span>
</p>

> Claude Code 的快速无头浏览器自动化插件 - 基于 Rust CLI 和 Node.js 守护进程构建的 Playwright 解决方案

一个全面的浏览器自动化插件，将 [agent-browser](https://github.com/ZenoWangzy/agent-browser) 的强大功能带给 Claude Code。使用简单的命令即可完成网页爬取、表单填写、截图、E2E 测试等操作。

---

## 特性

- 🚀 **快速** - Rust CLI 处理命令解析
- 🎭 **Playwright** - 行业标准的浏览器自动化
- 📸 **截图** - 全页或元素捕获
- 🕷️ **网页爬取** - 从任何网站提取结构化数据
- 📝 **表单自动化** - 自动填写和提交表单
- 🧪 **E2E 测试** - 测试你的 Web 应用
- 📑 **多标签页** - 管理多个浏览器标签
- 🔧 **100+ 命令** - 完整的浏览器控制 API

---

## 快速开始

### 方式 1: 作为插件安装（推荐）

```bash
# 添加为市场源
/plugin marketplace add ZenoWangzy/agent-browser-claude-plugin

# 安装插件
/plugin install agent-browser@agent-browser-claude-plugin
```

### 方式 2: 手动安装

```bash
# 克隆仓库
git clone https://github.com/ZenoWangzy/agent-browser-claude-plugin.git

# 复制组件到 Claude 配置
cp -r agent-browser-claude-plugin/agents/* ~/.claude/agents/
cp -r agent-browser-claude-plugin/skills/* ~/.claude/skills/
cp -r agent-browser-claude-plugin/commands/* ~/.claude/commands/
cp -r agent-browser-claude-plugin/rules/* ~/.claude/rules/
cp -r agent-browser-claude-plugin/hooks/* ~/.claude/hooks/
```

---

## 包含内容

```
agent-browser-claude-plugin/
├── .claude-plugin/          # 插件清单
│   ├── plugin.json          # 插件元数据
│   └── marketplace.json     # 市场目录
├── agents/                  # 专业化子代理
│   └── browser-automation.md
├── skills/                  # 工作流定义
│   ├── web-scraping/       # 网页爬取
│   ├── form-automation/    # 表单自动化
│   ├── screenshot-capture/ # 截图捕获
│   └── e2e-testing/       # E2E 测试
├── commands/               # 斜杠命令
│   ├── browse.md          # 浏览命令
│   ├── screenshot.md      # 截图命令
│   ├── scrape.md          # 爬取命令
│   └── fill-form.md       # 填表命令
├── rules/                  # 指南规则
│   └── browser-automation.md
├── hooks/                  # 自动化钩子
│   └── hooks.json
├── contexts/              # 系统提示
│   └── browser-mode.md
├── examples/              # 示例
└── docs/                  # 文档
```

---

## 使用示例

### 网站截图

```
你: 给 https://example.com 截个图

Claude: 我会导航到页面并截图。
[使用 /browse 导航]
[使用 /screenshot 截图]
完成！已保存到 screenshot.png
```

### 爬取数据

```
你: 从 https://shop.com 爬取所有产品价格

Claude: 我会提取价格信息。
[使用 /scrape 和 .price 选择器]
找到 25 个产品：
- Widget A: $29.99
- Widget B: $49.99
...
```

### 填写表单

```
你: 填写 https://example.com/contact 的联系表单

Claude: 我会填写并提交表单。
[使用 /fill-form 和字段数据]
表单提交成功！
```

---

## 命令参考

| 命令 | 描述 | 示例 |
|------|------|------|
| `/browse` | 导航和交互页面 | `/browse https://example.com click .button` |
| `/screenshot` | 捕获截图 | `/screenshot https://example.com --full` |
| `/scrape` | 提取结构化数据 | `/scrape https://blog.com h2 text` |
| `/fill-form` | 自动填写表单 | `/fill-form https://form.com '{"name":"张三"}'` |

---

## 代理能力

`browser-automation` 代理可以处理：

- **导航**: 前进、后退、刷新
- **交互**: 点击、填充、选择、悬停、拖拽
- **提取**: 文本、HTML、属性、JSON
- **视觉**: 截图、PDF、快照
- **测试**: 等待、断言、控制台检查
- **多标签**: 创建、切换、关闭标签页

---

## 包含的技能

### 网页爬取
从网站提取数据的完整工作流，处理分页、动态内容和反爬措施。

### 表单自动化
智能表单填写，支持字段检测、多种输入类型和提交处理。

### 截图捕获
全页和元素截图，支持视口控制、等待策略和输出选项。

### E2E 测试
基于浏览器的测试，包含断言、等待、控制台监控和错误处理。

---

## 系统要求

- **Claude Code CLI**: v2.1.0 或更高版本
- **agent-browser**: 从 https://github.com/ZenoWangzy/agent-browser 安装
- **Node.js**: v18 或更高版本（用于守护进程）
- **Rust**: （可选，用于 CLI 编译）

---

## 安装 agent-browser

此插件需要安装 agent-browser：

```bash
# 通过 cargo
cargo install agent-browser

# 或下载二进制文件
# 访问 https://github.com/ZenoWangzy/agent-browser/releases
```

启动守护进程：
```bash
agent-browser daemon
```

---

## 规则与最佳实践

包含的规则强制执行：

1. **选择器稳定性** - 使用语义化选择器
2. **等待策略** - 等待元素，不要猜测
3. **资源清理** - 完成后关闭浏览器
4. **错误处理** - 优雅的失败处理
5. **速率限制** - 尊重的自动化行为
6. **安全性** - 永不记录敏感数据

---

## 钩子自动化

内置钩子提供：

- **预命令检查** - 验证浏览器就绪
- **后命令验证** - 验证操作成功
- **截图提示** - 建议截图用于调试
- **错误恢复** - 自动重试建议

---

## 贡献

欢迎贡献！可以贡献的领域：

- 特定语言的技能
- 框架特定的 E2E 模式
- 更多反检测策略
- 其他命令类型
- 文档改进

查看 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南。

---

## 许可证

MIT 许可证 - 详见 [LICENSE](LICENSE)。

---

## 相关链接

- **agent-browser**: https://github.com/ZenoWangzy/agent-browser
- **Claude Code 文档**: https://code.claude.com
- **Playwright 文档**: https://playwright.dev
- **问题反馈**: https://github.com/ZenoWangzy/agent-browser-claude-plugin/issues

---

## Star 历史

[![Star History Chart](https://api.star-history.com/svg?repos=ZenoWangzy/agent-browser-claude-plugin&type=Date)](https://star-history.com/#ZenoWangzy/agent-browser-claude-plugin&Date)

---

**为 Claude Code 社区用 ❤️ 制作**
