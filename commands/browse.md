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

## Semantic Locators (Recommended)

Use semantic locators instead of CSS selectors for better stability:

```bash
# By role
/browse https://example.com find role button click --name "Submit"

# By label
/browse https://example.com find label "Email" fill "user@test.com"

# By text
/browse https://example.com find text "More info" click
```

## Best Practices

```bash
# Use --profile for session isolation and cookie persistence
agent-browser --profile myproject --headed open https://example.com

# Use --headed to see browser actions (recommended)
agent-browser --profile myproject --headed click .button
```

See `/skills/agent-browser/SKILL.md` for complete guide.
