---
description: Scrape data from web pages
---

# /scrape - Web Scraping Command

Extract structured data from web pages using agent-browser.

## Usage

```
/scrape <url> <selector> [format]
```

## Formats

| Format | Description | Example Output |
|--------|-------------|----------------|
| `text` | Extract text content | `["Item 1", "Item 2"]` |
| `html` | Extract inner HTML | `["<span>Item 1</span>", ...]` |
| `attribute:<name>` | Extract attribute | `["https://...", ...]` |
| `json` | Extract as JSON object | `{items: [...]}` |

## Examples

```bash
# Scrape all links
/scrape https://example.com a text

# Scrape article titles
/scrape https://blog.com h2 text

# Scrape image URLs
/scrape https://gallery.com img attribute:src

# Scrape product data
/scrape https://shop.com .product json
```

## Advanced Usage

```bash
# Scrape with JavaScript evaluation
/scrape https://example.com '
  Array.from(document.querySelectorAll(".item"))
    .map(el => ({
      title: el.querySelector("h3").textContent,
      price: el.querySelector(".price").textContent
    }))
'

# Scrape pagination
/scrape https://list.com/page/1 .item text
```

## Notes

- Returns structured JSON output
- Handles dynamic content (waits for load)
- Supports CSS and XPath selectors
- Use JSON format for complex data structures
