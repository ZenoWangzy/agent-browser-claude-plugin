# Advanced Waiting

Wait commands ensure elements or conditions are ready before proceeding.

## Wait Types

### wait --text

Wait for specific text to appear on page:

```bash
# Wait for exact text
agent-browser --profile $S wait --text "Loading complete"

# Wait for partial text
agent-browser --profile $S wait --text "Success"

# With timeout (default 30000ms)
agent-browser --profile $S wait --text "Done" --timeout 10000
```

### wait --url

Wait for URL to match pattern:

```bash
# Wait for exact URL
agent-browser --profile $S wait --url "https://example.com/dashboard"

# Wait for URL pattern (glob)
agent-browser --profile $S wait --url "**/dashboard"
agent-browser --profile $S wait --url "https://example.com/**/success"

# Wait for URL containing
agent-browser --profile $S wait --url "*checkout*"
```

### wait --selector

Wait for element to exist:

```bash
# Wait for CSS selector
agent-browser --profile $S wait --selector ".loaded"
agent-browser --profile $S wait --selector "#content"

# Wait for ref
agent-browser --profile $S wait --selector @e5

# Wait for element to be visible
agent-browser --profile $S wait --selector ".modal" --visible

# Wait for element to be hidden
agent-browser --profile $S wait --selector ".spinner" --hidden
```

### wait --network

Wait for network activity:

```bash
# Wait until no network requests for 500ms
agent-browser --profile $S wait --network idle

# Wait for specific request
agent-browser --profile $S wait --network "**/api/data"
```

### wait --fn

Wait for JavaScript condition:

```bash
# Wait for custom condition
agent-browser --profile $S wait --fn "document.readyState === 'complete'"

# Wait for element existence
agent-browser --profile $S wait --fn "document.querySelector('.ready') !== null"

# Wait for variable
agent-browser --profile $S wait --fn "window.appLoaded === true"

# Complex condition
agent-browser --profile $S wait --fn "document.querySelectorAll('.item').length > 5"
```

## Common Patterns

### After Navigation

```bash
agent-browser --profile $S click @e5  # Triggers navigation
agent-browser --profile $S wait --url "**/newpage"
agent-browser --profile $S snapshot -i  # Now safe to snapshot
```

### After Form Submit

```bash
agent-browser --profile $S click @e10  # Submit button
agent-browser --profile $S wait --text "Thank you"  # Success message
```

### After AJAX Load

```bash
agent-browser --profile $S click @e3  # Load more button
agent-browser --profile $S wait --selector ".new-items"
agent-browser --profile $S wait --network idle  # All requests done
```

### Modal/Dialog

```bash
agent-browser --profile $S click @e5  # Open modal
agent-browser --profile $S wait --selector ".modal-content" --visible
agent-browser --profile $S snapshot -i  # Snapshot modal content
```

### SPA Route Change

```bash
agent-browser --profile $S click @e3  # SPA navigation
agent-browser --profile $S wait --url "**/products"
agent-browser --profile $S wait --text "Product List"
agent-browser --profile $S snapshot -i
```

### Loading Spinner

```bash
agent-browser --profile $S click @e5  # Trigger loading
agent-browser --profile $S wait --selector ".spinner" --hidden  # Wait for spinner to disappear
agent-browser --profile $S wait --selector ".results"  # Results loaded
```

## Wait Options

| Option | Description | Default |
|--------|-------------|---------|
| `--timeout <ms>` | Maximum wait time | 30000 |
| `--visible` | Element must be visible | - |
| `--hidden` | Element must be hidden | - |
| `--state <state>` | attached/detached/visible/hidden | attached |

## Error Handling

### Timeout Error

```bash
# If wait times out, command fails
agent-browser --profile $S wait --text "Loading" --timeout 5000
# Error: Timeout 5000ms exceeded waiting for text "Loading"
```

### Retry Pattern

```bash
# Retry with increasing timeout
for timeout in 5000 10000 30000; do
    if agent-browser --profile $S wait --text "Ready" --timeout $timeout 2>/dev/null; then
        break
    fi
done
```

## Best Practices

1. **Always wait after navigation** before snapshotting
2. **Use --network idle** for AJAX-heavy pages
3. **Set appropriate timeouts** based on expected load time
4. **Prefer --text or --selector** over arbitrary delays
5. **Chain waits** for complex loading sequences:

```bash
# Wait for multiple conditions
agent-browser --profile $S wait --network idle
agent-browser --profile $S wait --selector ".content"
agent-browser --profile $S wait --text "Welcome"
```
