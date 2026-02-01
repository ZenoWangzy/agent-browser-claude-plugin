# Visual Mode (--headed)

Visual mode shows the browser window, letting users see AI operations in real-time.

## Why Use Visual Mode?

| Benefit | Description |
|---------|-------------|
| **Transparency** | Users see exactly what AI is doing |
| **Trust** | Builds confidence in automated actions |
| **Debugging** | Immediately spot issues |
| **Demo** | Perfect for presentations |
| **Learning** | Understand page interactions |

## Usage

### Basic Visual Mode

```bash
# Add --headed flag to show browser
agent-browser --profile mysession --headed open https://example.com

# All subsequent commands use the same visible browser
agent-browser --profile mysession click @e5
agent-browser --profile mysession fill @e6 "text"
```

### Comparison

```bash
# VISIBLE - User sees browser window
agent-browser --profile session1 --headed open https://example.com

# INVISIBLE (headless) - Background, faster but hidden
agent-browser --profile session2 open https://example.com
```

## When to Use Visual Mode

### ✅ Recommended Scenarios

1. **User-facing tasks** - When users want to monitor AI actions
2. **Login flows** - User may need to handle 2FA or captchas
3. **Debugging** - Identifying why automation fails
4. **Demos** - Showing automation capabilities
5. **Complex workflows** - Multi-step processes that may need intervention

### ⚡ Headless Mode (No --headed)

1. **Batch processing** - Processing many pages quickly
2. **Background tasks** - Tasks that don't need monitoring
3. **CI/CD pipelines** - Server environments
4. **Performance** - Slightly faster execution

## Slow Motion Mode

Combine `--headed` with `--slow-mo` to slow down actions for visibility:

```bash
# Slow down each action by 100ms
agent-browser --profile mysession --headed --slow-mo 100 open https://example.com
agent-browser --profile mysession click @e5  # Visible click with delay
```

## Window Size

Set browser viewport size:

```bash
# Set viewport size
agent-browser --profile mysession --headed --viewport 1920x1080 open https://example.com

# Mobile viewport
agent-browser --profile mysession --headed --viewport 375x812 open https://example.com
```

## Typical Workflow

```bash
SESSION="visual_demo_$(date +%s)"

# 1. Open with visible browser
agent-browser --profile $SESSION --headed open https://myapp.com

# 2. User can see navigation happen
agent-browser --profile $SESSION snapshot -i

# 3. User watches AI click button
agent-browser --profile $SESSION click @e5

# 4. User sees form being filled
agent-browser --profile $SESSION fill @e6 "username"
agent-browser --profile $SESSION fill @e7 "password"

# 5. User watches submission
agent-browser --profile $SESSION click @e8

# 6. Capture proof
agent-browser --profile $SESSION screenshot result.png
```

## Best Practices

1. **Default to --headed** for user-initiated tasks
2. **Use --slow-mo** when demonstrating to users
3. **Add waits** so users can see state changes
4. **Capture screenshots** at key steps as records
5. **Switch to headless** for repetitive background work
