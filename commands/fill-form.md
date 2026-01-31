---
description: Automatically fill and submit web forms
---

# /fill-form - Form Automation Command

Automatically fill and submit web forms using agent-browser.

## Usage

```
/fill-form <url> <field-data>
```

## Field Data Format

Field data can be provided as JSON or key-value pairs:

```bash
# JSON format
/fill-form https://example.com/contact '{
  "name": "John Doe",
  "email": "john@example.com",
  "message": "Hello"
}'

# Key-value format
/fill-form https://example.com/contact name="John" email="john@example.com"
```

## Selectors

Field matching follows this priority:
1. `name` attribute
2. `id` attribute
3. `placeholder` text
4. Label text

## Examples

```bash
# Contact form
/fill-form https://example.com/contact '{
  "name": "John Doe",
  "email": "john@example.com",
  "subject": "Inquiry",
  "message": "I have a question"
}'

# Login form
/fill-form https://example.com/login '{
  "username": "user@example.com",
  "password": "secretpass"
}'

# With custom selectors
/fill-form https://example.com/form '{
  "#first-name": "John",
  "#last-name": "Doe",
  "input[type=\"email\"]": "john@example.com"
}'

# Submit after fill
/fill-form https://example.com/form '{...}' --submit
```

## Options

| Option | Description | Default |
|--------|-------------|---------|
| `--submit` | Submit form after filling | false |
| `--wait <ms>` | Wait before fill | 0 |
| `--click <selector>` | Click element after fill | - |

## Notes

- Automatically detects input types (text, email, password, etc.)
- Handles checkboxes and radio buttons
- Supports select dropdowns
- Waits for form to be ready before filling
