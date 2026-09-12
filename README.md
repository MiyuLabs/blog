# MiyuLabs — Blog

This repository works hand-in-hand with the main [MiyuLabs/www](https://github.com/MiyuLabs/www) platform. It serves as the decoupled headless content source for all public blog posts on the site.

To update the blog, after pushing changes to the `main` branch of this repository, trigger the `workflow_dispatch` of the `update.yml` GitHub Action.

> It automatically triggers the deployment pipeline on the main platform to prepare a new build with the updated `content/` & atomically swaps the new build and purges the old cloudflare cache.

---

## Repository Structure

```text
blog/
├── posts/                  # The actual markdown (MDX) articles
│   ├── article-one.mdx     
│   └── article-two.mdx
├── authors/
│   └── id.png
└── authors.json            # Author metadata database
```

### Authors (`authors.json`)
Before assigning an author to an MDX post, they must be defined in the `authors.json` database.

```json
[
  {
    "id": "rick",
    "name": "Rick Astley",
    "bio": "never gonna give you up",
    "role": "Special Appearnce",
    "link": "https://rickastley.co.uk",
    "avatar": "https://raw.githubusercontent.com/MiyuLabs/blog/main/authors/id.png"
  }
]
```

### Posts (`posts/*.mdx`)
Posts are written using standard GitHub Flavored Markdown, supercharged with MDX components. The filename determines the final URL slug (e.g., `posts/hello-world.mdx` becomes `https://miyulabs.in/blog/hello-world`).

Every post **must** start with YAML frontmatter:

```mdx
---
title: "Hello World"
summary: "This is a brief summary of the article."
publish_date: "2026-09-12"
author_id: "rudra"
tags: ["engineering", "design"]
draft: false
---

## Hello World

Your content goes here...
```

---

## Available MDX Components

Because this repository renders via Next.js MDX, you can use custom React components directly inside your Markdown files!

### `<Callout />`
Used to draw attention to specific notes, warnings, or tips.

**Types available:** `note` (default), `tip`, `important`, `warning`, `caution`

**Usage:**
```mdx
<Callout type="tip">
  This is a helpful tip for the reader!
</Callout>

<Callout type="warning">
  Watch out! This action cannot be undone.
</Callout>
```

---

## Deployment

This repository contains an `.github/workflows/update.yml` script. When you trigger the `workflow_dispatch` event, this script reaches out to the main `MiyuLabs/www` repository and triggers a `workflow_dispatch` event on its `deploy.yml` pipeline which builds & deploys the updated build.

### Setup Instructions
For this cross-repository trigger to work, you must provide authentication:

1. Generate a **Fine-grained Personal Access Token** from your GitHub Developer Settings.
2. Grant it **Actions: Read & Write** permissions specifically on the `MiyuLabs/www` repository.
3. Add this token to the **Secrets** of *this* repository (`MiyuLabs/blog`) under the name:
   `DEPLOY_PAT`

*Note: You must generate a Fine-grained Personal Access Token with `Actions: Read & Write` permissions on `MiyuLabs/www` and add it to the `MiyuLabs/blog` repository secret as `DEPLOY_PAT`.*
