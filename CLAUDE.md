# Notes for agents working on this site

Rory McCune's portfolio site: talks, projects, articles and tutorials on container and Kubernetes security. `README.md` covers the content workflows (adding talks, projects and so on). Read it first. This file covers how the site is built and the rules to keep when changing it.

## Build and deploy

- Plain Jekyll 3 through the `github-pages` gem. There is no theme gem; all layouts are local.
- GitHub Pages uses the **classic (legacy) build** from `main`, repo root, with custom domain `www.mccune.org.uk`. Pushing to `main` deploys. There is no Actions workflow.
- Plugins must stay on the GitHub Pages whitelist. Current list: `jekyll-feed`, `jekyll-redirect-from`, `jekyll-seo-tag`, `jekyll-sitemap`. Anything else is silently ignored in production.
- Local preview: `bundle exec jekyll serve`. `_config.yml` changes need a server restart.
- Local production build: `PAGES_REPO_NWO=raesene/www.mccune.org.uk JEKYLL_ENV=production bundle exec jekyll build`. Without the env var the github-metadata plugin fails; GitHub's builder sets it for you. Keep `baseurl: ""` in the config, or offline builds guess a `/pages/...` baseurl.
- Google Analytics (`_includes/google-analytics.html`) is included only when `JEKYLL_ENV=production`.

## Structure

- **Talks are posts.** Each file in `_posts/` has `category: talks`, and every page that lists talks reads `site.categories.talks`. Posts get `layout: post` from config defaults. Permalinks are `/content/talks/YYYY-MM-DD-title/` and must not change, because old URLs are linked from elsewhere.
- `/talks/` is `_featured_categories/talks.md` (`layout: talks`, `slug: talks`) in a collection with permalink `/:name/`. It also carries `redirect_from: /posts/`.
- **Projects** are the `projects` collection (`/projects/:path/`). The front matter fields used by the layouts are `plate` (Trame illustration, required), `title`, `caption`, `description`, `image.path` (real screenshot, optional), `links` and `featured`.
- **Articles and tutorials** are data files: `_data/articles.yml` and `_data/tutorials.yml`. Homepage counts are computed from them in Liquid; never hard-code counts.
- **Layouts:** `default.html` (the shell: head, masthead, footer), then `home`, `page`, `post`, `talks`, `projects`, `project`, `articles`, `tutorials` and `not-found`.
- **Styling:** one hand-written stylesheet, `assets/css/nitrile.css`. There is no Sass.

## Design language: Nitrile

The site follows Katagami's **Nitrile** language. Full spec: https://katagami.ai/language/en-01a05fe2-99dc-72a3-8e04-23200da1edc6/DESIGN.md (or use the Katagami MCP: `get_library_entry` with kind `design_language` and slug `nitrile`).

Tokens (already CSS variables in `:root`, so use the variables):

| Role | Value |
|---|---|
| Background | `--bg #F4F0E7` |
| Tray surface | `--surface #FBF8F0` |
| Recess | `--recess #EFEADF` |
| Ink | `--ink #141311` |
| Muted | `--muted #6B665D` |
| Hairline | `--hair rgba(20,19,17,.22)` |
| Accent | `--accent #168EA0` |
| Accent wash | `--accent-soft #DCEBEC` |

Fonts: Archivo Narrow for headings, Noto Sans for body (17px, line-height 1.55), Fragment Mono for dates, labels and data.

Rules to keep:

- **Zero border radius, everywhere.** Borders are 1px hairlines. Rank comes from frame weight: the one dominant panel on a screen gets a full-ink border plus `--shadow-md`, and everything else uses `--hair`.
- **Cyan marks a decision, never decoration.** Use it for the 6px `.pin` square, the pinned (latest) row, filled meters and focus rings. Never use it as text colour (it fails contrast on ivory), and never as a coloured strip along an edge.
- **Layout:** a dominant panel (`.slab`), a narrow `.gutter`, and air between them. Never three equal cards in a row. Lists are packet strips (`.strip`, `.strip__row`), not card grids.
- **Uppercase only** for small mono module labels and button caps. Headings are sentence case.
- **Motion:** almost none. There's one load animation (the hero "stop-down" clip-path) and 140ms colour transitions. Respect `prefers-reduced-motion`.
- **No glow, glass, gradients, neon, code rain or malware iconography.**
- **Light mode only.** Rory decided against dark mode; don't add one.

After a substantial page or CSS change, run Katagami's `check_page_against_language` (slug `nitrile`) on the rendered HTML with the CSS inlined, and fix any failures. The measured checks flag any colour or font not in the tokens.

## Illustrations: Trame

Every illustration uses Katagami's **Trame** art style: https://katagami.ai/art-styles/en-01a05fe2-983d-7531-9f98-a94bb685e9dc. Generate them with the `openrouter-image` skill (default model, `-a 16:9 -r 2K`, about $0.10 each). Pass two reference images from the style's gallery:

```
--ref "https://katagami.ai/api/file/fl-01a0c9b1-f75f-7b13-84f5-e0c4e3b8c8f3?v=asset-cdn-v3"
--ref "https://katagami.ai/api/file/fl-01a0c9b1-cee2-78b0-b8c8-d6ee226289fe?v=asset-cdn-v3"
```

Use this prompt template verbatim, then add a composition line and a subject line:

```
Create a matte black pen and adhesive dot-screen drawing on warm ivory paper with faint tooth.
Use hard single-weight black contours and clear mechanically regular round dots in screened tone blocks.
Keep the subject recognizable with clean descriptive outlines and distinct tonal regions.
Model every shaded form through changes in visible dot coverage, using denser or larger dots for darker areas and open ivory for light; never smooth grey shading.
Use black and warm ivory only, apart from one small solid cyan square.
Give the subject a clear focused arrangement and retain one broad quiet ivory field, with slim outer margins.
Place thin registration crosses and unnumbered ruler ticks in the outer margin and put the small cyan fiducial square off centre.
Exclude text, letters, numbers, logos, signatures, watermarks, additional chromatic colors, gradients, blur, glow, airbrushing, photographic rendering and imitation of an individual artist.

Composition: One object at three-quarter view on a plain surface, dot-screen modelling on the shadow side, registration ticks along the bottom margin. Keep a broad quiet ivory field on one side of the frame.

Subject: <a concrete object or scene that stands for the project, no people>. The small cyan square sits on <the part that matters>.
```

- Subjects imply the person through what they left behind (an empty lectern, a pushed-back chair, gloves laid flat) and never draw anyone.
- Pick a physical metaphor for the project. For example, RBAC is a key board with an empty hook; the CIS benchmarks are an object under inspection with calipers and a checklist.
- Look at each result with the Read tool before using it, and regenerate if it has text in it or a second colour.
- Convert with `cwebp -q 80 -resize 1400 0 in.png -o out.webp` and save under `assets/img/nitrile/` (project plates in `projects/`, named after the project slug).

## Verifying a change

1. `bundle exec jekyll build` with no errors.
2. Check internal links: every `href`/`src` starting with `/` in `_site/**/*.html` should resolve to a file or a directory `index.html`.
3. Screenshot at 1440px and 390px. Snap Chromium can't write to `/tmp`, so save screenshots under `$HOME`. Headless Chrome won't render a window narrower than about 500px, so for a real 390px check, load the page in a 390px-wide `<iframe>` on a local page.
4. Don't change existing talk or project URLs. If a URL must move, add `redirect_from`.

## Privacy

Don't put Rory's email address in page content. The footer links are the blog, GitHub, LinkedIn and the feed.
