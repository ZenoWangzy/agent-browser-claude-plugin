# Example User-Level Claude Config

Example `~/.claude/CLAUDE.md` configuration with browser automation preferences.

```markdown
# My Preferences

## Browser Automation

I frequently use browser automation for:
- Web scraping for data collection
- Form testing
- Screenshot capture for documentation

When I mention browsing or screenshots:
1. Use agent-browser commands
2. Wait for elements before interaction
3. Take screenshots for debugging
4. Handle errors gracefully

## Preferred Selectors

I prefer using:
- `data-testid` attributes for testing
- Semantic classes (`.product-card`, not `.css-123`)
- IDs when available

## Viewport Defaults

For screenshots, use:
- Desktop: 1920x1080
- Tablet: 768x1024
- Mobile: 375x667
```
