# Copilot instructions

## Build and validation

- Build the site with `hugo --gc`. The GitHub Pages workflow runs the same build with its Pages `--baseURL` and a cache directory.
- Preview changes locally with `hugo server`.
- The repository has no configured test or lint runner, so there is no single-test command. A successful Hugo build is the available site-wide validation.
- CI uses Hugo Extended 0.154.4; use an Extended build because the theme processes CSS and JavaScript through Hugo's asset pipeline.

## Architecture

- The root `hugo.toml` configures the WCSA site (title, `en-gb` language code, canonical URL, theme, and section-based main menu). The presentation theme is the in-repository `themes/wcsaTheme`.
- Put WCSA pages in the root `content/` tree and site-owned images or other files in `static/`; Hugo serves static files from the site root (for example, `static/images/lane-ropes.jpeg` is `/images/lane-ropes.jpeg`). The theme's own `content/` and config contain starter examples, not the source of WCSA pages.
- The theme's `layouts/baseof.html` provides the page shell and invokes shared head, header, and footer partials. `home.html`, `section.html`, `page.html`, and taxonomy layouts supply page-specific content through the `main` block.
- Theme CSS and JavaScript live under `themes/wcsaTheme/assets/` and are built by the head partials with Hugo Pipes; production assets are minified and fingerprinted, while development assets use source maps. Component CSS is imported by `assets/css/main.css`.
- Bulma is loaded from the CDN in the theme head partial and is the site's CSS layout library. Prefer Bulma CSS classes and components wherever possible; add custom CSS only when Bulma has no suitable class or component. Use Bulma's layout and navbar patterns; keep top-level main-menu items horizontal.
- `.github/workflows/hugo.yml` builds and deploys to GitHub Pages on pushes to `main`. It sets `Europe/London`, builds with Hugo Extended 0.154.4, and supplies the Pages base URL; changes should pass the local Hugo build before deployment.

## Content and theme conventions

- Content files use TOML front matter delimited by `+++`; follow the existing fields such as `title`, `date`, and `draft`. The default archetype sets `draft = true`, and the normal build omits drafts until they are published.
- The main navigation is generated from root site sections via `sectionPagesMenu = 'main'`. Add or update content sections rather than duplicating their links in theme templates.
- Keep site-specific presentation in the theme layouts, partials, and assets. Use the existing Hugo partials and named `main` blocks when changing page rendering, and the Hugo Pipes resource pattern when changing CSS or JavaScript.
