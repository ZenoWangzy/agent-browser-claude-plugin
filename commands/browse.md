---
description: Navigate and interact with web pages using agent-browser
---

# /browse - Browser Navigation Command

Navigate to and interact with web pages using agent-browser.

## Usage

```
/browse <url> [actions...]
```

## Examples

```bash
# Simple navigation
/browse https://example.com

# Navigate and click
/browse https://example.com click .button

# Navigate with multiple actions
/browse https://github.com/search?q=test waitFor .repo-list click .repo-item:first-child
```

## Actions

| Action | Description | Example |
|--------|-------------|---------|
| `goto <url>` | Navigate to URL | `goto https://example.com` |
| `click <selector>` | Click element | `click .button` |
| `fill <selector> <value>` | Fill input | `fill #email user@example.com` |
| `select <selector> <value>` | Select dropdown | `select #country US` |
| `waitFor <selector>` | Wait for element | `waitFor .content` |
| `wait <ms>` | Wait milliseconds | `wait 1000` |
| `screenshot` | Take screenshot | `screenshot` |
| `back` | Go back | `back` |
| `forward` | Go forward | `forward` |

## Notes

- Browser session persists between commands
- Use `waitFor` before interacting with dynamic content
- Screenshots are saved to current directory
- Use CSS selectors for element targeting
