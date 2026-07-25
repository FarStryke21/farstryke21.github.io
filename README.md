# farstryke21.github.io

Personal academic portfolio — [farstryke21.github.io](https://farstryke21.github.io/)

Jekyll site deployed automatically by GitHub Pages on every push to `master`.

## Running locally

Requires **Ruby 3.1.x** (see `.ruby-version`). Newer Rubies cannot build this
site: the `github-pages` gem pins Liquid 4.0.3, which calls `String#tainted?`,
removed in Ruby 3.2.

```sh
brew install ruby@3.1
export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"

bundle install
bundle exec jekyll serve --config _config.yml,_config.dev.yml
```

Then open http://localhost:4000.

> Pass `_config.dev.yml` when serving locally. Without it, `site.url` points at
> the production domain and pages load CSS from the live site instead of your
> local build.

## Where things live

| I want to change… | Edit |
|---|---|
| Homepage (hero, about, background) | `_pages/about.html` |
| News items | `_data/news.yml` |
| Substack / Medium articles | `_data/articles.yml` |
| A project | `_projects/` |
| A research entry | `_research/` |
| Top navigation | `_data/navigation.yml` |
| CV PDF | replace `files/Resume_Aman.pdf` (keep the filename) |
| Colours, spacing, type | `_sass/_tokens.scss` |
| Reusable styles | `_sass/_components.scss` |

Front-matter contracts, component reference, and the house rules (no inline
`<style>`, tokens over hex, theme-specificity gotchas) are documented in
[`CLAUDE.md`](CLAUDE.md).

## Credits

Forked (then detached) from [academicpages](https://github.com/academicpages/academicpages.github.io),
itself a fork of the [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/)
Jekyll theme, © 2016 Michael Rose, MIT licensed. See `LICENSE`.
