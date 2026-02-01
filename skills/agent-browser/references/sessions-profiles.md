# Sessions & Profiles

Profiles provide isolated browser sessions with automatic cookie and state persistence.

## Why Use Profiles?

| Feature | Without Profile | With Profile |
|---------|-----------------|--------------|
| Cookie persistence | ❌ Lost on close | ✅ Saved automatically |
| Login state | ❌ Re-login each time | ✅ Stays logged in |
| Session isolation | ❌ Mixed state | ✅ Fully isolated |
| localStorage | ❌ Cleared | ✅ Persisted |
| Multiple tasks | ❌ Conflicts | ✅ Independent |

## Basic Usage

### Always Use --profile

```bash
# REQUIRED - Always specify a profile
agent-browser --profile myproject --headed open https://example.com

# BAD - No profile (state not isolated, cookies may conflict)
agent-browser open https://example.com  # Don't do this!
```

### Unique Session IDs

```bash
# Generate unique session ID
SESSION="task_$(date +%s)"
agent-browser --profile $SESSION --headed open https://example.com

# Or use descriptive names
agent-browser --profile github_work --headed open https://github.com
agent-browser --profile amazon_shopping --headed open https://amazon.com
```

## Cookie Persistence

### Login Once, Stay Logged In

```bash
# SESSION 1: First login
SESSION="github_persistent"
agent-browser --profile $SESSION --headed open https://github.com/login
agent-browser --profile $SESSION find label "Username" fill "myuser"
agent-browser --profile $SESSION find label "Password" fill "mypass"
agent-browser --profile $SESSION find role button click --name "Sign in"
agent-browser --profile $SESSION wait --url "**/dashboard"
# Cookies automatically saved!

# SESSION 2: Days later - still logged in!
agent-browser --profile $SESSION --headed open https://github.com
# No login needed - cookies restored automatically
```

### How It Works

1. **On close**: Cookies, localStorage, sessionStorage saved to profile directory
2. **On open**: State restored from profile
3. **Location**: `~/.agent-browser/profiles/<profile-name>/`

## Profile Data

Each profile stores:

```
~/.agent-browser/profiles/myprofile/
├── cookies.json          # All cookies
├── localStorage.json     # Local storage data
├── sessionStorage.json   # Session storage data
├── origins.json          # Origin-specific data
└── state.json            # Browser state
```

## Session Isolation

### Independent Sessions

```bash
# Task 1: Work account
agent-browser --profile work_github --headed open https://github.com
# Logged in as work@company.com

# Task 2: Personal account (completely separate session)
agent-browser --profile personal_github --headed open https://github.com
# Logged in as personal@gmail.com

# Both sessions run independently with their own cookies
```

### Parallel Tasks

```bash
# Can run simultaneously without interference
agent-browser --profile task_a --headed open https://site-a.com &
agent-browser --profile task_b --headed open https://site-b.com &
```

## Profile Management

### List Profiles

```bash
agent-browser profiles list
# Output:
# work_github (last used: 2024-01-15)
# personal_github (last used: 2024-01-14)
# amazon_shopping (last used: 2024-01-10)
```

### Delete Profile

```bash
# Delete specific profile
agent-browser profiles delete old_session

# Or manually remove
rm -rf ~/.agent-browser/profiles/old_session
```

### Clear All Profiles

```bash
agent-browser profiles clear
# Or
rm -rf ~/.agent-browser/profiles/*
```

## Best Practices

### 1. Naming Convention

```bash
# By purpose
agent-browser --profile "github_work"
agent-browser --profile "gmail_personal"

# By task
agent-browser --profile "scrape_job_$(date +%Y%m%d)"

# Temporary (with timestamp)
agent-browser --profile "temp_$(date +%s)"
```

### 2. Reuse vs Fresh Sessions

```bash
# REUSE: For persistent logins
agent-browser --profile github_work --headed open https://github.com

# FRESH: For clean-state testing
SESSION="test_$(date +%s)"
agent-browser --profile $SESSION --headed open https://example.com
```

### 3. Cleanup Old Profiles

```bash
# Periodically clean up old profiles
find ~/.agent-browser/profiles -maxdepth 1 -type d -mtime +30 -exec rm -rf {} \;
```

## Common Patterns

### Authentication Flow

```bash
SESSION="secure_app"

# First time: Full login
agent-browser --profile $SESSION --headed open https://secure.app.com/login
# ... login steps ...
# Cookies saved automatically

# Later uses: Skip login
agent-browser --profile $SESSION --headed open https://secure.app.com/dashboard
# Already authenticated!
```

### Multi-Account Testing

```bash
# Admin account
agent-browser --profile admin_account --headed open https://app.com
# Test admin features...

# Regular user account
agent-browser --profile user_account --headed open https://app.com
# Test user features...

# Both maintain separate sessions
```

### E-commerce Cart Persistence

```bash
SESSION="shopping_session"

# Day 1: Add items to cart
agent-browser --profile $SESSION --headed open https://store.com
# Add products...

# Day 2: Cart still has items!
agent-browser --profile $SESSION --headed open https://store.com/cart
# Continue shopping
