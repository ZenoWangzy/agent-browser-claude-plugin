---
name: e2e-testing
description: End-to-end testing workflows for web applications
---

# E2E Testing Skill

Expert workflows for end-to-end testing web applications using agent-browser.

## When to Use

Use this skill when you need to:
- Test user flows end-to-end
- Verify application functionality
- Test across different browsers
- Perform visual regression testing
- Test authentication flows
- Validate form submissions
- Test responsive layouts

## Core Workflows

### Basic User Flow Test

```bash
# Test signup flow
agent-browser goto https://example.com/signup
agent-browser waitFor #signup-form

agent-browser fill #username "testuser"
agent-browser fill #email "test@example.com"
agent-browser fill #password "TestPass123!"
agent-browser click button[type="submit"]

agent-browser waitFor .welcome-message
agent-browser getText .welcome-message

# Verify success
agent-browser evaluate '
  const welcome = document.querySelector(".welcome-message");
  return {
    found: welcome !== null,
    text: welcome?.textContent
  };
'
```

### Authentication Test

```bash
# Test login flow
agent-browser goto https://example.com/login
agent-browser waitFor #login-form

agent-browser fill #email "user@example.com"
agent-browser fill #password "password123"
agent-browser click button[type="submit"]

# Should redirect to dashboard
agent-browser waitFor .dashboard
agent-browser evaluate '
  return {
    onDashboard: document.querySelector(".dashboard") !== null,
    url: window.location.href
  };
'
```

### Form Validation Test

```bash
# Test form validation
agent-browser goto https://example.com/contact

# Submit empty form
agent-browser click button[type="submit"]

# Check for errors
agent-browser waitFor .error-message
agent-browser evaluate '
  const errors = document.querySelectorAll(".error-message");
  return {
    count: errors.length,
    messages: Array.from(errors).map(e => e.textContent)
  };
'
```

## Test Patterns

### Happy Path Testing
```bash
# Test the ideal user journey
agent-browser goto https://example.com

agent-browser click .cta-button
agent-browser waitFor .pricing-page

agent-browser click .plan-card:first-child .subscribe
agent-browser waitFor .checkout-form

agent-browser fill #email "test@example.com"
agent-browser click .submit-payment

agent-browser waitFor .success-message
```

### Error Path Testing
```bash
# Test error handling
agent-browser goto https://example.com/login

agent-browser fill #email "invalid@email"
agent-browser fill #password "wrong"
agent-browser click button[type="submit"]

agent-browser waitFor .error-message
agent-browser evaluate '
  const error = document.querySelector(".error-message");
  return {
    shown: error !== null,
    text: error?.textContent
  };
'
```

### Edge Case Testing
```bash
# Test with empty inputs
agent-browser goto https://example.com/search
agent-browser click .search-button
agent-browser waitFor .error-message

# Test with special characters
agent-browser fill #search "<script>alert('xss')</script>"
agent-browser click .search-button
agent-browser evaluate '
  const sanitized = !document.querySelector("script");
  return { sanitized };
'
```

## Assertions

### Text Assertions
```bash
agent-browser evaluate '
  const title = document.querySelector("h1").textContent;
  return {
    assert: title === "Expected Title",
    actual: title
  };
'
```

### Element Presence
```bash
agent-browser evaluate '
  return {
    elementExists: document.querySelector(".my-element") !== null
  };
'
```

### Visibility Assertions
```bash
agent-browser evaluate '
  const el = document.querySelector(".modal");
  const styles = window.getComputedStyle(el);
  return {
    isVisible: styles.display !== "none" && styles.visibility !== "hidden"
  };
'
```

### URL Assertions
```bash
agent-browser evaluate '
  return {
    currentUrl: window.location.href,
    path: window.location.pathname
  };
'
```

## Cross-Browser Testing

```bash
# Test in Chromium
BROWSER=chromium agent-browser test

# Test in Firefox
BROWSER=firefox agent-browser test

# Test in WebKit
BROWSER=webkit agent-browser test
```

## Responsive Testing

```bash
# Mobile
agent-browser viewport 375 667
agent-browser goto https://example.com
agent-browser screenshot mobile-test.png
agent-browser evaluate '
  const menu = document.querySelector(".mobile-menu");
  return { mobileMenuVisible: menu !== null };
'

# Tablet
agent-browser viewport 768 1024
agent-browser goto https://example.com
agent-browser screenshot tablet-test.png

# Desktop
agent-browser viewport 1920 1080
agent-browser goto https://example.com
agent-browser screenshot desktop-test.png
```

## Data-Driven Testing

```bash
# Test with multiple datasets
TEST_DATA='[
  {"email": "user1@example.com", "expected": "success"},
  {"email": "invalid", "expected": "error"},
  {"email": "", "expected": "required"}
]'

echo "$TEST_DATA" | jq -c '.[]' | while read -r test; do
  EMAIL=$(echo "$test" | jq -r '.email')
  EXPECTED=$(echo "$test" | jq -r '.expected')

  agent-browser goto https://example.com/signup
  agent-browser fill #email "$EMAIL"
  agent-browser click button[type="submit"]

  # Verify expected result
  agent-browser waitFor ".$EXPECTED"
done
```

## Test Organization

### Test Files Structure
```
tests/
├── e2e/
│   ├── authentication/
│   │   ├── login.test.sh
│   │   ├── signup.test.sh
│   │   └── password-reset.test.sh
│   ├── checkout/
│   │   ├── cart.test.sh
│   │   └── payment.test.sh
│   └── api/
│       └── endpoints.test.sh
```

### Test Template
```bash
#!/bin/bash
# test-name.test.sh

setup() {
  agent-browser goto https://example.com
}

test_something() {
  agent-browser click .button
  agent-browser waitFor .result

  local result=$(agent-browser evaluate '
    return document.querySelector(".result").textContent
  ')

  if [[ "$result" == "Expected" ]]; then
    echo "PASS"
  else
    echo "FAIL: Expected 'Expected', got '$result'"
    exit 1
  fi
}

teardown() {
  agent-browser close
}

setup
test_something
teardown
```

## Visual Regression Testing

```bash
# Baseline
agent-browser goto https://example.com
agent-browser screenshot tests/screenshots/baseline/homepage.png

# Compare
agent-browser goto https://example.com
agent-browser screenshot tests/screenshots/current/homepage.png

# Diff (using ImageMagick)
compare tests/screenshots/baseline/homepage.png \
        tests/screenshots/current/homepage.png \
        tests/screenshots/diff/homepage.png
```

## API Testing

```bash
# Test API calls from browser
agent-browser goto https://example.com

# Monitor network requests
agent-browser evaluate '
  const requests = [];
  const originalFetch = window.fetch;
  window.fetch = (...args) => {
    requests.push({ url: args[0], options: args[1] });
    return originalFetch(...args);
  };
  return requests;
'

# Trigger API call
agent-browser click .load-data

# Verify request was made
agent-browser evaluate '
  return requests.filter(r => r.url.includes("/api/data"));
'
```

## Best Practices

### 1. Use Data-testid Attributes
```html
<!-- GOOD -->
<button data-testid="submit-button">Submit</button>

<!-- BAD (may change) -->
<button class="btn btn-primary">Submit</button>
```

### 2. Wait for Elements
```bash
# GOOD
agent-browser waitFor .loading[hidden]
agent-browser click .button

# BAD
sleep 3
agent-browser click .button
```

### 3. Clean Up
```bash
# Clear localStorage between tests
agent-browser evaluate 'localStorage.clear()'

# Clear cookies
agent-browser evaluate '
  document.cookie.split(";").forEach(c => {
    document.cookie = c.trim().split("=")[0] + "=;expires=Thu, 01 Jan 1970 00:00:00 GMT";
  });
'
```

### 4. Use Page Objects
```bash
# Page object for login page
login_page() {
  agent-browser goto https://example.com/login
  agent-browser waitFor #login-form
}

fill_credentials() {
  agent-browser fill #email "test@example.com"
  agent-browser fill #password "password123"
}

submit_login() {
  agent-browser click button[type="submit"]
  agent-browser waitFor .dashboard
}
```

## Common Scenarios

### Shopping Cart Test
```bash
agent-browser goto https://shop.com/product
agent-browser click .add-to-cart
agent-browser waitFor .cart-count

agent-browser goto https://shop.com/cart
agent-browser waitFor .cart-item

agent-browser click .checkout-button
agent-browser waitFor .checkout-form
```

### Search Test
```bash
agent-browser goto https://example.com
agent-browser fill #search "test query"
agent-browser click .search-button

agent-browser waitFor .search-results
agent-browser evaluate '
  return document.querySelectorAll(".search-result").length > 0;
'
```

### Navigation Test
```bash
agent-browser goto https://example.com

agent-browser click nav a[href="/about"]
agent-browser waitFor .about-page

agent-browser evaluate '
  return window.location.pathname === "/about";
'
```

## Continuous Integration

```yaml
# .github/workflows/e2e.yml
name: E2E Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install agent-browser
        run: cargo install agent-browser
      - name: Run E2E tests
        run: |
          agent-browser daemon &
          bash tests/e2e/run-all.sh
```

## Resources

- agent-browser docs: https://github.com/ZenoWangzy/agent-browser
- Playwright testing: https://playwright.dev/docs/intro
- Testing best practices: https://testingjavascript.com/
