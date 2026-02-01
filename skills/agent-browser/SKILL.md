---
name: agent-browser
description: Complete browser automation using agent-browser CLI with AI-friendly semantic locators, ref system, and advanced waiting. Use when Claude needs to navigate and interact with web pages, extract structured data, fill forms, take screenshots, run E2E tests, or debug web applications. Covers semantic locators (role/text/label/placeholder), snapshot+ref workflow, session isolation with --profile, visible browser with --headed, JSON output for token efficiency, and error recovery strategies.
---

# agent-browser

AI-first browser automation CLI with semantic locators, ref system, and persistent sessions.

**Key Advantages over Playwright MCP:**
- **93% token savings** (daemon persistence)
- **AI-friendly semantic locators** (find role/text/label)
- **Visible browser mode** (--headed)
- **Cookie persistence** (--profile)

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

```bash
# 1. Generate unique session ID
SESSION="task_$(date +%s)"

# 2. Navigate with visible browser
agent-browser --profile $SESSION --headed open https://example.com

# 3. Snapshot to get element refs
agent-browser --profile $SESSION snapshot -i

# 4. Click using ref ID
agent-browser --profile $SESSION click @e2

# 5. Re-snapshot after page changes
agent-browser --profile $SESSION snapshot -i
```

### Alternative: Semantic Locators (No Snapshot Needed)

```bash
agent-browser --profile $SESSION --headed find role button click --name "Submit"
agent-browser --profile $SESSION --headed find label "Email" fill "test@test.com"
agent-browser --profile $SESSION --headed find text "More information" click
```

## Best Practices

### 1. Visual Mode (--headed) - RECOMMENDED

Show browser window so users can see AI operating in real-time:

```bash
# GOOD - Visible browser (user sees what's happening)
agent-browser --profile mysession --headed open https://example.com

# Acceptable - Headless mode (background, faster but invisible)
agent-browser --profile mysession open https://example.com
```

### 2. Session Isolation (--profile) - REQUIRED

**Always use `--profile <session_id>`** for state isolation and automatic cookie saving:

```bash
# GOOD - Isolated session with automatic cookie persistence
agent-browser --profile github_session --headed open https://github.com/login
# ... login process ...
# Cookies automatically saved to github_session profile

# Next time - no need to login again!
agent-browser --profile github_session --headed open https://github.com/dashboard
# Automatically logged in!

# BAD - Shared state (cookies may mix between tasks)
agent-browser open https://example.com  # Don't do this!
```

**Profile Benefits:**
- Login sessions saved across runs
- Cookies persist automatically
- localStorage/sessionStorage preserved
- Multiple isolated sessions can run simultaneously

### 3. Token Efficiency (--json)

Use `--json` flag to reduce output tokens:

```bash
agent-browser --json get title
agent-browser --json get text .content
agent-browser --json snapshot
```

### 4. Error Recovery

If Ref ID fails ("Node not found" or "Detach"), re-snapshot:

```bash
# Ref fails
agent-browser click @e2  # Error: Node not found

# Recovery: Re-snapshot and get new ref
agent-browser --profile $SESSION snapshot -i  # Now button is @e5
agent-browser --profile $SESSION click @e5
```

## Core Commands

| Command | Description | Example |
|---------|-------------|---------|
| `open <url>` | Navigate to URL | `open https://example.com` |
| `snapshot` | Get page elements with refs | `snapshot -i` (interactive refs) |
| `click` | Click element | `click @e2` or `click ".button"` |
| `fill` | Fill input field | `fill @e3 "text"` or `fill "#email" "a@b.com"` |
| `screenshot` | Capture screenshot | `screenshot output.png` |
| `get title` | Get page title | `--json get title` |
| `get text` | Get element text | `get text ".content"` |
| `wait` | Wait for condition | `wait --text "Loaded"` |
| `find` | Semantic locator | `find role button click` |

## Semantic Locators

AI-friendly element targeting without CSS selectors:

| Locator Type | Example | Use Case |
|--------------|---------|----------|
| `find role` | `find role button click --name "Submit"` | Buttons, links, inputs |
| `find text` | `find text "More info" click` | Any visible text |
| `find label` | `find label "Email" fill "a@b.com"` | Form labels |
| `find placeholder` | `find placeholder "Search..." fill "query"` | Input placeholders |
| `find testid` | `find testid "submit-btn" click` | data-testid attributes |

**Actions:** `click`, `fill`, `hover`, `check`, `uncheck`, `press`

**Modifiers:** `first`, `last`, `nth(2)`

See [references/semantic-locators.md](./references/semantic-locators.md) for complete guide.

## Ref System

The ref system assigns unique IDs (@e1, @e2...) to page elements:

```bash
# 1. Snapshot page
agent-browser --profile $SESSION snapshot -i

# Output:
# @e1 <html>
# @e2 <button>Submit</button>
# @e3 <input type="text">

# 2. Use refs
agent-browser --profile $SESSION click @e2
agent-browser --profile $SESSION fill @e3 "Hello"
```

**Important:** Refs are invalidated when page changes. Always re-snapshot after navigation or DOM updates.

See [references/ref-system.md](./references/ref-system.md) for details.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| "Node not found" | Re-snapshot: `snapshot -i` |
| Element not visible | Use `wait --text "..."` before clicking |
| Login state lost | Use `--profile` to persist cookies |
| Can't see browser | Add `--headed` flag |
| Too much output | Add `--json` flag |

## Security Warning

> [!CAUTION]
> Access to `file:///` local paths requires explicit user authorization.
> Never access local files without user consent.

## Advanced Features

For detailed documentation, see:

- [Semantic Locators](./references/semantic-locators.md) - Complete locator guide
- [Ref System](./references/ref-system.md) - Snapshot + ref workflow
- [Visual Mode](./references/visual-mode.md) - --headed mode guide
- [Sessions & Profiles](./references/sessions-profiles.md) - Cookie persistence
- [Advanced Waiting](./references/advanced-waiting.md) - wait commands
- [Network & Storage](./references/network-storage.md) - Cookies, interception
- [Debugging](./references/debugging.md) - Console, errors, trace
- [Complete API](./references/api-complete.md) - All commands reference
