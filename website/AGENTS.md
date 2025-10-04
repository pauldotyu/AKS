# AKS Engineering Blog - Website Module Guide

> **Scope**: Module-specific guidance for the `website/` directory.
> **For Blog Posts**: See `.github/instructions/website.blog.instructions.md`
> **For Repo Standards**: See `.github/copilot-instructions.md`

## Quick Start

```bash
cd website
npm install        # Install dependencies
npm start          # Dev server (http://localhost:3000)
npm run build      # Production build
npm run typecheck  # TypeScript validation
```

---

## Architecture Overview

### Technology Stack

- **Framework**: Docusaurus 3.8.1+ (blog-only mode, docs disabled)
- **React**: 19.0.0
- **TypeScript**: 5.6.2
- **Node.js**: >= 18.0
- **Deployment**: Static site generation

### Key Design Decisions

1. **Blog-first**: Root path `/` serves blog listing
2. **Purple branding**: Custom theme (`#9f62eb`)
3. **Privacy-first**: GDPR cookie consent, analytics opt-in
4. **SEO-optimized**: RSS/Atom feeds, meta descriptions, social cards
5. **Type-safe**: Full TypeScript coverage

---

## Directory Structure

```text
website/
├── blog/                          # Blog content (Markdown/MDX)
│   ├── YYYY-MM-DD-slug/          # Post directories
│   │   ├── index.md              # Post content
│   │   └── *.{png,jpg}           # Post assets
│   ├── authors.yml               # Author metadata
│   ├── tags.yml                  # Tag definitions
│   └── linters/                  # Content validation
├── src/
│   ├── components/               # React components
│   │   ├── CookieConsent.tsx    # GDPR banner
│   │   └── CookieConsent.css
│   ├── pages/                    # Custom pages
│   │   └── webinars.tsx         # Community webinar page
│   ├── theme/Root/               # Global layout wrapper
│   ├── utils/analytics.ts        # GA4 helpers
│   ├── css/custom.css           # Global styles (purple theme)
│   └── js/consentModule.ts      # Consent management
├── static/                       # Static assets
│   ├── img/                     # Logos, favicons, social cards
│   └── webinars/agenda/         # Monthly agendas (YYYY-MM.md)
├── docusaurus.config.ts         # Main configuration
├── package.json                 # Dependencies & scripts
├── tsconfig.json                # TypeScript config
└── AGENTS.md                    # This file
```

### Critical File Locations

| Purpose | Path |
|---------|------|
| Blog posts | `blog/YYYY-MM-DD-slug/index.md` |
| Authors | `blog/authors.yml` |
| Tags | `blog/tags.yml` |
| Site config | `docusaurus.config.ts` |
| Theme | `src/css/custom.css` |
| Analytics | `src/utils/analytics.ts` |
| Cookie consent | `src/components/CookieConsent.tsx` |
| Webinars | `src/pages/webinars.tsx` |

---

## Development Workflow

### Adding a Blog Post

**See** `.github/instructions/website.blog.instructions.md` **for content guidelines.**

```bash
# 1. Create post directory
mkdir -p blog/2025-10-04-my-post-slug
cd blog/2025-10-04-my-post-slug

# 2. Create index.md
cat > index.md << 'EOF'
---
title: "Post Title"
date: 2025-10-04
description: "SEO description (150-160 chars)"
authors: [author-key]
tags: [tag1, tag2]
---

Intro paragraph explaining the post...

<!-- truncate -->

## Main Content

Details and examples...
EOF

# 3. Add images to same directory
# 4. Preview: npm start
# 5. Validate: npm run build
```

### Front Matter Requirements

**Required**:

```yaml
title: "Post Title"
date: YYYY-MM-DD
description: "SEO summary (150-160 chars)"
authors: [key-from-authors-yml]
tags: [keys-from-tags-yml]
```

**Optional**:

```yaml
image: ./hero.png      # Social card
slug: custom-url       # Override auto-slug
draft: true            # Hide from production
```

### Component Development

```typescript
// src/components/MyComponent.tsx
import React from 'react';
import styles from './MyComponent.module.css';

export default function MyComponent(): JSX.Element {
  return <div className={styles.container}>Content</div>;
}
```

**Best Practices**:

- Use TypeScript (`.tsx`)
- CSS Modules for scoped styles (`.module.css`)
- Import theme components: `import Layout from '@theme/Layout';`
- Test client-side code: `import ExecutionEnvironment from '@docusaurus/ExecutionEnvironment';`

---

## Customizations

### 1. Purple Theme (src/css/custom.css)

```css
--ifm-color-primary: #9f62eb;        /* Brand purple */
--ifm-color-primary-dark: #8e42e6;   /* Hover states */
--ifm-color-primary-darkest: #6f2bc7; /* Footer */
```

**Typography**: Inter (body), JetBrains Mono (code)

### 2. Cookie Consent System

**Components**:

- `src/components/CookieConsent.tsx` - Banner UI
- `src/utils/analytics.ts` - GA4 integration
- `src/js/consentModule.ts` - Consent state

**Flow**:

1. Banner shows on first visit
2. User accepts/rejects → stored in `localStorage`
3. If accepted: Load GA4 and send consent
4. Default: All analytics denied

**Update tracking ID**:

```typescript
// src/utils/analytics.ts
export const GA_CONFIG = {
  trackingId: 'G-XXXXXXXXXX',  // Replace placeholder
  anonymizeIP: true,
} as const;
```

### 3. Webinar Page (src/pages/webinars.tsx)

**Data Sources**:

1. Hardcoded timezone cards (edit `timezoneCalls` array in component)
2. Monthly agendas: `static/webinars/agenda/YYYY-MM.md`

**Agenda Format**:

```markdown
---
month: October 2025
---

## Topic Title
Presenter: Name, Title
Description: Brief summary
Featured: true
- Key point 1
- Key point 2
```

**Maintenance**: Create new `YYYY-MM.md` before each month's call. Component auto-falls back to previous month if current missing.

### 4. Root Wrapper (src/theme/Root/)

Injects `<CookieConsent />` globally on all pages.

---

## Content Management

### Authors (blog/authors.yml)

```yaml
author-key:
  name: Full Name
  title: Job Title
  url: https://linkedin.com/in/username
  image_url: https://github.com/username.png
  page: true                    # Generate author page
  socials:
    x: twitter_handle
    linkedin: linkedin_username
    github: github_username
```

**Usage**: `authors: [author-key]` in front matter

### Tags (blog/tags.yml)

```yaml
tag-key:
  label: Display Name
  permalink: /tag-url
  description: SEO description for tag page
```

**Common tags**: `aks`, `kubernetes`, `ai`, `networking`, `security`, `storage`, `observability`, `troubleshooting`, `gpu`, `istio`, `open-source`

**Usage**: `tags: [tag1, tag2]` in front matter

### Navigation

**Navbar**: Posts (/) | Tags | Resources (dropdown) | Releases | Roadmap | GitHub

**Footer**: Resources | Community | More (Roadmap, FAQ, RSS, Contact)

---

## Common Tasks

### Add Author

```yaml
# Edit blog/authors.yml
john-doe:
  name: John Doe
  title: Senior Engineer, AKS
  url: https://linkedin.com/in/johndoe
  image_url: https://github.com/johndoe.png
  page: true
  socials:
    github: johndoe
    linkedin: johndoe
```

### Add Tag

```yaml
# Edit blog/tags.yml
new-topic:
  label: New Topic
  permalink: /new-topic
  description: Brief SEO description
```

### Update Webinar Agenda

```bash
cat > static/webinars/agenda/2025-11.md << 'EOF'
---
month: November 2025
---

## Feature Highlight
Presenter: Jane Smith, PM Lead
Featured: true
- Topic overview
- Key benefits
EOF
```

### Customize Theme Colors

```css
/* Edit src/css/custom.css */
:root {
  --ifm-color-primary: #YOUR_COLOR;
  /* Update all color variations */
}
```

---

## Build & Deployment

### Build Process

```bash
npm run build
# 1. Compile TypeScript + React
# 2. Generate static HTML
# 3. Optimize CSS/JS bundles
# 4. Create RSS/Atom feeds
# 5. Copy rss.xml → feed.xml
```

### Output Structure

```text
build/
├── index.html           # Blog listing
├── tags/                # Tag pages
├── blog/YYYY/MM/DD/     # Post pages
├── rss.xml              # RSS feed
├── feed.xml             # Alias
├── atom.xml             # Atom feed
└── assets/              # Optimized CSS/JS/images
```

### Deployment Config

- **Base URL**: `/` (root)
- **Trailing slashes**: Disabled
- **Broken links**: Fail build
- **Target**: Azure Static Web Apps

### SEO Features

- RSS/Atom feeds with proper titles
- Social card: `static/img/social-card.jpg`
- Edit links to GitHub
- Auto-generated sitemap
- Meta descriptions from front matter

---

## Troubleshooting

### Build fails: "broken links"

**Fix**: Run `npm run build` to see exact errors. Use `./image.png` for same-directory images.

### Post not appearing

**Check**:

- [ ] Date not in future
- [ ] No `draft: true` in front matter
- [ ] Author key exists in `authors.yml`
- [ ] Tag keys exist in `tags.yml`
- [ ] File named `index.md`

### Images not loading

**Fix**:

- Images in same directory as `index.md`
- Use relative path: `![alt](./image.png)`
- Check case-sensitive filenames (Linux)

### Analytics not working

**Check**:

- [ ] `GA_CONFIG.trackingId` not placeholder
- [ ] User accepted cookies
- [ ] No console errors (DevTools)
- [ ] `gtag/js` loads (Network tab)

### TypeScript errors

```bash
npm run typecheck  # See all errors
```

**Common**: Missing imports from `@docusaurus/types`, wrong prop types

### Hot reload broken

```bash
npm run clear  # Clear cache
npm start      # Restart
```

---

## Agent Checklist

### When Adding Blog Posts

- ✅ Use `blog/YYYY-MM-DD-slug/index.md` structure
- ✅ Complete required front matter
- ✅ Add `<!-- truncate -->` after intro
- ✅ Images in same dir with `./` prefix
- ✅ Validate author/tag keys exist
- ✅ Follow `.github/instructions/website.blog.instructions.md`
- ✅ Test: `npm start` + `npm run build`

### When Modifying Config

- ✅ Edit `docusaurus.config.ts` for navbar/footer/SEO
- ✅ Edit `src/css/custom.css` for theme
- ✅ Run `npm run typecheck` after changes
- ✅ Test light + dark modes

### When Creating Components

- ✅ Use TypeScript (`.tsx`)
- ✅ CSS Modules for styles
- ✅ Import from `@theme/` aliases
- ✅ Test client-side features with `ExecutionEnvironment`

### Quality Checks

- ✅ `npm run build` succeeds
- ✅ `npm run typecheck` passes
- ✅ Posts render with images
- ✅ No broken links
- ✅ RSS/Atom feeds valid
- ✅ SEO metadata complete
- ✅ Mobile responsive
- ✅ Accessibility (alt text, semantic HTML)

---

## Links

- **Production**: <https://blog.aks.azure.com\>
- **Repository**: <https://github.com/Azure/AKS\>
- **Docusaurus Docs**: <https://docusaurus.io/docs\>
- **Contact**: brian.redmond@microsoft.com

---

**Last Updated**: 2025-10-04
**Docusaurus**: 3.8.1
**Node**: >= 18.0
