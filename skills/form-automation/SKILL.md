---
name: form-automation
description: Automated form filling and submission workflows
---

# Form Automation Skill

Expert workflows for automatically filling and submitting web forms using agent-browser.

## When to Use

Use this skill when you need to:
- Fill out contact forms
- Submit registration forms
- Automate login processes
- Fill surveys or questionnaires
- Submit data through web forms

## Core Workflows

### Simple Form Fill

```bash
agent-browser goto https://example.com/contact
agent-browser waitFor #name
agent-browser fill #name "John Doe"
agent-browser fill #email "john@example.com"
agent-browser fill #message "Hello, world!"
agent-browser click button[type="submit"]
```

### Smart Field Detection

The form automation intelligently detects fields by:
1. `name` attribute
2. `id` attribute
3. `placeholder` text
4. Associated label text

### JSON-based Form Fill

```bash
agent-browser goto https://example.com/register

# Use /fill-form command
/fill-form https://example.com/register '{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "securepass123",
  "terms": true
}'
```

## Field Types

### Text Inputs
```bash
agent-browser fill #firstName "John"
agent-browser fill #lastName "Doe"
agent-browser fill #email "john@example.com"
```

### Textarea
```bash
agent-browser fill #message "This is a multi-line message."
```

### Select Dropdowns
```bash
agent-browser select #country "United States"
agent-browser select #state "California"
```

### Checkboxes
```bash
agent-browser check #terms
agent-browser uncheck #newsletter
```

### Radio Buttons
```bash
agent-browser click input[name="gender"][value="male"]
```

### File Uploads
```bash
agent-browser upload #avatar /path/to/image.png
```

## Best Practices

### 1. Wait for Form
```bash
# GOOD
agent-browser goto https://example.com/form
agent-browser waitFor form
agent-browser fill #name "John"

# BAD
agent-browser goto https://example.com/form
agent-browser fill #name "John"  # May fail if form not ready
```

### 2. Validate Before Submit
```bash
agent-browser fill #name "John"
agent-browser fill #email "john@example.com"

# Verify fields are filled
agent-browser evaluate '
  const name = document.querySelector("#name").value;
  const email = document.querySelector("#email").value;
  if (!name || !email) {
    return { error: "Fields not filled" };
  }
  return { success: true };
'

agent-browser click button[type="submit"]
```

### 3. Handle Success/Error
```bash
agent-browser click button[type="submit"]

# Check for success message
agent-browser waitFor .success-message
agent-browser getText .success-message

# Or check for errors
agent-browser evaluate '
  const errors = document.querySelectorAll(".error-message");
  if (errors.length > 0) {
    return Array.from(errors).map(e => e.textContent);
  }
  return null;
'
```

### 4. Rate Limiting
```bash
# Add delays between fills
agent-browser fill #field1 "value1"
agent-browser evaluate 'new Promise(r => setTimeout(r, 500))'
agent-browser fill #field2 "value2"
```

## Common Patterns

### Registration Form
```bash
agent-browser goto https://example.com/signup

agent-browser fill #username "newuser"
agent-browser fill #email "user@example.com"
agent-browser fill #password "SecurePass123!"
agent-browser fill #confirmPassword "SecurePass123!"
agent-browser check #agreeTerms

agent-browser click button[type="submit"]
agent-browser waitFor .welcome-message
```

### Login Form
```bash
agent-browser goto https://example.com/login

agent-browser fill #email "user@example.com"
agent-browser fill #password "mypassword"
agent-browser click button[type="submit"]

agent-browser waitFor .user-profile
```

### Multi-step Form
```bash
# Step 1
agent-browser goto https://example.com/wizard
agent-browser fill #name "John"
agent-browser click .next-button

agent-browser waitFor .step-2
agent-browser fill #address "123 Main St"
agent-browser click .next-button

agent-browser waitFor .step-3
agent-browser click .submit-button
```

### Search Form
```bash
agent-browser goto https://example.com/search
agent-browser fill #search "browser automation"
agent-browser click .search-button

agent-browser waitFor .results
agent-browser getText .result-item
```

## Anti-Automation Considerations

### Honeypot Fields
Some forms include hidden fields to detect bots:
```bash
# Check for hidden fields
agent-browser evaluate '
  const honeypots = document.querySelectorAll("input[type=\"hidden\"][name*=\"bot\"], input[name*=\"honeypot\"]");
  Array.from(honeypots).map(h => h.name);
'
```

### CAPTCHA Handling
If you encounter CAPTCHA:
1. Take screenshot to verify
2. Pause for manual intervention
3. Use CAPTCHA solving services (if legal)
4. Consider alternative approaches

### Rate Limiting
```bash
# Add random delays
agent-browser evaluate 'const delay = Math.random() * 2000 + 1000; new Promise(r => setTimeout(r, delay));'
```

## Security Notes

⚠️ **Never log or store sensitive information**:
- Passwords
- Credit card numbers
- SSN/Tax IDs
- API keys

Use placeholder values in logs:
```bash
# GOOD
agent-browser fill #password "***"
log "Password filled"

# BAD
agent-browser fill #password "MySecretPassword123"
log "Filled password: MySecretPassword123"
```

## Resources

- agent-browser docs: https://github.com/ZenoWangzy/agent-browser
- Form best practices: https://www.smashingmagazine.com/
- Playwright forms: https://playwright.dev/docs/input
