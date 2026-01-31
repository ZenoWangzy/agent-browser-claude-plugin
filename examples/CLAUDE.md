# Example Project-Level Claude Config

Example `.claude/CLAUDE.md` configuration for browser automation projects.

```markdown
# Browser Automation Project

This project uses agent-browser for web automation tasks.

## Browser Commands

When I ask you to interact with websites, use agent-browser:

```bash
agent-browser goto <url>
agent-browser screenshot
agent-browser scrape <selector>
```

## Context

- We're testing https://example.com
- Focus on checkout flow testing
- Use data-testid selectors where possible

## Testing Priorities

1. Test happy path for checkout
2. Test form validation
3. Test error handling
4. Take screenshots for debugging
```
