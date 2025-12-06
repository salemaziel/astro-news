# Astro News - Developer & AI Agent Instructions

This document provides comprehensive onboarding information for AI agents and developers working with the Astro News codebase.

## Project Overview

Astro News is a modern news website built with [Astro](https://astro.build) v5.x, featuring content management via Keystatic CMS, responsive design with Tailwind CSS and DaisyUI, and full-text search with Pagefind.

**Live Demo:** https://astro-news-six.vercel.app/

## Technology Stack

| Technology | Version | Purpose |
|------------|---------|---------|
| [Astro](https://astro.build) | v5.13+ | Static site generation framework |
| [Bun](https://bun.sh) | v1.2+ | JavaScript runtime and package manager |
| [TypeScript](https://typescriptlang.org) | v5.9+ | Type-safe JavaScript |
| [Tailwind CSS](https://tailwindcss.com) | v4.1+ | Utility-first CSS framework |
| [DaisyUI](https://daisyui.com) | v5+ | Tailwind CSS component library |
| [Keystatic](https://keystatic.com) | v0.5+ | Headless CMS |
| [Pagefind](https://pagefind.app) | v1.3+ | Static search engine |
| [MDX](https://mdxjs.com) | - | Markdown with JSX support |

## Directory Structure

```
astro-news/
├── src/
│   ├── assets/              # Static assets (images, fonts)
│   ├── components/          # Reusable UI components
│   │   ├── bases/           # Base/atomic components (icons, dividers)
│   │   ├── cards/           # Card components for articles/authors
│   │   ├── elements/        # UI elements (navbar, menus)
│   │   └── shared/          # Shared components (header, footer)
│   ├── content/             # Content collections (articles, authors, etc.)
│   │   ├── articles/        # MDX article files
│   │   ├── authors/         # Author profiles
│   │   ├── categories/      # Category definitions
│   │   └── views/           # Page view configurations
│   ├── layouts/             # Page layouts
│   │   ├── base.astro       # Base HTML layout
│   │   ├── content.astro    # Article content layout
│   │   └── list.astro       # List page layout
│   ├── lib/                 # Shared utilities and configurations
│   │   ├── config/          # Site configuration (SITE, navigation links)
│   │   ├── handlers/        # Data handlers (articles, authors, categories)
│   │   ├── keystatic/       # Keystatic CMS collection definitions
│   │   ├── schema/          # Zod schemas for content validation
│   │   ├── types/           # TypeScript type definitions
│   │   └── utils/           # Utility functions (date, meta, remarks)
│   ├── pages/               # Astro pages (file-based routing)
│   │   ├── _home/           # Home page components
│   │   ├── articles/        # Article pages with dynamic routing
│   │   ├── authors/         # Author pages
│   │   ├── categories/      # Category pages
│   │   └── search/          # Search page with Pagefind
│   ├── styles/              # Global CSS styles
│   └── content.config.ts    # Content collection definitions
├── public/                  # Public static files
├── astro.config.mjs         # Astro configuration
├── keystatic.config.ts      # Keystatic CMS configuration
├── tsconfig.json            # TypeScript configuration
└── package.json             # Project dependencies and scripts
```

## Getting Started

### Prerequisites

- [Bun](https://bun.sh) v1.2+ installed
- Node.js 18+ (if not using Bun)

### Installation

```bash
# Clone the repository (update URL to your fork if applicable)
git clone https://github.com/salemaziel/astro-news.git
cd astro-news

# Install dependencies
bun install

# Start development server
bun dev
```

The development server runs at `http://localhost:4321`.

### Available Scripts

| Command | Description |
|---------|-------------|
| `bun dev` | Start development server |
| `bun build` | Build for production |
| `bun preview` | Preview production build |

## Content Management

### Content Collections

The project uses Astro's Content Layer API with four collections defined in `src/content.config.ts`:

1. **Articles** (`src/content/articles/`)
   - MDX files with frontmatter
   - Schema: `articleSchema` in `src/lib/schema/index.ts`
   - Fields: title, description, cover image, category, authors, publishedTime, isDraft, isMainHeadline, isSubHeadline

2. **Authors** (`src/content/authors/`)
   - MDX files with author bio
   - Schema: `authorSchema`
   - Fields: name, job, avatar, bio, social links

3. **Categories** (`src/content/categories/`)
   - JSON files
   - Schema: `categorySchema`
   - Fields: title, path (slug)

4. **Views** (`src/content/views/`)
   - MDX files for page metadata
   - Schema: `viewSchema`
   - Used for pages like home, about, search, etc.

### Creating New Content

#### New Article

Create a new folder in `src/content/articles/` with an `index.mdx` file:

```mdx
---
title: "Article Title"
description: "Article description (max 160 chars)"
cover: "@assets/images/articles/your-image.jpg"
category: technology
authors:
  - john-smith
publishedTime: 2025-01-01T00:00:00Z
isDraft: false
isMainHeadline: false
isSubHeadline: false
---

Article content goes here...
```

#### New Author

Create a new folder in `src/content/authors/` with an `index.mdx` file:

```mdx
---
name: "Author Name"
job: "Job Title"
avatar: "@assets/images/authors/avatar.jpg"
bio: "Author biography"
social:
  - name: "Twitter"
    url: "https://twitter.com/username"
    icon: "twitter"
---

Extended bio content...
```

#### New Category

Create a new folder in `src/content/categories/` with an `index.json` file:

```json
{
  "title": "Category Name",
  "path": "category-slug"
}
```

### Keystatic CMS (Optional)

For visual content management:

1. Copy `.env.example` to `.env`
2. Set `RUN_KEYSTATIC=true`
3. Run `bun dev`
4. Access CMS at `http://localhost:4321/keystatic`

Keystatic collection schemas are defined in `src/lib/keystatic/`.

## Key Patterns and Conventions

### Data Handlers

Located in `src/lib/handlers/`, these provide data access patterns:

- **`articlesHandler`**: Get all articles, main headline, sub-headlines
- **`authorsHandler`**: Get all authors, find author by ID
- **`categoriesHandler`**: Get all categories, categories with articles

Example usage:
```typescript
import { articlesHandler } from "@/lib/handlers/articles";

const articles = articlesHandler.allArticles();
const mainHeadline = articlesHandler.mainHeadline();
```

### Path Aliases

Configured in `tsconfig.json`:
- `@/*` → `src/*`
- `@assets/*` → `src/assets/*`

### Styling

- **Tailwind CSS v4** with Vite plugin
- **DaisyUI** for component classes
- **Global styles** in `src/styles/global.css`
- Theme support: light (default) and dark mode

### Layouts

1. **`base.astro`**: Root layout with HTML structure, head, header, footer
2. **`content.astro`**: Article content wrapper with prose styling
3. **`list.astro`**: List pages with pagination

### Remark Plugins

Custom plugins in `src/lib/utils/remarks.mjs`:
- **`readingTime`**: Calculates reading time for articles
- **`modifiedTime`**: Gets last modified date from git or filesystem

## Site Configuration

Site settings in `src/lib/config/index.ts`:

```typescript
export const SITE = {
  title: "Astro News",
  description: "A news website built with Astro",
  author: "Mohammad Rahmani",
  url: "https://astro-news-six.vercel.app",
  locale: "en-US",
  dir: "ltr",
  basePath: "/",
  postsPerPage: 4,
};
```

Navigation links, social links, and other link configurations are also exported from this file.

## Features

### Search

Full-text search powered by Pagefind:
- Automatically indexes content at build time
- Search page at `/search`
- Supports URL query parameters (`?q=searchterm`)

### RSS Feed

RSS feed available at `/rss.xml` (generated by `src/pages/rss.xml.js`)

### Sitemap

Automatically generated sitemap at `/sitemap-index.xml` via `@astrojs/sitemap`

### Dark Mode

DaisyUI theme switcher with `light` and `dark` themes, respecting system preferences.

## Common Tasks

### Adding a New Page

1. Create `.astro` file in `src/pages/`
2. Use appropriate layout (`BaseLayout`, `ListLayout`)
3. Get view metadata from `views` collection

### Adding a New Component

1. Create `.astro` file in appropriate `src/components/` subdirectory
2. Use Tailwind/DaisyUI classes for styling
3. Import with path alias: `import Component from "@/components/..."`

### Modifying the Navigation

Edit `NAVIGATION_LINKS` in `src/lib/config/index.ts`

### Updating Site Metadata

Edit `SITE` object in `src/lib/config/index.ts`

## Type Definitions

Key types in `src/lib/types/index.ts`:
- `Icon`: Props for icon components
- `Link`: Navigation link structure
- `Meta`: Page metadata
- `ArticleMeta`: Extended metadata for articles
- `Entry`: Content collection entry type

## Debugging Tips

1. **Content errors**: Check schema validation in `src/lib/schema/index.ts`
2. **Build errors**: Verify content frontmatter matches schema
3. **Missing content**: Ensure handler filters allow the content (e.g., `isDraft: false`)
4. **Keystatic issues**: Verify `RUN_KEYSTATIC=true` in `.env`

## Deployment

The project is configured for Vercel deployment:
1. Push to main branch
2. Vercel automatically builds and deploys

For other platforms, run `bun build` and deploy the `dist/` directory.

## Additional Resources

- [Astro Documentation](https://docs.astro.build)
- [Keystatic Documentation](https://keystatic.com/docs)
- [Tailwind CSS Documentation](https://tailwindcss.com/docs)
- [DaisyUI Documentation](https://daisyui.com/components)
- [Project GitHub Repository](https://github.com/salemaziel/astro-news) (update to your fork if applicable)
