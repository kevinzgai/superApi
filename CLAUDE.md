# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **Strapi 5** CMS application (`@strapi/strapi` v5.34.0) with SQLite database by default. It uses `better-sqlite3` for local development and pnpm for package management.

## Commands

```bash
pnpm develop    # Start Strapi in development mode with hot reload
pnpm start      # Start Strapi in production mode
pnpm build      # Build the admin panel
pnpm seed:example # Run the seed script to populate initial content
pnpm strapi     # Access Strapi CLI (e.g., strapi generate, strapi install)
```

## Architecture

### Content Types

Located in `src/api/` with each content type having:
- `content-types/<name>/schema.json` - Data model definition
- `controllers/<name>.js` - Request handlers
- `services/<name>.js` - Business logic
- `routes/<name>.js` - Route configuration

**Available content types:**
- `article` - Blog posts with dynamic zone blocks (media, quote, rich-text, slider)
- `author` - Blog authors
- `category` - Blog categories
- `global` - Site-wide settings with SEO component
- `about` - About page content

### Reusable Components

Located in `src/components/shared/`:
- `media` - Single file/media component
- `slider` - Multiple image slider
- `quote` - Quote block
- `rich-text` - Rich text content
- `seo` - SEO meta tags (metaTitle, metaDescription, shareImage)

### Bootstrap & Seeding

`src/bootstrap.js` runs on startup and seeds the database from:
- `data/data.json` - Content data (articles, authors, categories, global, about)
- `data/uploads/` - Media files

The seed only runs once (tracked via Strapi's plugin store). To re-seed, clear the database first.

### Configuration

- `config/database.js` - Database connections (SQLite default, supports MySQL/PostgreSQL)
- `config/server.js` - Server host/port settings
- `config/plugins.js` - Plugin configuration
- `config/middlewares.js` - Middleware stack
- `config/api.js` - API defaults
- `config/admin.js` - Admin panel configuration

### Admin Customization

- `src/admin/vite.config.example.js` - Vite config for admin panel with `@` alias to `/src`

### API Structure

Strapi's REST API follows this pattern:
```
GET  /api/articles          # List articles
GET  /api/articles/:id      # Get single article
POST /api/articles          # Create article
PUT  /api/articles/:id      # Update article
DELETE /api/articles/:id    # Delete article
```

Public access is configured in bootstrap for find/findOne on all content types.

## Database

- Default: SQLite at `.tmp/data.db`
- Supports MySQL and PostgreSQL via environment variables
- Uses `better-sqlite3` driver (native module, may require rebuild on node version changes)
