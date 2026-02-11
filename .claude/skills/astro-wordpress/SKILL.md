---
name: astro-wordpress
description: Expert guide for building Astro static sites powered by WordPress. Use when editing Astro components, pages, layouts, WordPress integration, Tailwind styling, fonts, icons, SEO, or navigation. Covers all project conventions and available APIs.
---

# Astro WordPress Development

## Rules

- Astro uses `class=` (not `className=`), frontmatter between `---` markers, no React hooks
- API endpoints: Use `.ts` files (e.g., `src/pages/api/submit.ts`) with `export const prerender = false;` at the top
- WordPress content: `set:html={post.content.rendered}`
- Images: import local images; remote images need explicit width/height
- **Tailwind V4 only** (not V3) - never use `<style>` tags or inline CSS; prefer theme tokens (`bg-surface`, `text-content`, `bg-primary`)
- Clickable elements must have `cursor-pointer`
- Flag security issues; make minimal diffs
- Frontmatter uses TypeScript - always define proper types/interfaces

## WordPress Functions (src/lib/wordpress.ts)

```typescript
getPosts()                              // Get all posts
getPost(slug)                           // Get single post by slug
getPages()                              // Get all pages
getPage(slug)                           // Get single page by slug
getFeaturedImageUrl(post, size)         // sizes: thumbnail, medium, large, full
formatDate(date)                        // Format a date string
makeCMSRequest(route, body?, options?)  // Custom WordPress REST API request
submitFluentForm(formId, fields)        // Submit a Fluent Form
createComment(postId, content, {name, email})
```

## Navigation (src/lib/navigation.ts)

```typescript
getNavigationPages()                    // Get pages for nav
getMenuItems()                          // Menu items from Menu Builder
isActivePath(path, target, exact?)      // Check active state
getBreadcrumbs(pathname)                // Breadcrumb trail
```

## Menus (src/components/menus/)

Created via Menu Builder UI, saved as components (`<MainMenu/>`, `<FooterMenu/>`). Config in `src/config/menus.json`.

## Fonts

- **FIRST** check `package.json` for `@fontsource/*` - if font exists, just update theme.css (no install needed!)
- To install new: `npm install @fontsource/{id}` then import in `BaseLayout.astro`
- To use: update `--font-sans` in `src/styles/theme.css`
- `@fontsource/inter` and `@fontsource-variable/inter` both use font-family: `'Inter'`

For detailed font guide, see [fonts.md](fonts.md)

## Icons (lucide-astro)

```astro
---
import { ArrowRight, Mail, Phone } from 'lucide-astro';
---
<ArrowRight class="w-5 h-5 text-primary" />
```

Common icons: ArrowRight, ArrowLeft, ChevronDown, Menu, X, Plus, Edit, Trash, Mail, Phone, Search, Settings, User, Home, Star, Check, AlertCircle

For full icon guide, see [icons.md](icons.md)

## Tailwind V4 Theme (src/styles/theme.css)

Tokens in `@theme { }` blocks create utility classes:
- `--color-primary` -> `bg-primary`, `text-primary`, `border-primary`
- `--color-surface` -> `bg-surface` (page background)
- `--color-surface-alt` -> `bg-surface-alt` (cards)
- `--color-content` -> `text-content` (main text)
- `--color-content-light` -> `text-content-light` (secondary text)
- `--color-outline` -> `border-outline` (borders)
- `--font-sans`, `--font-mono`, `--font-heading`

For detailed theming guide, see [tailwind.md](tailwind.md)

## Layouts

**NEVER use BaseLayout directly in pages.** BaseLayout is the low-level shell (head, scripts, styles) -- it does NOT include Header, Footer, or page structure.

Always use one of these:

- **PageLayout** (`src/layouts/PageLayout.astro`) - For pages with custom components, grids, cards. No prose styling. Use as `layout: '@layouts/PageLayout.astro'` in MDX.
- **ContentLayout** (`src/layouts/ContentLayout.astro`) - For text-heavy MDX pages (articles, docs). Includes `prose` typography. Use as `layout: '@layouts/ContentLayout.astro'` in MDX.

MDX frontmatter example: `layout: '@layouts/PageLayout.astro'`

For .astro pages, import and wrap: `import PageLayout from '../layouts/PageLayout.astro';` then use `<PageLayout frontmatter={{ title: 'My Page' }}><slot content /></PageLayout>`

## Astro Component Patterns

```astro
---
interface Props {
    title: string;
    description?: string;
}
const { title, description = '' } = Astro.props;
---

<div class="bg-surface-alt rounded-lg p-6 border border-outline">
    <h3 class="text-lg font-semibold text-content">{title}</h3>
    {description && <p class="text-content-light mt-2">{description}</p>}
    <slot />
</div>
```

- Use `<slot />` for children (not `{children}`)
- Use `class:list={[...]}` for conditional classes
- Dynamic routes: `[...slug].astro` with `getStaticPaths()`

For more patterns, see [components.md](components.md)

## SEO

For SEO and meta tags guide, see [seo.md](seo.md)

## WordPress Integration

For detailed WordPress API guide with examples, see [wordpress.md](wordpress.md)

## Project Structure

- `src/pages/` - Astro pages and routes
- `src/components/` - Reusable Astro components
- `src/layouts/` - Layout components (PageLayout, ContentLayout, BaseLayout)
- `src/lib/` - Utilities (wordpress.ts, navigation.ts)
- `src/styles/` - Theme and global styles (theme.css, global.css)
- `src/config/` - Configuration files (menus.json)
- `docs/ai-instructions.md` - Custom user instructions (read this)
- `docs/notes.md` - Project notes scratchpad
