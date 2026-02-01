# Ref System

The ref system assigns unique IDs (@e1, @e2, @e3...) to page elements after a snapshot, enabling precise element targeting without CSS selectors.

## Core Workflow

```bash
# 1. Take snapshot to generate refs
agent-browser --profile $SESSION snapshot -i

# Output:
# @e1 <html lang="en">
# @e2 <body>
# @e3 <header>
# @e4 <nav>
# @e5 <button>Login</button>
# @e6 <button>Sign Up</button>
# @e7 <main>
# @e8 <h1>Welcome</h1>
# @e9 <input type="email" placeholder="Email">
# @e10 <input type="password" placeholder="Password">
# @e11 <button>Submit</button>

# 2. Use refs for actions
agent-browser --profile $SESSION click @e5      # Click Login
agent-browser --profile $SESSION fill @e9 "a@b.com"  # Fill email
agent-browser --profile $SESSION fill @e10 "secret"  # Fill password
agent-browser --profile $SESSION click @e11     # Click Submit
```

## Snapshot Commands

### Basic Snapshot

```bash
# Interactive mode - shows ref IDs
agent-browser --profile $SESSION snapshot -i

# With JSON output (token efficient)
agent-browser --profile $SESSION --json snapshot -i

# Snapshot specific element and children
agent-browser --profile $SESSION snapshot -i --selector "main"
```

### Snapshot Options

| Option | Description | Example |
|--------|-------------|---------|
| `-i, --interactive` | Show interactive refs | `snapshot -i` |
| `--selector` | Limit to specific element | `snapshot -i --selector "form"` |
| `--depth` | Limit tree depth | `snapshot -i --depth 3` |
| `--visible` | Only visible elements | `snapshot -i --visible` |

## Using Refs

### Click

```bash
agent-browser --profile $SESSION click @e5
```

### Fill

```bash
agent-browser --profile $SESSION fill @e9 "hello world"
agent-browser --profile $SESSION fill @e9 ""  # Clear field
```

### Get Text

```bash
agent-browser --profile $SESSION get text @e8
# Output: Welcome
```

### Get Attribute

```bash
agent-browser --profile $SESSION get attribute @e9 placeholder
# Output: Email
```

### Hover

```bash
agent-browser --profile $SESSION hover @e4  # Hover over nav
```

### Check/Uncheck

```bash
agent-browser --profile $SESSION check @e12    # Check checkbox
agent-browser --profile $SESSION uncheck @e12  # Uncheck checkbox
```

## Ref Persistence

> [!IMPORTANT]
> **Refs are invalidated when the page changes!**
> 
> After any of these actions, you MUST re-snapshot:
> - Navigation to a new URL
> - Page refresh
> - DOM mutations (new elements added/removed)
> - AJAX content loading
> - Single Page App route changes

### Example: Multi-step Flow

```bash
SESSION="checkout_flow"

# Step 1: Product page
agent-browser --profile $SESSION --headed open https://store.com/product
agent-browser --profile $SESSION snapshot -i
agent-browser --profile $SESSION click @e5  # Add to Cart

# Step 2: Cart page (refs invalidated after navigation!)
agent-browser --profile $SESSION wait --url "**/cart"
agent-browser --profile $SESSION snapshot -i  # MUST re-snapshot!
agent-browser --profile $SESSION click @e3  # Checkout button

# Step 3: Checkout page
agent-browser --profile $SESSION wait --url "**/checkout"
agent-browser --profile $SESSION snapshot -i  # Re-snapshot again
agent-browser --profile $SESSION fill @e4 "John Doe"
agent-browser --profile $SESSION fill @e5 "123 Main St"
```

## Error Recovery

### "Node not found" Error

```bash
# Ref @e5 was invalidated
agent-browser --profile $SESSION click @e5
# Error: Node not found

# Solution: Re-snapshot and find new ref
agent-browser --profile $SESSION snapshot -i
# Now button has ref @e8
agent-browser --profile $SESSION click @e8
```

### "Detached from DOM" Error

Same solution - page content changed, refs are stale:

```bash
agent-browser --profile $SESSION snapshot -i
# Find new ref ID for target element
agent-browser --profile $SESSION click @eN  # Use new ref
```

### Recovery Pattern

```bash
# Robust pattern for clicking elements that may change
attempt_click() {
    if ! agent-browser --profile $SESSION click @e5 2>/dev/null; then
        agent-browser --profile $SESSION snapshot -i
        # Find element again (parse output or use semantic locator fallback)
        agent-browser --profile $SESSION find text "Submit" click
    fi
}
```

## Best Practices

### 1. Always Snapshot After Page Changes

```bash
# After navigation
agent-browser --profile $SESSION open https://example.com
agent-browser --profile $SESSION snapshot -i  # Always!

# After clicking that causes navigation
agent-browser --profile $SESSION click @e5
agent-browser --profile $SESSION wait --url "**/newpage"
agent-browser --profile $SESSION snapshot -i  # Always!

# After AJAX loads content
agent-browser --profile $SESSION click @e3  # Opens modal
agent-browser --profile $SESSION wait --text "Modal Title"
agent-browser --profile $SESSION snapshot -i  # Always!
```

### 2. Use --json for Token Efficiency

```bash
# Verbose output (many tokens)
agent-browser --profile $SESSION snapshot -i

# Token-efficient output
agent-browser --profile $SESSION --json snapshot -i
```

### 3. Limit Snapshot Scope

For complex pages, limit snapshot to relevant area:

```bash
# Full page (slow, many refs)
agent-browser --profile $SESSION snapshot -i

# Just the form (faster, fewer refs)
agent-browser --profile $SESSION snapshot -i --selector "form#login"

# Limited depth
agent-browser --profile $SESSION snapshot -i --depth 5
```

### 4. Combine with Semantic Locators

Use refs when you have them, fall back to semantic locators:

```bash
# Primary: Use ref if available
agent-browser --profile $SESSION click @e5

# Fallback: Use semantic locator if ref fails
agent-browser --profile $SESSION find role button click --name "Submit"
```

## Comparison: Refs vs Semantic Locators

| Aspect | Refs (@e1) | Semantic Locators |
|--------|-----------|-------------------|
| Precision | Exact element | May match multiple |
| Speed | Faster (direct ID) | Slower (search) |
| Stability | Fragile (invalidates) | More stable |
| Setup | Requires snapshot | No setup |
| Use case | Sequential actions | One-off actions |

**Recommendation:** Use refs for multi-step workflows on the same page. Use semantic locators for simple, one-off actions or as fallback.
