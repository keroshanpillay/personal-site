# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal website repository with two distinct components:

1. **Root-level static site** (`index.html` and other `.html` files in root): Personal portfolio site deployed via Vercel with Edge Middleware
2. **React app** (`psite/` subdirectory): Create React App bootstrapped application (appears to be unused/archived)

The main site is a simple, static HTML portfolio showcasing articles, projects, and contact information. It uses new.css for styling and Vercel Edge Functions for security headers.

## Repository Structure

```
.
├── index.html              # Main portfolio page (primary site)
├── middleware.js           # Vercel Edge Middleware for security headers
├── package.json            # Root dependencies (@vercel/edge, react-markdown)
├── *.html                  # Individual article pages (DefiLoyalty, AmmPathDependency, etc.)
└── psite/                  # Create React App (separate, likely inactive)
    ├── src/
    ├── public/
    └── package.json        # React app dependencies
```

## Development Commands

### Main Site (Root)
The root site has no build process - it's pure static HTML deployed to Vercel.

- **Deploy**: Push to git (Vercel auto-deploys)
- **Local preview**: Open `index.html` directly in browser or use a local server

### React App (psite/ directory)
If working with the React app subdirectory:

```bash
cd psite
npm start          # Development server on http://localhost:3000
npm test           # Run tests in watch mode
npm run build      # Production build to psite/build/
```

## Key Architecture

### Edge Middleware (`middleware.js`)
- Runs on Vercel Edge Network before cache
- Adds security headers to all responses:
  - `Referrer-Policy: origin-when-cross-origin`
  - `X-Frame-Options: DENY`
  - `X-Content-Type-Options: nosniff`
  - `X-DNS-Prefetch-Control: on`
  - `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload`

### Content Structure
- `index.html`: Main landing page with bio, projects, articles, and contact
- Individual HTML files for articles/essays (e.g., `DefiLoyalty.html`, `AmmPathDependency.html`)
- All content uses new.css framework for minimal styling
- Vercel Analytics integrated via `/_vercel/insights/script.js`

## Deployment

- **Platform**: Vercel
- **Trigger**: Automatic on git push to main/master branch
- **Primary site**: Root HTML files
- **Edge Functions**: `middleware.js` runs globally on Vercel Edge Network

## SEO Optimization

The site is fully optimized for search engines with structured metadata, Open Graph tags, and JSON-LD structured data. **When adding new articles, you MUST follow this template to maintain SEO standards.**

### Template for New Article Pages

Every new article HTML file must include the following in the `<head>` section:

```html
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <script defer src="/_vercel/insights/script.js"></script>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@exampledev/new.css@1.1.2/new.min.css" />

  <!-- REQUIRED: Unique title (50-60 chars) -->
  <title>Article Title Here | Keroshan Pillay</title>

  <!-- REQUIRED: Unique description (150-160 chars) -->
  <meta name="description" content="Compelling description summarizing the article's main points and value." />

  <meta name="author" content="Keroshan Pillay" />
  <meta name="keywords" content="relevant, keywords, separated, by, commas" />
  <link rel="canonical" href="https://keroshanpillay.com/ArticleFileName.html" />

  <!-- Open Graph / Facebook -->
  <meta property="og:type" content="article" />
  <meta property="og:url" content="https://keroshanpillay.com/ArticleFileName.html" />
  <meta property="og:title" content="Article Title for Social Media" />
  <meta property="og:description" content="Description for social media sharing (can be different from meta description)." />
  <meta property="og:site_name" content="Keroshan Pillay" />
  <meta property="article:published_time" content="YYYY-MM-DDT00:00:00Z" />
  <meta property="article:author" content="Keroshan Pillay" />
  <meta property="article:section" content="CategoryName" />
  <meta property="article:tag" content="Tag1" />
  <meta property="article:tag" content="Tag2" />

  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image" />
  <meta name="twitter:url" content="https://keroshanpillay.com/ArticleFileName.html" />
  <meta name="twitter:title" content="Article Title for Twitter" />
  <meta name="twitter:description" content="Description for Twitter (max 200 chars)." />
  <meta name="twitter:creator" content="@keropillay" />
  <meta name="twitter:site" content="@keropillay" />

  <!-- JSON-LD Structured Data -->
  <script type="application/ld+json">
  {
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "Full Article Headline",
    "author": {
      "@type": "Person",
      "name": "Keroshan Pillay",
      "url": "https://keroshanpillay.com"
    },
    "datePublished": "YYYY-MM-DD",
    "dateModified": "YYYY-MM-DD",
    "description": "Article description for search engines.",
    "keywords": "keyword1, keyword2, keyword3",
    "publisher": {
      "@type": "Person",
      "name": "Keroshan Pillay"
    },
    "mainEntityOfPage": {
      "@type": "WebPage",
      "@id": "https://keroshanpillay.com/ArticleFileName.html"
    }
  }
  </script>
</head>
```

### Checklist When Adding New Articles

1. **Create the HTML file** with proper SEO metadata using the template above
2. **Update `index.html`**: Add the article to the "Thoughts" table with date and link
3. **Update `sitemap.xml`**: Add a new `<url>` entry:
   ```xml
   <url>
     <loc>https://keroshanpillay.com/NewArticle.html</loc>
     <lastmod>YYYY-MM-DD</lastmod>
     <changefreq>yearly</changefreq>
     <priority>0.8</priority>
   </url>
   ```
4. **Verify metadata**: Check that title, description, keywords, and dates are unique and accurate

### SEO Best Practices

- **Titles**: 50-60 characters, include main keyword and "| Keroshan Pillay"
- **Descriptions**: 150-160 characters, compelling and descriptive
- **Keywords**: 5-10 relevant keywords separated by commas
- **Dates**: Use ISO 8601 format (YYYY-MM-DD or YYYY-MM-DDT00:00:00Z)
- **Canonical URLs**: Always use full absolute URLs with https://keroshanpillay.com
- **Article sections**: Use categories like "DeFi", "Philosophy", "Fintech", "Crypto Culture"

### Files to Keep Updated

- `sitemap.xml` - Add all new pages here
- `index.html` - Update the "Thoughts" table with new articles
- `robots.txt` - No changes needed (already allows all crawlers)
