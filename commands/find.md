---
description: Find and interact with page elements using semantic locators
---

# /find - Semantic Element Finder

Find elements using AI-friendly semantic locators instead of CSS selectors.

## Usage

```
/find <locator-type> <value> [action] [options]
```

## Locator Types

| Type | Example | Description |
|------|---------|-------------|
| `role` | `/find role button click` | ARIA role |
| `text` | `/find text "Submit" click` | Visible text |
| `label` | `/find label "Email" fill "a@b.com"` | Form label |
| `placeholder` | `/find placeholder "Search" fill "query"` | Input placeholder |
| `testid` | `/find testid "submit-btn" click` | data-testid attr |

## Examples

### Click by Role

```bash
/find role button click
/find role button click --name "Submit"
/find role link click --name "Learn more"
```

### Fill by Label

```bash
/find label "Email" fill "user@example.com"
/find label "Password" fill "secret123"
```

### Click by Text

```bash
/find text "More information" click
/find text "Add to cart" click
```

### With Modifiers

```bash
/find role button first click
/find role button last click
/find role button nth 2 click
```

## Actions

| Action | Description |
|--------|-------------|
| `click` | Click element |
| `fill` | Fill input with text |
| `hover` | Hover over element |
| `check` | Check checkbox |
| `uncheck` | Uncheck checkbox |
| `press` | Press keyboard key |

## Options

| Option | Description |
|--------|-------------|
| `--name` | Filter by accessible name |
| `--exact` | Require exact text match |
| `--visible` | Only visible elements |

## Notes

- Semantic locators are more stable than CSS selectors
- Use `--name` with roles for precision
- Prefer labels for form inputs
- See `/skills/agent-browser/references/semantic-locators.md` for complete guide
