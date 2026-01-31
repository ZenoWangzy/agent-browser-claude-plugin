---
description: Capture screenshots of web pages
---

# /screenshot - Screenshot Command

Take screenshots of web pages using agent-browser.

## Usage

```
/screenshot <url> [options]
```

## Options

| Option | Description | Default |
|--------|-------------|---------|
| `--full` | Capture full page | false |
| `--selector <css>` | Capture specific element | - |
| `--output <path>` | Output file path | screenshot.png |
| `--wait <selector>` | Wait for element before capture | - |

## Examples

```bash
# Basic screenshot
/screenshot https://example.com

# Full page screenshot
/screenshot https://example.com --full

# Screenshot specific element
/screenshot https://example.com --selector .hero --output hero.png

# Wait for content then capture
/screenshot https://example.com --wait .content --output page.png
```

## Notes

- Supports PNG format
- Output path is relative to current directory
- Browser viewport defaults to 1280x720
- Use --full for scrollable pages
