# Architecture Overview

This repository is a Next.js-based content platform for public goods funding discovery. The architecture combines a content-driven App Router frontend, reusable UI components, automation scripts for publishing from GitHub issues, and static/public assets for images and generated content.

```mermaid
flowchart LR
    User[Browser / Visitor]

    subgraph App[Next.js Application]
        Router[src/app\nApp Router pages and routes]
        Components[src/components\nReusable UI blocks and layouts]
        Content[src/content\nMarkdown content for apps, mechanisms, research, campaigns]
        Data[src/data\nStatic metadata, authors, config]
        Lib[src/lib\nUtilities and helpers]
        API[src/app/api\nAPI endpoints and integrations]
    end

    subgraph Source[Content Pipeline]
        Issues[GitHub Issues\nSubmissions and approvals]
        Scripts[scripts/\nPublish, validate, banner generation]
    end

    subgraph Assets[Static Runtime Assets]
        Public[public/\nImages, generated assets, content-images]
        CDN[Static hosting / CDN]
    end

    User -->|Requests pages| Router
    Router --> Components
    Router --> Content
    Router --> Data
    Router --> Lib
    Router --> API
    API --> Data
    API --> Content

    Issues -->|Approved items| Scripts
    Scripts -->|Transforms and validates| Content
    Scripts -->|Publishes assets| Public

    Components -->|Render content and layouts| User
    Content -->|Markdown + frontmatter| Router
    Data -->|Metadata and lookup| Router
    Public -->|Static files| CDN
    CDN --> User
```

## Key Architectural Layers

- Frontend: Next.js App Router in `src/app` handles routes such as home, apps, mechanisms, research, campaigns, and preview pages.
- UI Layer: `src/components` contains reusable cards, layouts, search, impact sections, and markdown rendering helpers.
- Content Layer: `src/content` stores the actual application content in structured markdown files, while `src/data` holds supporting metadata.
- Publishing Automation: `scripts/` validates content, generates banners, and syncs content from issue-driven workflows.
- Static Assets: `public/` stores images, generated content images, and other files served directly by the app.

## Runtime Flow

1. A user visits a route in the app.
2. The Next.js router loads the relevant page and associated components.
3. Content is read from `src/content` and metadata from `src/data`.
4. Reusable UI components render cards, detail pages, and sections.
5. Static public assets are served for banners, logos, and images.
6. Content contributors can submit or update entries via GitHub Issues, which are then processed by automation scripts.
