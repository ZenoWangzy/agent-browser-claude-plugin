---
name: screenshot-capture
description: Screenshot and visual capture workflows
---

# Screenshot Capture Skill

Expert workflows for capturing screenshots and visual content using agent-browser.

## When to Use

Use this skill when you need to:
- Capture webpage screenshots
- Document visual states
- Create visual documentation
- Debug rendering issues
- Monitor website changes
- Archive web content

## Core Workflows

### Full Page Screenshot

```bash
agent-browser goto https://example.com
agent-browser waitFor body
agent-browser screenshot --full fullpage.png
```

### Element Screenshot

```bash
agent-browser goto https://example.com
agent-browser waitFor .hero-section
agent-browser screenshot .hero-section hero.png
```

### Viewport Screenshot

```bash
# Set viewport first
agent-browser viewport 1920 1080
agent-browser goto https://example.com
agent-browser screenshot desktop.png

agent-browser viewport 375 667
agent-browser screenshot mobile.png
```

## Screenshot Types

### 1. Full Page Capture
Captures entire scrollable page:
```bash
agent-browser screenshot --full page.png
```

### 2. Viewport Capture
Captures only visible area:
```bash
agent-browser screenshot visible.png
```

### 3. Element Capture
Captures specific element:
```bash
agent-browser screenshot .header header.png
```

### 4. Multiple Elements
```bash
# Capture all product cards
agent-browser evaluate '
  const cards = document.querySelectorAll(".product-card");
  cards.forEach((card, i) => {
    card.classList.add(`screenshot-${i}`);
  });
  return cards.length;
'

agent-browser screenshot .screenshot-0 product-0.png
agent-browser screenshot .screenshot-1 product-1.png
# ... etc
```

## Best Practices

### 1. Wait for Content
```bash
# GOOD - Wait for critical content
agent-browser goto https://example.com
agent-browser waitFor .content-loaded
agent-browser screenshot

# BAD - May capture partially loaded page
agent-browser goto https://example.com
agent-browser screenshot
```

### 2. Handle Overlays
```bash
# Close cookie banners first
agent-browser click .cookie-banner-close
agent-browser waitFor .cookie-banner[hidden]
agent-browser screenshot

# Or evaluate before screenshot
agent-browser evaluate '
  document.querySelector(".modal")?.remove();
  document.querySelector(".overlay")?.remove();
'
agent-browser screenshot
```

### 3. Stable Viewport
```bash
# Set consistent viewport
agent-browser viewport 1280 720
agent-browser goto https://example.com
agent-browser screenshot
```

### 4. Clean State
```bash
# Clear focus states
agent-browser evaluate 'document.activeElement.blur()'

# Clear selections
agent-browser evaluate 'window.getSelection().removeAllRanges()'

# Close tooltips
agent-browser evaluate 'document.querySelectorAll(".tooltip").forEach(t => t.remove())'

agent-browser screenshot
```

## Advanced Workflows

### Before/After Comparison
```bash
# Before action
agent-browser screenshot before.png

# Perform action
agent-browser click .button
agent-browser waitFor .result

# After action
agent-browser screenshot after.png
```

### Responsive Screenshots
```bash
# Desktop
agent-browser viewport 1920 1080
agent-browser goto https://example.com
agent-browser screenshot desktop.png

# Tablet
agent-browser viewport 768 1024
agent-browser screenshot tablet.png

# Mobile
agent-browser viewport 375 667
agent-browser screenshot mobile.png
```

### Screenshot with Timestamp
```bash
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
agent-browser goto https://example.com
agent-browser screenshot "screenshot_${TIMESTAMP}.png"
```

### Progressive Loading Capture
```bash
# Capture at different stages
agent-browser goto https://example.com
agent-browser screenshot 1_initial.png

agent-browser waitFor header
agent-browser screenshot 2_header.png

agent-browser waitFor .content
agent-browser screenshot 3_content.png

agent-browser waitFor footer
agent-browser screenshot 4_complete.png
```

### Archive/LPR Screenshots
```bash
# High resolution for archival
agent-browser goto https://example.com
agent-browser viewport 3840 2160
agent-browser screenshot --full archive.png
```

## Debugging Screenshots

### Visual Regression Testing
```bash
# Baseline
agent-browser goto https://example.com
agent-browser screenshot baseline.png

# After changes
agent-browser goto https://example.com
agent-browser screenshot current.png

# Compare (using ImageMagick)
compare baseline.png current.png difference.png
```

### Element Issue Debugging
```bash
# Capture page
agent-browser screenshot page.png

# Capture specific problematic element
agent-browser screenshot .problematic-element element.png

# Get element info
agent-browser evaluate '
  const el = document.querySelector(".problematic-element");
  return {
    visible: el.offsetParent !== null,
    styles: window.getComputedStyle(el),
    rect: el.getBoundingClientRect()
  };
'
```

### Console Error Capture
```bash
agent-browser goto https://example.com

# Check for console errors
agent-browser evaluate '
  const errors = [];
  const originalError = console.error;
  console.error = (...args) => errors.push(args);
  return errors;
'

agent-browser screenshot
```

## PDF Generation

### Full Page PDF
```bash
agent-browser goto https://example.com
agent-browser pdf page.pdf
```

### PDF with Options
```bash
agent-browser evaluate '
  await page.pdf({
    format: "A4",
    printBackground: true,
    margin: { top: "1cm", right: "1cm", bottom: "1cm", left: "1cm" }
  });
'
```

## Output Options

### File Formats
```bash
# PNG (default, lossless)
agent-browser screenshot image.png

# JPEG (smaller file)
agent-browser screenshot image.jpg

# WebP (modern, efficient)
agent-browser screenshot image.webp
```

### Quality Settings
```bash
# High quality
agent-browser screenshot --quality 100 high.png

# Balanced (default)
agent-browser screenshot medium.png

# Low quality (smaller file)
agent-browser screenshot --quality 50 low.png
```

### Naming Conventions
```bash
# Descriptive names
agent-browser screenshot homepage-hero.png
agent-browser screenshot checkout-form.png
agent-browser screenshot pricing-table.png

# With timestamps
agent-browser screenshot "backup_$(date +%Y%m%d).png"

# With context
agent-browser screenshot "prod-homepage-v1.2.3.png"
```

## Common Use Cases

### Documentation
```bash
# Capture UI states for docs
agent-browser goto https://example.com/features
agent-browser screenshot docs/ui-features.png
```

### Monitoring
```bash
# Scheduled screenshots for uptime monitoring
agent-browser goto https://example.com
agent-browser screenshot "monitors/$(date +%Y%m%d_%H%M).png"
```

### Archival
```bash
# Archive important pages
agent-browser goto https://example.com/important-page
agent-browser screenshot --full "archive/important_$(date +%Y%m%d).png"
```

### Social Media Preview
```bash
# Capture for sharing
agent-browser viewport 1200 630
agent-browser goto https://example.com/article
agent-browser screenshot social-preview.png
```

## Tips

1. **Use consistent viewports** for comparable screenshots
2. **Wait for animations** to complete before capturing
3. **Handle dynamic content** with proper waits
4. **Close popups/modals** before full-page captures
5. **Use descriptive filenames** for organization
6. **Consider file size** for full-page screenshots
7. **Capture at 2x resolution** for retina displays

## Resources

- agent-browser docs: https://github.com/ZenoWangzy/agent-browser
- Screenshot testing: https://www.percy.io/
- ImageMagick: https://imagemagick.org/
