# Network & Storage

Manage cookies, local storage, and network requests.

## Cookies

### Get Cookies

```bash
# Get all cookies
agent-browser --profile $S cookies get

# Get specific cookie
agent-browser --profile $S cookies get --name "session"

# Get cookies for domain
agent-browser --profile $S cookies get --domain "example.com"

# JSON output
agent-browser --profile $S --json cookies get
```

### Set Cookies

```bash
# Set simple cookie
agent-browser --profile $S cookies set "name" "value"

# Set with domain
agent-browser --profile $S cookies set "token" "abc123" --domain "example.com"

# Set with path
agent-browser --profile $S cookies set "pref" "dark" --path "/app"

# Set with expiry (timestamp)
agent-browser --profile $S cookies set "session" "xyz" --expires 1735689600

# Set secure cookie
agent-browser --profile $S cookies set "auth" "secret" --secure --httpOnly
```

### Clear Cookies

```bash
# Clear all cookies
agent-browser --profile $S cookies clear

# Clear for specific domain
agent-browser --profile $S cookies clear --domain "example.com"

# Clear specific cookie
agent-browser --profile $S cookies clear --name "session"
```

## Local Storage

### Get Storage

```bash
# Get all localStorage
agent-browser --profile $S storage get local

# Get specific key
agent-browser --profile $S storage get local --key "user"

# JSON output
agent-browser --profile $S --json storage get local
```

### Set Storage

```bash
# Set localStorage item
agent-browser --profile $S storage set local "key" "value"

# Set JSON value
agent-browser --profile $S storage set local "settings" '{"theme":"dark","lang":"en"}'
```

### Clear Storage

```bash
# Clear all localStorage
agent-browser --profile $S storage clear local

# Clear localStorage for specific origin
agent-browser --profile $S storage clear local --origin "https://example.com"
```

## Session Storage

```bash
# Get sessionStorage
agent-browser --profile $S storage get session

# Set sessionStorage
agent-browser --profile $S storage set session "temp" "data"

# Clear sessionStorage
agent-browser --profile $S storage clear session
```

## Network Interception

### Route Requests

```bash
# Block requests to pattern
agent-browser --profile $S route block "**/ads/*"
agent-browser --profile $S route block "**/analytics/*"

# Modify responses
agent-browser --profile $S route mock "**/api/user" --body '{"name":"Test User"}'

# Abort requests
agent-browser --profile $S route abort "**/tracking/*"
```

### Wait for Network

```bash
# Wait for specific request
agent-browser --profile $S wait --network "**/api/data"

# Wait for network idle
agent-browser --profile $S wait --network idle
```

### Capture Requests

```bash
# Enable request logging
agent-browser --profile $S network log start

# Do actions...
agent-browser --profile $S click @e5

# Get logged requests
agent-browser --profile $S network log get
agent-browser --profile $S --json network log get

# Stop logging
agent-browser --profile $S network log stop
```

## Common Patterns

### Authentication Token

```bash
# Set auth token before loading page
agent-browser --profile $S storage set local "authToken" "Bearer xyz123"
agent-browser --profile $S open https://app.com/dashboard
# Page loads with authentication
```

### Clear State Before Test

```bash
# Fresh start
agent-browser --profile $S cookies clear
agent-browser --profile $S storage clear local
agent-browser --profile $S storage clear session
agent-browser --profile $S refresh
```

### Mock API Response

```bash
# Mock slow API for testing
agent-browser --profile $S route mock "**/api/users" \
  --body '[{"id":1,"name":"Test"}]' \
  --status 200

agent-browser --profile $S open https://app.com
# App uses mocked data
```

### Block Ads/Trackers

```bash
# Block common ad/tracking domains
agent-browser --profile $S route block "**/googleads.g.doubleclick.net/*"
agent-browser --profile $S route block "**/analytics.google.com/*"
agent-browser --profile $S route block "**/facebook.com/tr/*"
```

### Export/Import Cookies

```bash
# Export cookies
agent-browser --profile $S --json cookies get > cookies.json

# Import cookies (set each one)
# Use with jq or scripting to parse and set
```

## Best Practices

1. **Use profiles** for automatic cookie persistence
2. **Mock APIs** for reliable testing
3. **Block trackers** for faster page loads
4. **Export cookies** for sharing sessions
5. **Clear state** before each test run for reproducibility
