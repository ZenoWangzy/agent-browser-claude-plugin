---
name: web-scraping
description: Web scraping workflows and best practices using agent-browser
---

# Web Scraping Skill

Expert web scraping workflows using agent-browser with Playwright.

## When to Use

Use this skill when you need to:
- Extract data from websites
- Monitor website changes
- Aggregate content from multiple pages
- Harvest structured data
- Build datasets from web sources

## Core Workflows

### 1. Simple Text Extraction

```bash
# 使用 profile 保持会话隔离（推荐）
agent-browser --profile scraper_session goto https://example.com
agent-browser waitFor .content
agent-browser getText .content
```

**会话隔离说明**：使用 `--profile` 可以：
- 自动保存 cookies，避免重复登录
- 隔离不同爬取任务的状态
- 断点续传时保持登录状态

```bash
# 使用可视化模式调试爬虫
agent-browser --profile scraper_session --headed goto https://example.com
agent-browser waitFor .content
agent-browser getText .content
```

**可视化模式优势**：`--headed` 让浏览器可见，便于：
- 实时查看爬虫执行过程
- 验证选择器是否正确
- 调试动态加载问题

### 2. Multi-Element Scraping

```bash
# 使用语义定位器（推荐）
agent-browser --profile scraper_session goto https://shop.com/products
agent-browser waitFor text="Products"
agent-browser find role="link" name="Product" click
agent-browser waitFor text="Price"
agent-browser evaluate '
  Array.from(document.querySelectorAll("[role=\"listitem\"]"))
    .map(item => ({
      name: item.querySelector("[role=\"heading\"]")?.textContent,
      price: item.querySelector("[role=\"text\"]")?.textContent
    }))
'
```

**语义定位器优势**：
- `role="link" name="Product"` - 比 `.product-link` 更稳定
- `text="Price"` - 比嵌套 CSS 选择器更易维护
- `label="Email"` - 自动关联表单标签

### 3. Pagination Scraping（会话持久化）

```bash
# 使用 profile 自动保存 cookies（避免重复登录）
for page in {1..10}; do
  agent-browser --profile scraper_session goto https://example.com/page/$page
  agent-browser waitFor .item
  agent-browser getText .item >> data.json
done
```

**Cookie 持久化**：登录一次后，后续页面自动保持登录状态。

### 3. Pagination Scraping

```bash
# Scrape all pages
for page in {1..10}; do
  agent-browser goto https://example.com/page/$page
  agent-browser waitFor .item
  agent-browser getText .item >> data.json
done
```

### 4. Dynamic Content Scraping

```bash
# Wait for JavaScript-rendered content
agent-browser goto https://app.example.com/data
agent-browser waitFor .data-loaded
agent-browser evaluate '
  Array.from(document.querySelectorAll(".row"))
    .map(row => ({
      id: row.dataset.id,
      name: row.querySelector(".name").textContent,
      value: row.querySelector(".value").textContent
    }))
'
```

## Best Practices

### Selector Strategy
1. **Use stable selectors**: Prefer classes over nth-child
2. **Avoid layout selectors**: Don't use `div > div > span`
3. **Test selectors**: Use browser dev tools to verify
4. **Handle missing data**: Always check if elements exist

### Data Quality
1. **Clean whitespace**: Trim and normalize extracted text
2. **Handle encoding**: Use proper UTF-8 handling
3. **Validate data**: Check for expected formats
4. **Deduplicate**: Remove duplicate entries

### Performance
1. **Wait smartly**: Use `waitForSelector` not fixed timeouts
2. **Block resources**: Disable images/styles when possible
3. **Reuse sessions**: Keep browser open for multiple requests
4. **Rate limiting**: Add delays between requests

### Error Handling
1. **Retry failed requests**: Implement exponential backoff
2. **Log errors**: Save failed URLs for retry
3. **Handle CAPTCHAs**: Detect and pause for manual intervention
4. **Respect robots.txt**: Follow site policies

## Common Patterns

### Extract Links
```bash
agent-browser evaluate '
  Array.from(document.querySelectorAll("a[href]"))
    .map(a => a.href)
    .filter(href => href.startsWith("http"))
'
```

### Extract Tables
```bash
agent-browser evaluate '
  Array.from(document.querySelectorAll("table tr"))
    .slice(1)
    .map(row =>
      Array.from(row.querySelectorAll("td"))
        .map(td => td.textContent.trim())
    )
'
```

### Extract Images
```bash
agent-browser evaluate '
  Array.from(document.querySelectorAll("img"))
    .map(img => ({
      src: img.src,
      alt: img.alt,
      width: img.width,
      height: img.height
    }))
'
```

### Extract Meta Tags
```bash
agent-browser evaluate '
  {
    title: document.title,
    description: document.querySelector('meta[name="description"]')?.content,
    keywords: document.querySelector('meta[name="keywords"]")?.content,
    ogImage: document.querySelector('meta[property="og:image"]')?.content
  }
'
```

## Anti-Scraping Considerations

### Detection Signs
- Suspicious request patterns
- Missing headers
- JavaScript execution issues
- CAPTCHAs appearing

### Mitigation
1. **Rotate user agents**: Vary browser signatures
2. **Use delays**: Add random wait times
3. **Respect rate limits**: Check robots.txt
4. **Use proxy rotation**: Distribute requests
5. **Headless detection**: Some sites detect headless browsers

## Example: E-commerce Scraper

```bash
#!/bin/bash
# Scrape product data

agent-browser goto https://shop.com/category/electronics
agent-browser waitFor .product-card

agent-browser evaluate '
  Array.from(document.querySelectorAll(".product-card"))
    .map(card => ({
      name: card.querySelector(".title").textContent,
      price: card.querySelector(".price").textContent,
      rating: card.querySelector(".rating")?.dataset.value,
      url: card.querySelector("a").href
    }))
' > products.json

cat products.json | jq .
```

## Resources

- agent-browser docs: https://github.com/ZenoWangzy/agent-browser
- Playwright selectors: https://playwright.dev/docs/selectors
- CSS Selector Reference: https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors
