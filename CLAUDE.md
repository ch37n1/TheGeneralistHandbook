# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is "The Generalist Handbook" - a digital garden/knowledge base built with Quartz v4 (a static site generator for publishing Obsidian notes as a website). The handbook covers:
- Computer Science (frontend, backend, devops)
- Data Science (ML, DS, analytics)
- Marketing (PMF, communications)
- Product Management
- Tools and development practices
- AI-Driven Development (AID)

## Architecture

### Core Structure

Quartz follows a plugin-based architecture with three main plugin types:

1. **Transformers** (`quartz/plugins/transformers/`) - Process markdown and HTML:
   - Parse markdown into AST using remark/rehype
   - Handle frontmatter, links, syntax highlighting, LaTeX, etc.
   - Configured in `quartz.config.ts` under `plugins.transformers`

2. **Filters** (`quartz/plugins/filters/`) - Filter content before emission:
   - Remove drafts, handle private pages
   - Configured in `quartz.config.ts` under `plugins.filters`

3. **Emitters** (`quartz/plugins/emitters/`) - Generate output files:
   - ContentPage, FolderPage, TagPage for HTML pages
   - Assets, Static for resources
   - ContentIndex for search, sitemap, RSS
   - Configured in `quartz.config.ts` under `plugins.emitters`

### Key Configuration Files

- `quartz.config.ts` - Main configuration (site title, theme, plugins, analytics)
- `quartz.layout.ts` - Page layout components (header, footer, sidebar content)
- `content/` - All markdown content (main content directory)
- `content/index.md` - Homepage

### Build Process

1. Parse command-line args via `quartz/bootstrap-cli.mjs`
2. Transpile TypeScript to JavaScript using esbuild
3. Recursively glob all markdown files in `content/`
4. Parse markdown files (optionally using worker threads for >128 files)
5. Apply transformers → filters → emitters
6. Generate static HTML + assets in `public/`

## Common Development Commands

### Building and Testing

```bash
# Start local development server with hot-reload
bun run quartz build --serve

# Using Makefile shortcuts
make r    # Run local server (same as bun run quartz build --serve)
make p    # Sync to GitHub (same as bun run quartz sync)

# Build for production (output to public/)
bun run quartz build

# Sync content to GitHub
bun run quartz sync
```

### Code Quality

```bash
# Type check and format check
bun run check

# Format code with Prettier
bun run format

# Run tests
bun test
```

### Build Options

```bash
# Serve with custom directory
bun run quartz build --serve -d content

# Serve on custom port
bun run quartz build --serve --port 8080

# Build with verbose logging
bun run quartz build -v

# Custom output directory
bun run quartz build -o public

# Control concurrency
bun run quartz build --concurrency=4
```

## Content Management

### File Organization

- **Content root**: `/content` directory
- **Homepage**: `content/index.md`
- **Checklists**: `content/checklists/` - Design doc checklists (SWE, ML)
- **Guidelines**: `content/guidelines/` - Best practices and guidelines
- **Theory**: `content/theory/` - Theoretical content
- **Ignored patterns**: `private/`, `templates/`, `.obsidian/` (configured in quartz.config.ts)

### Frontmatter

All markdown files support frontmatter for metadata:

```yaml
---
title: Page Title
description: Page description for SEO
tags: [tag1, tag2]
draft: false
date: 2026-02-10
aliases: [alias1, alias2]
permalink: /custom-url
---
```

### Markdown Features

- **Wikilinks**: `[[page name]]` or `[[page name|display text]]`
- **Obsidian callouts**: `> [!note]`, `> [!warning]`, etc.
- **GitHub Flavored Markdown**: Tables, task lists, strikethrough, footnotes
- **LaTeX math**: Inline `$math$` and block `$$math$$` using KaTeX
- **Mermaid diagrams**: Code blocks with `mermaid` language

## Component System

### Layout Components (`quartz/components/`)

Components are defined in TypeScript and rendered server-side using Preact. Key components:

- **PageTitle** - Site title in header
- **Search** - Full-text search
- **Explorer** - File tree navigation
- **Graph** - Interactive link graph
- **TableOfContents** - Page TOC
- **Backlinks** - Pages linking to current page
- **Darkmode** - Theme toggle
- **ReaderMode** - Distraction-free reading

Layout is configured in `quartz.layout.ts` with three areas:
- `sharedPageComponents` - Applied to all pages (head, header, footer)
- `defaultContentPageLayout` - Single page layout (beforeBody, left, right)
- `defaultListPageLayout` - List pages (tags, folders)

### Creating New Components

Components must:
1. Export a default function that returns a `QuartzComponent`
2. Optionally export `css` string and `beforeDOMLoaded`/`afterDOMLoaded` scripts
3. Be registered in `quartz/components/index.ts`

## Plugin Development

### Creating Plugins

Plugins follow the pattern:

```typescript
export const PluginName: QuartzTransformerPlugin = (userOpts) => {
  const opts = { ...defaultOptions, ...userOpts }
  return {
    name: "PluginName",
    textTransform(ctx, src) { /* optional */ },
    markdownPlugins(ctx) { /* optional */ },
    htmlPlugins(ctx) { /* optional */ },
    externalResources(ctx) { /* optional */ }
  }
}
```

### Plugin Registration

1. Add plugin file to appropriate directory (`transformers/`, `filters/`, `emitters/`)
2. Export from `quartz/plugins/index.ts`
3. Add to `quartz.config.ts` in relevant plugin array

## Important Notes

- **Builds are cached**: `.quartz-cache/` directory stores transpiled code
- **Hot reload**: Changes to `.ts`, `.tsx`, `.scss` files trigger rebuild
- **Content changes**: Only `.md` files trigger content rebuild in serve mode
- **Worker threads**: Used automatically for >128 files for faster parsing
- **SPA mode enabled**: Client-side navigation with `micromorph` for smooth transitions
- **Analytics**: Configured for Plausible in `quartz.config.ts`

## Customization Points

When making changes, commonly modified files are:

1. **Site metadata**: `quartz.config.ts` - pageTitle, baseUrl, theme colors
2. **Layout**: `quartz.layout.ts` - component arrangement
3. **Styling**: `quartz/styles/` - Custom SCSS styles
4. **Components**: `quartz/components/` - UI components
5. **Plugins**: `quartz/plugins/` - Content transformation logic

## Dependencies

- **Bun**: >=1.0.0 required (JavaScript runtime and package manager)
- **Runtime**: TypeScript compiled to ESM modules
- **Key libraries**: esbuild, Preact, remark/rehype, d3 (graph), flexsearch (search)

### Installation

```bash
# Install dependencies
bun install

# Or if you need to clean install
rm -rf node_modules bun.lockb
bun install
```
