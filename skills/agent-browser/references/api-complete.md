# Complete API Reference

Full command reference for agent-browser CLI. Commands marked with 🔷 support `--json` output.

## Global Flags

| Flag | Description | Example |
|------|-------------|---------|
| `--profile <name>` | Session profile (REQUIRED for isolation) | `--profile myproject` |
| `--headed` | Show browser window (RECOMMENDED) | `--headed` |
| `--json` | JSON output (token efficient) | `--json` |
| `--timeout <ms>` | Command timeout | `--timeout 30000` |
| `--slow-mo <ms>` | Slow down actions | `--slow-mo 100` |

## Navigation

### open

Navigate to URL.

```bash
agent-browser --profile $S open <url>
agent-browser --profile $S --headed open https://example.com
agent-browser --profile $S open "https://example.com/search?q=test"
```

### back / forward

Browser history navigation.

```bash
agent-browser --profile $S back
agent-browser --profile $S forward
```

### refresh

Reload current page.

```bash
agent-browser --profile $S refresh
```

## Page Information

### 🔷 get title

Get page title.

```bash
agent-browser --profile $S get title
agent-browser --profile $S --json get title
```

### 🔷 get url

Get current URL.

```bash
agent-browser --profile $S get url
agent-browser --profile $S --json get url
```

### 🔷 get text

Get text content of element.

```bash
agent-browser --profile $S get text <selector>
agent-browser --profile $S get text ".content"
agent-browser --profile $S get text @e5
agent-browser --profile $S --json get text "h1"
```

### 🔷 get html

Get HTML of element.

```bash
agent-browser --profile $S get html <selector>
agent-browser --profile $S get html "main"
agent-browser --profile $S get html @e3 --outer  # Include element itself
```

### 🔷 get attribute

Get element attribute.

```bash
agent-browser --profile $S get attribute <selector> <attr>
agent-browser --profile $S get attribute "a" href
agent-browser --profile $S get attribute @e5 "data-id"
```

### 🔷 get value

Get input value.

```bash
agent-browser --profile $S get value <selector>
agent-browser --profile $S get value "#email"
```

## Interaction

### click

Click element.

```bash
agent-browser --profile $S click <selector|ref>
agent-browser --profile $S click @e5
agent-browser --profile $S click ".submit-button"
agent-browser --profile $S click "text=Submit"
```

### dblclick

Double-click element.

```bash
agent-browser --profile $S dblclick <selector|ref>
```

### fill

Fill input with text.

```bash
agent-browser --profile $S fill <selector|ref> <text>
agent-browser --profile $S fill @e3 "hello world"
agent-browser --profile $S fill "#email" "user@example.com"
agent-browser --profile $S fill @e5 ""  # Clear input
```

### type

Type text character by character (with key events).

```bash
agent-browser --profile $S type <selector|ref> <text>
agent-browser --profile $S type @e3 "hello" --delay 100
```

### press

Press keyboard key.

```bash
agent-browser --profile $S press <key>
agent-browser --profile $S press Enter
agent-browser --profile $S press Tab
agent-browser --profile $S press Escape
agent-browser --profile $S press Control+a
agent-browser --profile $S press Meta+c  # Cmd+C on Mac
```

### hover

Hover over element.

```bash
agent-browser --profile $S hover <selector|ref>
agent-browser --profile $S hover @e4
agent-browser --profile $S hover ".dropdown-menu"
```

### check / uncheck

Toggle checkbox.

```bash
agent-browser --profile $S check <selector|ref>
agent-browser --profile $S uncheck <selector|ref>
```

### select

Select dropdown option.

```bash
agent-browser --profile $S select <selector> <value>
agent-browser --profile $S select "#country" "US"
agent-browser --profile $S select @e7 --label "United States"
agent-browser --profile $S select @e7 --index 3
```

### focus / blur

Focus or blur element.

```bash
agent-browser --profile $S focus <selector>
agent-browser --profile $S blur <selector>
```

### scroll

Scroll page or element.

```bash
agent-browser --profile $S scroll <direction> [amount]
agent-browser --profile $S scroll down 500
agent-browser --profile $S scroll up 200
agent-browser --profile $S scroll @e5  # Scroll element into view
```

## Semantic Locators

### find

Locate and interact using semantic selectors.

```bash
agent-browser --profile $S find <type> <value> [action]

# Types: role, text, label, placeholder, alt, testid
agent-browser --profile $S find role button click
agent-browser --profile $S find role button click --name "Submit"
agent-browser --profile $S find text "More info" click
agent-browser --profile $S find label "Email" fill "a@b.com"
agent-browser --profile $S find placeholder "Search" fill "query"
agent-browser --profile $S find testid "submit-btn" click

# Modifiers
agent-browser --profile $S find role button first click
agent-browser --profile $S find role button last click
agent-browser --profile $S find role button nth 2 click
```

## Snapshot & Refs

### 🔷 snapshot

Capture page elements with refs.

```bash
agent-browser --profile $S snapshot -i
agent-browser --profile $S --json snapshot -i
agent-browser --profile $S snapshot -i --selector "form"
agent-browser --profile $S snapshot -i --depth 5
agent-browser --profile $S snapshot -i --visible
```

## Screenshots & PDF

### screenshot

Capture screenshot.

```bash
agent-browser --profile $S screenshot <filename>
agent-browser --profile $S screenshot page.png
agent-browser --profile $S screenshot --fullpage full.png
agent-browser --profile $S screenshot --selector "main" section.png
agent-browser --profile $S screenshot @e5 element.png
```

### pdf

Generate PDF (print mode).

```bash
agent-browser --profile $S pdf <filename>
agent-browser --profile $S pdf output.pdf
agent-browser --profile $S pdf output.pdf --format A4
```

## Waiting

### wait

Wait for conditions.

```bash
# Wait for text to appear
agent-browser --profile $S wait --text "Loading complete"

# Wait for URL match
agent-browser --profile $S wait --url "**/dashboard"
agent-browser --profile $S wait --url "https://example.com/success"

# Wait for element
agent-browser --profile $S wait --selector ".loaded"
agent-browser --profile $S wait --selector @e5

# Wait for network idle
agent-browser --profile $S wait --network idle

# Wait with timeout
agent-browser --profile $S wait --text "Done" --timeout 10000

# Wait for JavaScript condition
agent-browser --profile $S wait --fn "document.querySelector('.loaded') !== null"
```

## Cookies & Storage

### 🔷 cookies get

Get cookies.

```bash
agent-browser --profile $S cookies get
agent-browser --profile $S --json cookies get
agent-browser --profile $S cookies get --name "session"
```

### cookies set

Set cookie.

```bash
agent-browser --profile $S cookies set <name> <value> [options]
agent-browser --profile $S cookies set "session" "abc123"
agent-browser --profile $S cookies set "token" "xyz" --domain "example.com"
```

### cookies clear

Clear cookies.

```bash
agent-browser --profile $S cookies clear
agent-browser --profile $S cookies clear --domain "example.com"
```

### 🔷 storage get

Get localStorage/sessionStorage.

```bash
agent-browser --profile $S storage get local
agent-browser --profile $S storage get session
agent-browser --profile $S storage get local --key "user"
```

### storage set

Set storage item.

```bash
agent-browser --profile $S storage set local <key> <value>
agent-browser --profile $S storage set session "token" "abc"
```

### storage clear

Clear storage.

```bash
agent-browser --profile $S storage clear local
agent-browser --profile $S storage clear session
```

## Debugging

### 🔷 console

Get console logs.

```bash
agent-browser --profile $S console
agent-browser --profile $S --json console
agent-browser --profile $S console --level error
```

### 🔷 errors

Get JavaScript errors.

```bash
agent-browser --profile $S errors
agent-browser --profile $S --json errors
```

### trace

Enable tracing.

```bash
agent-browser --profile $S trace start
agent-browser --profile $S trace stop trace.zip
```

## Evaluate

### 🔷 eval

Execute JavaScript.

```bash
agent-browser --profile $S eval <script>
agent-browser --profile $S eval "document.title"
agent-browser --profile $S eval "window.scrollTo(0, 1000)"
agent-browser --profile $S --json eval "JSON.stringify(window.performance.timing)"
```

## Session Management

### close

Close browser/tab.

```bash
agent-browser --profile $S close        # Close current tab
agent-browser --profile $S close --all  # Close all tabs
```

### profiles

Manage profiles.

```bash
agent-browser profiles list            # List all profiles
agent-browser profiles delete myproj   # Delete profile
agent-browser profiles clear           # Clear all profiles
```

## Quick Reference

### Most Common Workflow

```bash
SESSION="task_$(date +%s)"
agent-browser --profile $SESSION --headed open https://example.com
agent-browser --profile $SESSION snapshot -i
agent-browser --profile $SESSION click @e5
agent-browser --profile $SESSION fill @e6 "text"
agent-browser --profile $SESSION screenshot result.png
```

### Token-Efficient Pattern

```bash
agent-browser --profile $S --headed open https://example.com
agent-browser --profile $S --json snapshot -i
agent-browser --profile $S click @e3
agent-browser --profile $S --json get title
```
