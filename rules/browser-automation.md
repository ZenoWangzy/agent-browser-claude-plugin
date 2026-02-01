# Browser Automation Rules

Always-follow guidelines for browser automation tasks with agent-browser.

## 0. Profile Isolation (CRITICAL)

ALWAYS use `--profile` for session isolation:

```bash
# GOOD - Isolated session with cookie persistence
agent-browser --profile myproject --headed open https://example.com

# BAD - Shared state, no cookie persistence
agent-browser open https://example.com
```

**WHY**: Profiles provide session isolation and automatic cookie saving.

## 0.1. Visual Mode (RECOMMENDED)

ALWAYS use `--headed` when user is monitoring:

```bash
# GOOD - User can see browser actions
agent-browser --profile $S --headed open https://example.com

# ACCEPTABLE - Headless for batch operations
agent-browser --profile $S open https://example.com
```

**WHY**: Users see exactly what AI is doing, building trust and aiding debugging.

## 0.2. Semantic Locators (PREFERRED)

PREFER semantic locators over CSS selectors:

```bash
# GOOD - Semantic, stable
agent-browser --profile $S find role button click --name "Submit"
agent-browser --profile $S find label "Email" fill "user@test.com"

# ACCEPTABLE - When semantic not possible
agent-browser --profile $S click ".submit-button"

# BAD - Fragile selectors
agent-browser --profile $S click "div > div > button"
```

**WHY**: Semantic locators are more stable and AI-friendly.

## 1. Selector Stability (CRITICAL)

ALWAYS use stable, semantic selectors:

```css
/* GOOD - Semantic classes */
.product-card
.user-profile
.submit-button

/* BAD - Generated or layout-based */
.css-1a2b3c
div > div > span
:nth-child(3)
```

**WHY**: Layout changes and CSS-in-JS generate unstable selectors that break easily.

## 2. Wait Strategies (CRITICAL)

ALWAYS wait for elements before interaction:

```bash
# GOOD - Wait for specific element
agent-browser waitFor .load-complete
agent-browser click .button

# BAD - Fixed timeout
sleep 5
agent-browser click .button  # May not be ready!
```

**WHY**: Network and rendering times vary. Fixed timeouts either waste time or fail too soon.

## 3. Resource Cleanup

ALWAYS clean up browser resources:

```bash
# GOOD - Clean shutdown
agent-browser goto https://example.com
agent-browser screenshot
agent-browser close  # Explicit cleanup

# ACCEPTABLE - Let daemon handle
# Browser daemon auto-closes after timeout
```

**WHY**: Orphaned browser processes consume memory and can hit system limits.

## 4. Error Handling

ALWAYS handle errors gracefully:

```bash
# GOOD - Check if element exists
agent-browser evaluate '
  const el = document.querySelector(".button");
  if (el) {
    el.click();
    return { success: true };
  }
  return { success: false, error: "Element not found" };
'

# BAD - Assume it exists
agent-browser click .button  # Throws if missing!
```

**WHY**: Pages change, load differently, or have dynamic content.

## 5. Screenshot for Debugging

ALWAYS take screenshots when debugging visual issues:

```bash
agent-browser goto https://example.com
agent-browser screenshot before-click.png
agent-browser click .button
agent-browser screenshot after-click.png
```

**WHY**: Screenshots reveal rendering issues, overlays, and element states.

## 6. Rate Limiting

ALWAYS respect rate limits and add delays:

```bash
# GOOD - Respectful scraping
for url in "${urls[@]}"; do
  agent-browser goto "$url"
  agent-browser scrape .data
  sleep 2  # Polite delay
done

# BAD - Aggressive requests
for url in "${urls[@]}"; do
  agent-browser goto "$url"
  agent-browser scrape .data
  # No delay - may trigger IP bans!
done
```

**WHY**: Aggressive automation triggers IP bans and CAPTCHAs.

## 7. Viewport Awareness

ALWAYS set appropriate viewport for target:

```bash
# Mobile testing
agent-browser viewport 375 667

# Desktop testing
agent-browser viewport 1920 1080

# Tablet testing
agent-browser viewport 768 1024
```

**WHY**: Responsive sites behave differently at various viewports.

## 8. Secure Data Handling

NEVER log sensitive information:

```bash
# GOOD - Sanitize output
agent-browser evaluate '
  document.querySelector("#password").value = "***";
  { status: "filled" };
'

# BAD - Exposes secrets
agent-browser fill #password "mySecretPassword123"
```

**WHY**: Logs may be shared or stored, exposing credentials.

## 9. Headless Detection Awareness

BE AWARE some sites detect headless browsers:

```bash
# Mitigation - Use realistic user agent
agent-browser evaluate '
  Object.defineProperty(navigator, "webdriver", {
    get: () => undefined
  });
'
```

**WHY**: Some sites block or behave differently for headless browsers.

## 10. Test Locality

PREFER specific selectors over broad searches:

```bash
# GOOD - Search within container
agent-browser getText .product-list .price

# ACCEPTABLE - If container doesn't exist
agent-browser getText .price

# BAD - Too broad, may match unrelated elements
agent-browser getText span
```

**WHY**: Specific selectors are faster and less error-prone.

---

**Remember**: These rules ensure reliable, efficient, and respectful browser automation.
