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
| `alt` | `/find alt "Logo" click` | Image alt text |
| `testid` | `/find testid "submit-btn" click` | data-testid attr |

## Examples

### Click by Role

```bash
/find role button click
/find role button click --name "Submit"
/find role link click --name "Learn more"
/find role checkbox check
/find role combobox click
```

### Fill by Label

```bash
/find label "Email" fill "user@example.com"
/find label "Password" fill "secret123"
/find label "I agree to terms" check
```

### Click by Text

```bash
/find text "More information" click
/find text "Add to cart" click
/find text "Submit" click --exact
```

### By Placeholder

```bash
/find placeholder "Search..." fill "my query"
/find placeholder "Enter your email" fill "a@b.com"
```

### By Alt Text

```bash
/find alt "Company Logo" click
/find alt "Profile Picture"  # Just locate, no action
```

### By Test ID

```bash
/find testid "submit-button" click
/find testid "email-input" fill "test@test.com"
```

### With Modifiers

```bash
# First matching button
/find role button first click

# Last matching button
/find role button last click

# Second matching element (0-indexed)
/find role button nth 1 click

# Third matching element
/find text "Item" nth 2 click
```

## Actions

| Action | Description | Example |
|--------|-------------|---------|
| `click` | Click element | `find role button click` |
| `fill` | Fill input with text | `find label "Email" fill "a@b.com"` |
| `hover` | Hover over element | `find text "Menu" hover` |
| `check` | Check checkbox | `find role checkbox check` |
| `uncheck` | Uncheck checkbox | `find role checkbox uncheck` |
| `press` | Press key | `find role textbox press Enter` |
| `focus` | Focus element | `find testid "input" focus` |
| `clear` | Clear input | `find label "Search" clear` |

## Options

| Option | Description | Example |
|--------|-------------|---------|
| `--name` | Filter by accessible name | `--name "Submit"` |
| `--exact` | Require exact text match | `--exact` |
| `--visible` | Only visible elements | `--visible` |
| `--enabled` | Only enabled elements | `--enabled` |

## Modifiers

| Modifier | Description | Example |
|----------|-------------|---------|
| `first` | First matching element | `find role button first click` |
| `last` | Last matching element | `find role button last click` |
| `nth <n>` | Nth matching element (0-indexed) | `find role button nth 1 click` |

## Notes

- Semantic locators are more stable than CSS selectors
- Use `--name` with roles for precision
- Prefer labels for form inputs
- See `/skills/agent-browser/references/semantic-locators.md` for complete guide
