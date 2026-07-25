# CLAUDE.md

Personal academic portfolio for Aman Chulawala — PhD student at CERLAB, Carnegie
Mellon. Jekyll site deployed by GitHub Pages at https://farstryke21.github.io/.

Originally forked from the [academicpages](https://github.com/academicpages/academicpages.github.io)
template (itself a Minimal Mistakes fork). The theme plumbing is still upstream;
the presentation layer and all content are ours.

## Running locally

**Requires Ruby 3.1.x** — see `.ruby-version`. This is not optional: the
`github-pages` gem pins Liquid 4.0.3, which calls `String#tainted?`, and that
method was removed in Ruby 3.2. Newer Rubies cannot build this site.

```sh
brew install ruby@3.1
export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"
bundle install
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

Always pass `_config.dev.yml` when serving locally. Without it `site.url` stays
at the production domain and every page pulls its CSS from the live site rather
than your local build — the page renders with stale styles and it is not obvious
that anything is wrong.

## Structure

| Path | Purpose |
|---|---|
| `_pages/` | Standalone pages. `about.html` is the homepage (`permalink: /`). |
| `_projects/` | 15 project write-ups → `/projects/<filename>/` |
| `_research/` | 7 research write-ups → `/research/<slug>/` |
| `_data/news.yml` | News feed — homepage (latest 5) and `/news/` |
| `_data/articles.yml` | Substack/Medium links → `/writing/` and homepage strip |
| `_data/navigation.yml` | Top nav |
| `_sass/_tokens.scss` | Design tokens + dark mode + theme bridge |
| `_sass/_components.scss` | Every reusable component |
| `_includes/entry-cards.html` | Card grid for projects/research |
| `_includes/news-list.html`, `article-cards.html` | News and writing renderers |

Talks, teaching, publications and portfolio are **shelved** — the collection
definitions are commented out in `_config.yml`. Re-enable by uncommenting and
adding entries to the matching directory.

## Rules

**No inline `<style>` blocks in content files.** This was the single biggest
problem with the old site: ~20 files each shipped their own CSS, most starting
with `body { font-family: Arial; margin: 20px }`, which overrode the theme
globally and made every page look slightly different. Add or extend a component
in `_sass/_components.scss` instead.

**Colours come from tokens**, never hardcoded hex. `var(--surface)`,
`var(--text)`, `var(--accent)`, etc. — defined in `_sass/_tokens.scss` with a
`prefers-color-scheme: dark` block. Hardcoding a colour means it breaks in dark
mode.

**Watch specificity against the theme.** Minimal Mistakes styles
`.page__content a`, `.page__content h2/h3`, and `.btn`, all of which outrank a
bare class. Components that need to win are written as
`.page__content .foo, .archive .foo, .foo { … }`. If you add a modifier
(e.g. `.btn--primary`), it needs the same treatment or the base rule outranks it.

## Adding content

**A project** — new file in `_projects/`:

```yaml
---
title: "Project Name"
collection: projects
order: 7                # controls position on /projects/; lower is earlier
featured: true          # optional — surfaces it on the homepage
excerpt: "One or two sentences. Feeds the card, SEO, and link previews."
image: /images/projects/<name>/icon.png
external: https://…     # optional — marks it as an external project
tags:
  - Computer Vision
  - Planning
---
```

Tags must exactly match existing tags (they drive the filter chips on
`/projects/`) — a stray trailing space creates a duplicate chip.

**Research** — same, in `_research/`, without `tags`. Use a descriptive
filename; it becomes the URL. Add `redirect_from` if renaming an existing entry.

**News** — one entry at the top of `_data/news.yml`. Only month and year display.

**An article** — one entry in `_data/articles.yml` (`title`, `url`, `date`,
`source`, optional `image`/`excerpt`). `/writing/` and the homepage strip both
hide themselves when the file is empty.

Body markup uses these components: `.figure` + `.figure__caption` for images and
video, `.btn-row` + `.btn` for links out, `.feature` for image-beside-text,
`.timeline` for dated lists.

## Verifying a change

```sh
bundle exec jekyll build --config _config.yml,_config.dev.yml   # must be warning-free

# No broken asset references
grep -rhoE '/(images|files)/[A-Za-z0-9_./ -]+\.(png|jpg|jpeg|JPG|gif|svg|pdf|mp4|webm)' \
  _projects _research _pages _includes _config.yml | sort -u \
  | while read -r p; do [ -f ".$p" ] || echo "MISSING: $p"; done

grep -rln "<style>" _pages _projects _research    # must return nothing
```

Then check `/`, `/projects/`, `/research/`, a detail page, and `/resume/` at
375px and 1440px, in both light and dark mode.
