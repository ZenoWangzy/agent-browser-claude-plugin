# Semantic Locators

AI-friendly element targeting without CSS selectors. Use semantic locators when you need to interact with elements by their role, text content, or accessibility attributes.

## Locator Types

### find role

Target elements by ARIA role:

```bash
# Buttons
agent-browser find role button click
agent-browser find role button click --name "Submit"
agent-browser find role button click --name "Login" --exact

# Links
agent-browser find role link click --name "Learn more"

# Inputs
agent-browser find role textbox fill "hello"
agent-browser find role combobox click

# Other roles
agent-browser find role checkbox check
agent-browser find role radio click --name "Option 1"
agent-browser find role menuitem click --name "Settings"
agent-browser find role tab click --name "Details"
```

**Common Roles:**
| Role | Elements |
|------|----------|
| `button` | `<button>`, `<input type="button">` |
| `link` | `<a href="...">` |
| `textbox` | `<input type="text">`, `<textarea>` |
| `checkbox` | `<input type="checkbox">` |
| `radio` | `<input type="radio">` |
| `combobox` | `<select>` |
| `menuitem` | Menu items |
| `tab` | Tab elements |
| `heading` | `<h1>` - `<h6>` |

### find text

Target elements by visible text content:

```bash
# Exact text match
agent-browser find text "More information" click

# Partial text match (contains)
agent-browser find text "Learn" click

# With exact flag
agent-browser find text "Submit" click --exact
```

### find label

Target form inputs by their associated label:

```bash
# Text input by label
agent-browser find label "Email" fill "user@example.com"

# Password field
agent-browser find label "Password" fill "secret123"

# Checkbox by label
agent-browser find label "I agree to terms" check
```

### find placeholder

Target inputs by placeholder text:

```bash
agent-browser find placeholder "Search..." fill "my query"
agent-browser find placeholder "Enter your email" fill "a@b.com"
```

### find alt

Target images by alt text:

```bash
agent-browser find alt "Company Logo" click
agent-browser find alt "Profile Picture"  # Just locate, no action
```

### find testid

Target elements by `data-testid` attribute:

```bash
agent-browser find testid "submit-button" click
agent-browser find testid "email-input" fill "test@test.com"
agent-browser find testid "user-menu" hover
```

## Actions

All locators support these actions:

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

## Modifiers

Refine locator matches:

### first / last

```bash
# First matching button
agent-browser find role button first click

# Last matching button
agent-browser find role button last click
```

### nth

```bash
# Second matching element (0-indexed)
agent-browser find role button nth 1 click

# Third matching element
agent-browser find text "Item" nth 2 click
```

### Chaining

Combine locators for precision:

```bash
# Find submit button within a specific form
agent-browser find role form --name "Login" >> find role button click --name "Submit"
```

## Flags

| Flag | Description | Example |
|------|-------------|---------|
| `--name` | Filter by accessible name | `--name "Submit"` |
| `--exact` | Require exact text match | `--exact` |
| `--visible` | Only visible elements | `--visible` |
| `--enabled` | Only enabled elements | `--enabled` |

## Best Practices

1. **Prefer semantic locators** over CSS selectors for AI readability
2. **Use `--name` with roles** for precision: `find role button --name "Submit"`
3. **Use labels for forms**: `find label "Email"` instead of `find role textbox`
4. **Add `--exact`** when multiple similar text matches exist
5. **Use testid** for elements without clear semantic meaning

## Examples

### Login Form

```bash
SESSION="login_task"
agent-browser --profile $SESSION --headed open https://example.com/login
agent-browser --profile $SESSION find label "Username" fill "myuser"
agent-browser --profile $SESSION find label "Password" fill "mypass"
agent-browser --profile $SESSION find role button click --name "Sign In"
agent-browser --profile $SESSION wait --url "**/dashboard"
```

### Search and Select

```bash
agent-browser --profile $SESSION find placeholder "Search..." fill "product"
agent-browser --profile $SESSION find role button click --name "Search"
agent-browser --profile $SESSION wait --text "Results"
agent-browser --profile $SESSION find text "Best Product" first click
```

### Form with Multiple Fields

```bash
agent-browser --profile $SESSION find label "First Name" fill "John"
agent-browser --profile $SESSION find label "Last Name" fill "Doe"
agent-browser --profile $SESSION find label "Email" fill "john@example.com"
agent-browser --profile $SESSION find role combobox --name "Country" click
agent-browser --profile $SESSION find role option click --name "United States"
agent-browser --profile $SESSION find label "I accept terms" check
agent-browser --profile $SESSION find role button click --name "Submit"
```
