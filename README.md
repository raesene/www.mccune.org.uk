# www.mccune.org.uk

Personal site: talks, projects, articles and tutorials. Plain Jekyll, published by GitHub Pages (classic build) from `main`. Pushing to `main` makes it live within a minute or two.

The look is the [Nitrile](https://katagami.ai/language/en-01a05fe2-99dc-72a3-8e04-23200da1edc6) design language from Katagami, with illustrations in its [Trame](https://katagami.ai/art-styles/en-01a05fe2-983d-7531-9f98-a94bb685e9dc) art style.

## Preview locally

```bash
bundle install          # first time only
bundle exec jekyll serve
```

Open http://localhost:4000. Changes to content and layouts reload on save; changes to `_config.yml` need a restart.

## Add a talk

Create `_posts/YYYY-MM-DD-short-slug.md`:

```markdown
---
title: The title of the talk
category: talks
---

The abstract, a paragraph or two.

This talk was delivered at BSides Somewhere 2026. There's a video [here](https://www.youtube.com/watch?v=...).
```

- The date in the filename is the talk date. It sets the order and the year grouping on `/talks/`.
- `category: talks` is required, or the talk won't appear anywhere.
- A YouTube or Vimeo link anywhere in the text marks the talk as having a video.
- The homepage talk log, the counts and the talk feed all update on their own.

## Add a project

Create `_projects/short-slug.md`:

```markdown
---
layout: project
plate: /assets/img/nitrile/projects/short-slug.webp
title: 'Project name'
caption: One line shown under the title and in lists
description: >
  A paragraph describing the project and your part in it.
image:
  path: /assets/img/projects/short-slug.png
links:
  - title: Project website
    url: https://example.com
sitemap: false
---

Optional extra detail in Markdown.
```

- `plate` is the Trame drawing shown on the projects pages. Every project needs one (see "Images" below).
- `image.path` is optional: a real screenshot, shown on the project page and used for social previews.
- A link titled plain `Link` is shown as "Visit the project".
- To feature a project on the homepage and at the top of `/projects/`, add `featured: true`. Only one project should have it (currently OWASP Kubernetes Top 10).

## Add an article

Articles live in `_data/articles.yml`, grouped by publication, newest first within each:

```yaml
- publication: Datadog Blog
  articles:
    - title: "The article title"
      url: https://...
```

Add to an existing publication, or add a new `- publication:` block. The counts and the homepage's "writing elsewhere" section update automatically. The homepage shows the latest four from the first publication in the file.

## Add a tutorial

Tutorials live in `_data/tutorials.yml`:

```yaml
- title: Tutorial name
  url: https://labs.iximiuz.com/tutorials/...
  summary: One line for the homepage.
  description: >
    A paragraph for the /tutorials/ page.
```

## Images

Illustrations use the Trame style: black pen and dot-screen on ivory paper, with one small cyan square. They show objects, never people. Claude Code can make new ones with the `openrouter-image` skill (about $0.10 each). `CLAUDE.md` has the exact prompt, so just ask for "a Trame plate for the new project about X".

Save them as WebP under `assets/img/nitrile/` (project plates go in `assets/img/nitrile/projects/`), about 1400px wide.

## Other pages

| Page | Content | Layout |
|---|---|---|
| Home | built from talks, projects and data files | `_layouts/home.html` |
| `/talks/` | intro text in `_featured_categories/talks.md` | `_layouts/talks.html` |
| `/projects/` | intro in `projects.md` | `_layouts/projects.html` |
| `/articles/` | intro in `articles.md` | `_layouts/articles.html` |
| `/tutorials/` | intro in `tutorials.md` | `_layouts/tutorials.html` |
| 404 | `404.md` | `_layouts/not-found.html` |

The navigation menu is the `menu:` list in `_config.yml`. All styling is in `assets/css/nitrile.css`.
