# Debugging

Tools for troubleshooting browser automation issues.

## Console Logs

### Get Console Output

```bash
# Get all console messages
agent-browser --profile $S console

# Get only errors
agent-browser --profile $S console --level error

# Get warnings and errors
agent-browser --profile $S console --level warning

# JSON output
agent-browser --profile $S --json console
```

### Console Levels

| Level | Description |
|-------|-------------|
| `log` | console.log messages |
| `info` | console.info messages |
| `warning` | console.warn messages |
| `error` | console.error messages |
| `debug` | console.debug messages |

## JavaScript Errors

### Get Page Errors

```bash
# Get uncaught JavaScript errors
agent-browser --profile $S errors

# JSON output
agent-browser --profile $S --json errors
```

### Common Error Types

- `TypeError` - Null reference, undefined property
- `SyntaxError` - JavaScript parsing error
- `ReferenceError` - Undefined variable
- `NetworkError` - Failed fetch/XHR requests

## Tracing

### Enable Tracing

```bash
# Start trace
agent-browser --profile $S trace start

# Perform actions
agent-browser --profile $S open https://example.com
agent-browser --profile $S click @e5
agent-browser --profile $S fill @e6 "text"

# Stop and save trace
agent-browser --profile $S trace stop trace.zip
```

### View Trace

Open `trace.zip` in Playwright Trace Viewer:
```bash
npx playwright show-trace trace.zip
```

## Screenshots for Debugging

### Capture Current State

```bash
# Full page screenshot
agent-browser --profile $S screenshot debug.png

# Full page (including below fold)
agent-browser --profile $S screenshot --fullpage debug_full.png

# Specific element
agent-browser --profile $S screenshot --selector ".error-message" error.png
```

### Screenshot on Error

```bash
# Capture when error occurs
if ! agent-browser --profile $S click @e5 2>/dev/null; then
    agent-browser --profile $S screenshot error_state.png
    agent-browser --profile $S console --level error > console_errors.txt
fi
```

## Page Evaluation

### Debug with JavaScript

```bash
# Get page state
agent-browser --profile $S eval "document.readyState"

# Check element existence
agent-browser --profile $S eval "!!document.querySelector('.target')"

# Get element count
agent-browser --profile $S eval "document.querySelectorAll('.item').length"

# Debug CSS
agent-browser --profile $S eval "getComputedStyle(document.querySelector('.btn')).display"
```

## Network Debugging

### Log Network Requests

```bash
# Start network logging
agent-browser --profile $S network log start

# Perform actions
agent-browser --profile $S click @e5

# View requests
agent-browser --profile $S --json network log get

# Stop logging
agent-browser --profile $S network log stop
```

### Check for Failed Requests

```bash
# Filter failed requests
agent-browser --profile $S --json network log get | jq '.[] | select(.status >= 400)'
```

## Common Issues & Solutions

### Element Not Found

```bash
# Problem: click @e5 fails with "Node not found"

# Solution 1: Re-snapshot
agent-browser --profile $S snapshot -i

# Solution 2: Use semantic locator instead
agent-browser --profile $S find role button click --name "Submit"

# Solution 3: Wait for element
agent-browser --profile $S wait --selector ".button"
agent-browser --profile $S snapshot -i
```

### Page Not Loaded

```bash
# Problem: Elements missing or page incomplete

# Solution: Add proper waits
agent-browser --profile $S wait --network idle
agent-browser --profile $S wait --text "Welcome"
agent-browser --profile $S snapshot -i
```

### Login State Lost

```bash
# Problem: Need to login every time

# Solution: Use profile for persistence
agent-browser --profile persistent_session --headed open https://app.com
# Cookies and login state saved automatically
```

### Timeout Errors

```bash
# Problem: "Timeout exceeded"

# Solution 1: Increase timeout
agent-browser --profile $S wait --text "Loading" --timeout 60000

# Solution 2: Check if page actually loads
agent-browser --profile $S screenshot debug.png
agent-browser --profile $S get text "body"
```

### Click Not Working

```bash
# Problem: Click doesn't trigger action

# Solution 1: Wait for element to be clickable
agent-browser --profile $S wait --selector ".button" --visible

# Solution 2: Scroll into view first
agent-browser --profile $S scroll @e5
agent-browser --profile $S click @e5

# Solution 3: Use JavaScript click
agent-browser --profile $S eval "document.querySelector('.button').click()"
```

## Debugging Workflow

```bash
SESSION="debug_$(date +%s)"

# 1. Enable visible mode for observation
agent-browser --profile $SESSION --headed open https://example.com

# 2. Add slow motion to see actions
agent-browser --profile $SESSION --headed --slow-mo 500 click @e5

# 3. Capture state at each step
agent-browser --profile $SESSION screenshot step1.png
agent-browser --profile $SESSION console --level error

# 4. Check for JavaScript errors
agent-browser --profile $SESSION errors

# 5. Enable tracing for detailed analysis
agent-browser --profile $SESSION trace start
# ... actions ...
agent-browser --profile $SESSION trace stop debug_trace.zip
```

## Best Practices

1. **Always use --headed** when debugging
2. **Add --slow-mo** to observe actions
3. **Take screenshots** at each step
4. **Check console errors** after failures
5. **Use tracing** for complex issues
6. **Verify waits** are working correctly
