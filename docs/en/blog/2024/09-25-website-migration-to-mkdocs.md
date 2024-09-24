---
comments: true
title:  "Website migration to MkDocs"
date:   2024-09-25
tags:
    - Development
hide:
    - navigation
excerpt:
---

After maintaining this website using Jekyll for a couple of years, I decided it was time for a change. As much as I enjoyed the flexibility Jekyll offered, the needs of my projects had evolved. I had already been using MkDocs for documenting Python projects, so I decided to consolidate everything under MkDocs. This post will explain the rationale behind this migration, the benefits, some trade-offs, and why I switched from Disqus to Giscus for managing comments.

## Why Move from Jekyll to MkDocs?

Jekyll is a fantastic static site generator, especially if you're hosting on GitHub Pages. It provides great support for blog posts, themes, and plugins, but it wasn’t meeting all my needs anymore. I did not have a search bar, light dark themes and my website felt overall hacky. Maintaining separate platforms for blogging and documentation started to feel counterproductive. MkDocs, a static site generator designed primarily for documentation, seemed like the perfect alternative to unify everything.

## Benefits of Using MkDocs for My Site

### Integrated Documentation

MkDocs is built for documentation, making it easier to make tutorials with blocks for notes, search bar, navigation features. This integration allows me to work seamlessly between my blogs and technical write-ups without switching frameworks.

### Simplified Workflow

MkDocs is incredibly simple. With its Markdown-based structure, there's no need to deal with complex templating like with Jekyll. I can write everything in Markdown, which significantly speeds up content creation.

### Multilingual Support with Ease

One of my core requirements was to support both English and French. While Jekyll does provide ways to handle multilingual websites, MkDocs has plugins, such as [mkdocs-static-i18n](https://ultrabug.github.io/mkdocs-static-i18n/), that simplify managing translations for pages. It's also easier to keep track of language-specific documentation without complicated folder structures or configurations.

### MkDocs Material Theme

The Material theme for MkDocs provides an elegant, modern look right out of the box. With a few tweaks, I was able to make the blog look visually appealing without spending much time on custom CSS or theme development, unlike Jekyll where I often found myself tinkering with themes to get things right.

## Cons of Switching to MkDocs

### Fewer Plugins for Blogging

Jekyll has a rich ecosystem of plugins specifically designed for bloggers, while MkDocs has fewer options. For example, managing a series of posts or custom taxonomies is more mature in Jekyll, while MkDocs requires some workarounds to achieve similar functionality. Nevertheless, I never used this functionality, so it is not a huge loss for me.

### Limited Theme Flexibility

MkDocs is more rigid in terms of theming compared to Jekyll. While the Material theme looks great, it doesn’t allow the same level of customization that Jekyll themes do without delving into custom CSS.
This is a big plus for me, as I spent too much time tweaking Jekyll's default theme, only to achieve somewhat average results.

## Giscus Instead of Disqus

One of the more technical changes I made was switching from Disqus to Giscus for comment handling. Here’s why:

### Privacy and Performance

Disqus, while popular, comes with a cost to privacy and page performance. It injects ads and loads several third-party trackers, which results in slower page loads. Giscus, on the other hand, is lightweight and leverages GitHub Discussions for comments. It’s fast, doesn’t serve ads, and aligns more with my privacy-focused approach.

### GitHub Integration

Since I already use GitHub for version control and collaboration on various projects, Giscus seamlessly integrates comments into GitHub Discussions. This creates a unified space where users can not only leave comments but also engage in discussions related to specific blog posts.

### Open Source

Giscus is open source, which aligns with my preference for using open and transparent tools wherever possible. Disqus’s proprietary nature and ad-driven model had been a drawback for me for a while, so Giscus was a natural choice when looking for alternatives.

## Final Thoughts

Switching from Jekyll to MkDocs has streamlined my workflow, making it easier to manage both website and my blog. While MkDocs might not be the go-to choice for traditional bloggers, its simplicity and focus on documentation make it a powerful tool for technical content creators like myself. The transition has allowed me to simplify my setup and keep everything more organized.

For those who run documentation-heavy blogs or have technical projects that need detailed documentation, I highly recommend trying out MkDocs (especially with the Material theme). And if you’re looking for a fast, privacy-respecting comment system, Giscus is a perfect fit.

I hope this post helps and/or inspires some of you.
If you have suggestions feel free to comment or reach me.

See you again, Vincent.
