# Project Instructions for Claude

This is an Astro static site generated from WordPress content by PhantomWP.

## Core Rules

- This is an **Astro** project, NOT React. Use `class=` (not `className=`), frontmatter between `---` markers, no React hooks.
- API endpoints: Use `.ts` files (e.g., `src/pages/api/submit.ts`) with `export const prerender = false;` at the top.
- WordPress content: Use `set:html={post.content.rendered}` for HTML content.
- Images: Import local images; remote images need explicit `width` and `height` attributes.
- **Tailwind V4 only** (not V3) - never use `<style>` tags or inline CSS. Prefer theme tokens (`bg-surface`, `text-content`, `bg-primary`).
- Flag security issues. Make minimal diffs.
- Always use `cursor: pointer` (or Tailwind `cursor-pointer`) on clickable elements.

## WordPress Functions (src/lib/wordpress.ts)

```typescript
getPosts()                     // Get all posts
getPost(slug)                  // Get single post by slug
getPages()                     // Get all pages
getPage(slug)                  // Get single page by slug
getFeaturedImageUrl(post, size) // Get featured image URL (sizes: thumbnail, medium, large, full)
formatDate(date)               // Format a date string
makeCMSRequest(route, body?, options?) // Custom WordPress REST API request
submitFluentForm(formId, fields)       // Submit a Fluent Form
createComment(postId, content, {name, email}) // Create a comment
```

## Navigation (src/lib/navigation.ts)

```typescript
getNavigationPages()           // Get pages for navigation
getMenuItems()                 // Get menu items (from Menu Builder)
isActivePath(path, target, exact?) // Check if path is active
getBreadcrumbs(pathname)       // Get breadcrumb trail
```

## Menus (src/components/menus/)

Created via the Menu Builder UI, saved as components (`<MainMenu/>`, `<FooterMenu/>`). Config lives in `src/config/menus.json`.

## Fonts

- **FIRST** check `package.json` for `@fontsource/*` packages. If the font exists, just update `theme.css` (no install needed).
- To install new: `npm install @fontsource/{id}` then import in `BaseLayout.astro`.
- To use: update `--font-sans` in `src/styles/theme.css`.
- Both `@fontsource/inter` and `@fontsource-variable/inter` use `font-family: 'Inter'`.

## Icons (lucide-astro)

```astro
---
import { ArrowRight, Mail, Phone } from 'lucide-astro';
---
<ArrowRight class="w-5 h-5 text-primary" />
```

## Tailwind V4 Theme (src/styles/theme.css)

Theme tokens are defined in `@theme { }` blocks:
- Colors: `--color-primary`, `--color-surface`, `--color-content`, `--color-outline`, etc.
- Fonts: `--font-sans`, `--font-mono`, `--font-heading`
- Usage: `bg-primary`, `text-content`, `border-outline`, `bg-surface-alt`

## Astro Patterns

- Frontmatter goes between `---` markers at the top of `.astro` files.
- Use `class=` NOT `className=`.
- Dynamic routes use `[...slug].astro` with `getStaticPaths()`.
- Components use `<slot />` for children (not `{children}`).
- TypeScript in frontmatter - always define proper types/interfaces.

## Layouts

**NEVER use BaseLayout directly in pages.** BaseLayout is the low-level shell (head, scripts, styles) and does NOT include Header, Footer, or any page structure.

Always use one of these higher-level layouts:

- **PageLayout** (`src/layouts/PageLayout.astro`) - For pages with custom components, cards, grids, etc. No prose styling. Use as `layout: '@layouts/PageLayout.astro'` in MDX frontmatter.
- **ContentLayout** (`src/layouts/ContentLayout.astro`) - For text-heavy MDX pages (articles, docs, about pages). Includes `prose` typography styling. Use as `layout: '@layouts/ContentLayout.astro'` in MDX frontmatter.

Example MDX page:
```mdx
---
layout: '@layouts/PageLayout.astro'
title: My Page
description: A description of my page
---

# Hello World

Page content here.
```

For `.astro` pages, import and wrap with the layout component:
```astro
---
import PageLayout from '../layouts/PageLayout.astro';
---
<PageLayout frontmatter={{ title: 'My Page', description: 'A description' }}>
  <h1>Hello World</h1>
</PageLayout>
```

## Project Structure

- `src/pages/` - Astro pages and routes
- `src/components/` - Reusable Astro components
- `src/layouts/` - Layout components (PageLayout, ContentLayout, BaseLayout)
- `src/lib/` - Utilities (wordpress.ts, navigation.ts)
- `src/styles/` - Theme and global styles (theme.css, global.css)
- `src/config/` - Configuration files (menus.json)
- `docs/ai-instructions.md` - Custom user instructions (read this first)
- `docs/notes.md` - Project notes scratchpad

## IDE Integration (phantomwp-ide CLI)

You have a CLI tool to control the PhantomWP web IDE. **Use it proactively:**

```bash
phantomwp-ide open <file> [line]        # Open a file in the editor
phantomwp-ide close <file>              # Close a file tab
phantomwp-ide tabs                      # List currently open tabs
phantomwp-ide preview <path>            # Navigate the live preview to a URL
phantomwp-ide refresh                   # Refresh the live preview
phantomwp-ide refresh-files [folder]    # Refresh file tree (or a specific folder)
phantomwp-ide notify "<message>" [type] # Show a notification (info|success|error)
```

**Rules:**
- After editing a file, run `phantomwp-ide open <file>` so the user sees the changes.
- After creating or deleting files, run `phantomwp-ide refresh-files` (or `phantomwp-ide refresh-files <folder>` for a specific folder) so the file tree updates and new components appear in autocomplete.
- After renaming or deleting a file, run `phantomwp-ide close <old-path>` to remove the stale tab, then `phantomwp-ide refresh-files <parent-folder>` to update the sidebar.
- After visual changes, run `phantomwp-ide refresh` to update the preview.
- When asked to navigate to a page, run `phantomwp-ide preview /path`.
- Run `phantomwp-ide notify "Done" success` after completing multi-step tasks.
- Use `phantomwp-ide tabs` to check which files the user currently has open before making changes.
- Batch your IDE commands: after multiple edits, open the most relevant file last so it stays active.
- If a command fails (prints "Failed:"), it usually means the IDE is not connected yet. Wait a moment and retry once.
