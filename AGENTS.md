# AGENTS.md - Zabbix HTML Templates

## Project Overview

This repository contains Zabbix HTML email notification templates. These are modern, mobile-aware HTML templates used for email alerts/notifications in Zabbix monitoring system.

## File Structure

```
zabbix-html-templates/
├── problem.html          # Problem alert template
├── problem 2.html        # Alternative problem template
├── problem recovery.html # Recovery alert template
├── problem recovery 2.html # Alternative recovery template
├── .agents/skills/
│   └── playwright-cli/   # Browser automation skill
└── .gitignore
```

## Build / Lint / Test Commands

### No Build System
This is a simple HTML/CSS template repository - there is **no build system** (no npm, make, or other build tools).

### Testing Templates

#### Manual Browser Preview
```bash
xdg-open preview.html    # Linux
open preview.html        # macOS
start preview.html       # Windows
```

#### Automated Browser Testing (playwright-cli)
Use the available playwright-cli skill for browser automation:

```bash
# Open template in browser for visual testing
playwright-cli open preview.html

# Take snapshot to verify rendering
playwright-cli snapshot

# Test with different viewport sizes
playwright-cli resize 480 800   # Mobile
playwright-cli resize 768 1024  # Tablet
playwright-cli resize 1920 1080 # Desktop

# Close browser
playwright-cli close
```

#### Running a Single Test
Since there are no automated tests, single template testing is done manually via browser or playwright-cli. To add tests in the future, consider:
- Visual regression testing with Playwright
- HTML validation with html-validate or similar
- Email client compatibility testing with Litmus/Email on Acid

### Linting
No automated linting is configured. For HTML validation:
- Use browser developer tools
- W3C HTML Validator: https://validator.w3.org/

## Code Style Guidelines

### General Principles
- Keep templates compatible with major email clients (Outlook, Gmail, Apple Mail)
- Use table-based layouts for email client compatibility
- Inline all CSS (email clients often strip `<style>` blocks in `<head>`)
- Test across multiple email clients and devices

### HTML Structure
```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Zabbix Alert</title>
  <!-- Inline styles preferred -->
</head>
<body>...</body>
</html>
```

### CSS Guidelines
- Use inline `style="..."` attributes on elements
- Use tables for layout (div support is inconsistent in email clients)
- Specify fallback fonts: `font-family: Verdana, "DejaVu Sans", "Helvetica Neue", Helvetica, Arial, sans-serif`
- Use hex color codes
- Include media queries for mobile responsiveness
- Support dark mode with `@media (prefers-color-scheme: dark)`

### Naming Conventions
- Files: lowercase with spaces (e.g., `problem recovery.html`)
- Classes: lowercase with hyphens (e.g., `.hero-section`)
- IDs: lowercase with hyphens (if used)

### Email-Specific Requirements
- Use absolute URLs for images (host externally)
- Set explicit width on images
- Use `alt` text for all images
- Include preheader text (hidden div with preview text)
- Keep main content width around 600px
- Test with Litmus or similar email testing service

### Accessibility
- Use semantic HTML where possible
- Include alt attributes on images
- Ensure sufficient color contrast
- Use readable font sizes (minimum 14px for body text)

### Mobile Layout
Templates use a side-by-side layout on mobile with narrower columns:

- Label column: 110px
- Value column: 1fr
- Gap: 14px
- Font sizes: 12px labels, 14px values

### Preheader Text
Include hidden preheader text for email client preview:
```html
<div style="display:none; max-height:0; overflow:hidden; mso-hide:all;">
    Problem: {HOST.HOST} - {TRIGGER.NAME} - {TRIGGER.SEVERITY}
</div>
```

## Common Tasks

### Adding a New Template
1. Copy an existing template (e.g., `problem.html`)
2. Rename appropriately using lowercase with spaces
3. Modify the content while maintaining table-based structure
4. Test in browser and with playwright-cli
5. Update README.md if needed

### Modifying Existing Templates
1. Make changes to HTML/CSS
2. Test in browser: `playwright-cli open <filename>`
3. Verify responsive behavior with different viewport sizes
4. Check dark mode rendering

### Testing Responsive Design
```bash
playwright-cli open "problem 2.html"
playwright-cli resize 320 568    # Small mobile
playwright-cli resize 375 667    # iPhone
playwright-cli resize 768 1024   # Tablet
playwright-cli resize 1440 900   # Desktop
```

## Skills Available

### playwright-cli
Located at `.agents/skills/playwright-cli/SKILL.md`. Use this for:
- Opening HTML files in browser
- Taking snapshots of rendered templates
- Testing different viewport sizes
- Verifying dark mode support

## Notes for Agents

- This is NOT a JavaScript/TypeScript project - do not add package.json or npm scripts
- Do NOT add TypeScript, React, or other frontend frameworks
- Work only with plain HTML and inline CSS
- Email clients have limited CSS support - always test templates
- When in doubt about email compatibility, check Can I Email (https://www.caniemail.com/)
