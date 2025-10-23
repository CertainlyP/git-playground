# Threat Hunting Research Blog

A GitHub Pages blog for sharing threat hunting analysis, detection techniques, and security research.

## Live Blog

Once deployed, visit: `https://certainlyp.github.io/git-playground/`

## Quick Start

See [BLOG_GUIDE.md](BLOG_GUIDE.md) for complete instructions on:
- Enabling GitHub Pages
- Writing blog posts
- Publishing content
- Customization options

## Blog Structure

```
.
├── _config.yml           # Site configuration
├── _posts/               # Blog posts (YYYY-MM-DD-title.md)
├── _layouts/             # Custom layouts (optional)
├── _includes/            # Reusable components (optional)
├── assets/
│   ├── css/             # Custom styling
│   └── images/          # Images for posts
├── index.md             # Homepage
├── about.md             # About page
├── Gemfile              # Ruby dependencies
└── BLOG_GUIDE.md        # Complete blog usage guide
```

## Writing Posts

Create posts in `_posts/` with the format `YYYY-MM-DD-title.md`:

```markdown
---
layout: post
title: "Your Post Title"
date: 2025-10-23
categories: threat-hunting
tags: [detection, sigma, kql]
---

Your content here...
```

## Features

- Markdown-based content
- Syntax highlighting for detection queries (KQL, SPL, Sigma, PowerShell)
- Responsive design
- SEO optimization
- RSS feed

## Content Focus

- Threat hunting techniques
- Detection engineering
- Security analysis
- MITRE ATT&CK mapping
- IoC sharing
- Tool reviews
