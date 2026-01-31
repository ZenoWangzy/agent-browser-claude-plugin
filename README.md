# agent-browser Claude Code Plugin

[![Stars](https://img.shields.io/github/stars/ZenoWangzy/agent-browser-claude-plugin?style=flat)](https://github.com/ZenoWangzy/agent-browser-claude-plugin/stargazers)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
![Rust](https://img.shields.io/badge/-Rust-000000?logo=rust&logoColor=white)
![Node.js](https://img.shields.io/badge/-Node.js-339933?logo=node.js&logoColor=white)
![Playwright](https://img.shields.io/badge/-Playwright-2EAD33?logo=playwright&logoColor=white)

<p align="left">
  <span>English</span> |
  <a href="README.zh-CN.md">简体中文</a>
</p>

> Fast headless browser automation plugin for Claude Code - Built with Rust CLI and Node.js daemon on Playwright

A comprehensive browser automation plugin that brings the power of [agent-browser](https://github.com/ZenoWangzy/agent-browser) to Claude Code. Perform web scraping, form filling, screenshots, E2E testing, and more with simple commands.

---

## Features

- 🚀 **Fast** - Rust CLI for quick command parsing
- 🎭 **Playwright** - Industry-standard browser automation
- 📸 **Screenshots** - Full page or element captures
- 🕷️ **Web Scraping** - Extract structured data from any website
- 📝 **Form Automation** - Fill and submit forms automatically
- 🧪 **E2E Testing** - Test your web applications
- 📑 **Multi-tab** - Manage multiple browser tabs
- 🔧 **100+ Commands** - Complete browser control API

---

## Quick Start

### Option 1: Install as Plugin (Recommended)

```bash
# Add as marketplace
/plugin marketplace add ZenoWangzy/agent-browser-claude-plugin

# Install the plugin
/plugin install agent-browser@agent-browser-claude-plugin
```

### Option 2: Manual Installation

```bash
# Clone the repo
git clone https://github.com/ZenoWangzy/agent-browser-claude-plugin.git

# Copy components to your Claude config
cp -r agent-browser-claude-plugin/agents/* ~/.claude/agents/
cp -r agent-browser-claude-plugin/skills/* ~/.claude/skills/
cp -r agent-browser-claude-plugin/commands/* ~/.claude/commands/
cp -r agent-browser-claude-plugin/rules/* ~/.claude/rules/
cp -r agent-browser-claude-plugin/hooks/* ~/.claude/hooks/
```

---

## What's Inside

```
agent-browser-claude-plugin/
├── .claude-plugin/          # Plugin manifests
│   ├── plugin.json          # Plugin metadata
│   └── marketplace.json     # Marketplace catalog
├── agents/                  # Specialized subagents
│   └── browser-automation.md
├── skills/                  # Workflow definitions
│   ├── web-scraping/
│   ├── form-automation/
│   ├── screenshot-capture/
│   └── e2e-testing/
├── commands/                # Slash commands
│   ├── browse.md
│   ├── screenshot.md
│   ├── scrape.md
│   └── fill-form.md
├── rules/                   # Guidelines
│   └── browser-automation.md
├── hooks/                   # Automations
│   └── hooks.json
├── contexts/                # System prompts
│   └── browser-mode.md
├── examples/                # Examples
└── docs/                    # Documentation
```

---

## Usage Examples

### Screenshot a Website

```
You: Take a screenshot of https://example.com

Claude: I'll navigate to the page and capture a screenshot.
[Uses /browse to navigate]
[Uses /screenshot to capture]
Done! Saved to screenshot.png
```

### Scrape Data

```
You: Scrape all product prices from https://shop.com

Claude: I'll extract the pricing information.
[Uses /scrape with .price selector]
Found 25 products:
- Widget A: $29.99
- Widget B: $49.99
...
```

### Fill a Form

```
You: Fill out the contact form at https://example.com/contact

Claude: I'll fill and submit the form.
[Uses /fill-form with field data]
Form submitted successfully!
```

---

## Commands Reference

| Command | Description | Example |
|---------|-------------|---------|
| `/browse` | Navigate and interact with pages | `/browse https://example.com click .button` |
| `/screenshot` | Capture screenshots | `/screenshot https://example.com --full` |
| `/scrape` | Extract structured data | `/scrape https://blog.com h2 text` |
| `/fill-form` | Auto-fill forms | `/fill-form https://form.com '{"name":"John"}'` |

---

## Agent Capabilities

The `browser-automation` agent handles:

- **Navigation**: goto, back, forward, refresh
- **Interaction**: click, fill, select, hover, drag
- **Extraction**: text, HTML, attributes, JSON
- **Visual**: screenshots, PDFs, snapshots
- **Testing**: waits, assertions, console checks
- **Multi-tab**: create, switch, close tabs

---

## Skills Included

### Web Scraping
Complete workflows for extracting data from websites, handling pagination, dynamic content, and anti-scraping measures.

### Form Automation
Intelligent form filling with field detection, multiple input types, and submission handling.

### Screenshot Capture
Full page and element screenshots with viewport control, wait strategies, and output options.

### E2E Testing
Browser-based testing with assertions, waits, console monitoring, and error handling.

---

## Requirements

- **Claude Code CLI**: v2.1.0 or later
- **agent-browser**: Install from https://github.com/ZenoWangzy/agent-browser
- **Node.js**: v18 or later (for daemon)
- **Rust**: (optional, for CLI compilation)

---

## Installation: agent-browser

This plugin requires agent-browser to be installed:

```bash
# Via cargo
cargo install agent-browser

# Or download binary
# Visit https://github.com/ZenoWangzy/agent-browser/releases
```

Start the daemon:
```bash
agent-browser daemon
```

---

## Rules & Best Practices

The included rules enforce:

1. **Selector Stability** - Use semantic selectors
2. **Wait Strategies** - Wait for elements, don't guess
3. **Resource Cleanup** - Close browsers when done
4. **Error Handling** - Graceful failure handling
5. **Rate Limiting** - Respectful automation
6. **Security** - Never log sensitive data

---

## Hooks Automation

Built-in hooks provide:

- **Pre-command checks** - Verify browser is ready
- **Post-command validation** - Verify actions succeeded
- **Screenshot prompts** - Suggest screenshots for debugging
- **Error recovery** - Automatic retry suggestions

---

## Contributing

Contributions are welcome! Areas for contribution:

- Additional language-specific skills
- Framework-specific E2E patterns
- More anti-detection strategies
- Additional command types
- Documentation improvements

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## License

MIT License - See [LICENSE](LICENSE) for details.

---

## Links

- **agent-browser**: https://github.com/ZenoWangzy/agent-browser
- **Claude Code Docs**: https://code.claude.com
- **Playwright Docs**: https://playwright.dev
- **Issues**: https://github.com/ZenoWangzy/agent-browser-claude-plugin/issues

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=ZenoWangzy/agent-browser-claude-plugin&type=Date)](https://star-history.com/#ZenoWangzy/agent-browser-claude-plugin&Date)

---

**Built with ❤️ for the Claude Code community**
