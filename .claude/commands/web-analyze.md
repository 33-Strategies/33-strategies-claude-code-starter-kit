---
description: Analyze live web page content with custom prompts using WebFetch
allowed-tools: WebFetch
argument-hint: "<url> [optional: specific prompt for what to extract/analyze]"
category: workflow
---

# Web Page Analysis with WebFetch

**User Request:** $ARGUMENTS

---

## Instructions

Parse the user's arguments to extract:
1. **URL** - The web page to analyze (required)
2. **Prompt** - What specific information to extract or analyze (optional)

If no specific prompt is provided, use a default comprehensive analysis prompt.

### How WebFetch Works

WebFetch is a real-time web content analyzer that:
- Fetches **live** content from the URL (not cached snapshots, except for 15-min performance cache)
- Converts HTML to markdown format for easier analysis
- Processes content with an AI model based on your prompt
- Returns structured analysis/response

### Default Analysis Prompt (when user doesn't specify)

If the user only provides a URL without a specific prompt, use this comprehensive analysis:

```
Analyze this web page and provide:
1. Page purpose and main content type
2. Key data structures (JSON schemas, HTML patterns, data tables)
3. Navigation structure and links
4. Forms, inputs, or interactive elements
5. Embedded structured data (JSON-LD, microdata, etc.)
6. Recommended extraction strategy for scraping this page
```

### Custom Analysis Prompt (when user specifies)

If the user provides a specific prompt like:
- "Extract all product pricing data"
- "Show me the HTML structure of the navigation menu"
- "Find all JSON-LD schemas on the page"

Use their exact prompt as the WebFetch prompt.

---

## Execution

Use the WebFetch tool with:
- **URL:** {extracted from $ARGUMENTS}
- **Prompt:** {custom prompt if provided, otherwise default comprehensive analysis}

After receiving results, present them to the user in a clear, organized format with:
1. **Page URL:** {url}
2. **Analysis Focus:** {what was requested}
3. **Findings:** {WebFetch results}
4. **Recommendations:** {if applicable, suggest next steps like scraping strategies, data extraction patterns, etc.}

---

## Example Usage

```bash
# Comprehensive analysis (no specific prompt)
/web-analyze https://example.com/products

# Specific extraction prompt
/web-analyze https://slamgoods.com/collections/graded-magazines Show me the HTML structure of the product grid

# Data structure discovery
/web-analyze https://shopify-store.com/product/123 Extract the JSON-LD schema and pricing data

# Navigation analysis
/web-analyze https://news-site.com Analyze the navigation menu structure and main content sections
```

---

## Notes

- WebFetch shows **real-time** website structure (current live state)
- Results are cached for 15 minutes for performance (repeated calls to same URL within 15 min use cache)
- Particularly useful for:
  - Pre-scraper development (understanding page structure)
  - API discovery (finding embedded JSON data)
  - Competitive analysis (analyzing competitor site structure)
  - Data validation (verifying expected page structure)
