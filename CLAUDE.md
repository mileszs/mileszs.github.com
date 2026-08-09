# CLAUDE.md

Guidance for Claude Code when working in this repository.

## What this is

The personal blog of Miles Z. Sterrett. Jekyll, built and hosted by **GitHub
Pages**, served at <https://mileszs.com>. Posts go back to 2008.

## Critical constraints

**Deploys run through GitHub Actions**, not GitHub's legacy Pages builder. See
`.github/workflows/pages.yml`. Pushing to `main` triggers a build with
`bundle exec jekyll build` and a deploy via `actions/deploy-pages`. Pages is
configured with `build_type: workflow`; the legacy builder is off.

- **Build failures show up in the Actions tab** with the real Jekyll error.
  Check there first. (The legacy builder reported only `Page build failed.`,
  which is why this repo moved off it.)
- **CI builds from this repo's `Gemfile.lock`**, so local and deployed builds
  use identical gem versions. Jekyll is pinned to 3.10 by `github-pages`; do
  not use Jekyll 4-only syntax, and bump `github-pages` rather than `jekyll`.
- **Jekyll no longer runs in `--safe` mode.** Plugins outside
  [GitHub Pages' allowlist](https://pages.github.com/versions/) would now work.
  Nothing currently relies on that — enabled plugins are `jekyll-feed`,
  `jekyll-paginate`, `jekyll-seo-tag`, `jekyll-sitemap` — but a custom plugin
  in `_plugins/` is a real option if it ever earns its keep.
- **`_site/` is not committed.** It is a local build artifact and is
  gitignored. There is no `gh-pages` branch.

**`exclude:` in `_config.yml` REPLACES Jekyll's default exclude list — it does
not extend it.** The comment originally shipped in that file said the opposite
and was wrong. The defaults are repeated verbatim in the config for this
reason. If you add an entry, do not delete the defaults: dropping `Gemfile`
and `Gemfile.lock` publishes them to the live site, and dropping
`vendor/bundle/` breaks CI outright, because `ruby/setup-ruby` installs gems
into the workspace and Jekyll then tries to parse the Jekyll gem's own
`site_template` post, whose front matter has an ERB placeholder for a date.

Files and directories starting with `.` or `_` are always ignored by Jekyll, so
`.github/`, `.claude/` and `_plans/` need no exclude entry.

**Branches.** Deploys come from `main`. The old `master` branch was deleted.

**`CNAME`** contains `mileszs.com`. Do not delete it — that file is what binds
the custom domain, and losing it takes the site off its domain. The workflow
asserts it survives the build and fails the deploy if it does not.

**Three things must name the same host, or canonical tags point at a URL that
redirects:** the `CNAME` file, `url:` in `_config.yml`, and the GitHub Pages
custom domain (`gh api repos/mileszs/mileszs.github.com/pages --jq .cname`).
All three are `mileszs.com`. The apex is canonical; `www.mileszs.com` 301s to
it. These drifted apart once already during a DNS migration.

## Commands

```bash
bundle install                 # first time / after Gemfile changes
bundle exec jekyll serve       # local preview at http://127.0.0.1:4000
bundle exec jekyll build       # one-off build into _site/
bundle update github-pages     # the correct way to upgrade the toolchain
```

Ruby version is pinned in `.tool-versions` (asdf/mise).

## Layout of the repo

```
_posts/               99 posts, `YYYY-MM-DD-slug.md` or `.markdown`
_layouts/             default, home (paginated index), post, page
_includes/            head, navigation, footer
_sass/tale/           the Tale theme, vendored in full so it can be edited
assets/               main.scss, favicons, wizard avatar, resume PDF
images/               post images
about/, gifts/,       standalone pages (`layout: page`)
  archive/, northview-theater/
llms.txt, robots.txt  generated through Liquid; see "AI discoverability"
```

The Tale theme was deliberately vendored rather than used as a theme gem, so
every file under `_sass/tale/` is fair game to edit.

## Conventions

**Post front matter.** Minimum is `title` and `date`; `layout: post` is applied
automatically by the `defaults` block in `_config.yml`.

```yaml
---
title: "Post Title in Title Case"
date: 2026-08-03
description: "One or two sentences. Optional, but it becomes the meta description and the AI-facing summary."
---
```

**Permalinks are `/:title`** — flat, no dates, no `/blog` prefix. This means the
filename slug determines the URL. **Never rename an existing post file**; the
URL is the permalink and old links will break. If a title genuinely needs
changing, change `title:` in the front matter and leave the filename alone.

**Historical posts are not revised.** A 2010 post about Rails describes Rails as
it was in 2010. Do not "fix" the technical content of old posts, update code
samples, or modernize opinions. Typos and broken metadata are fair to fix;
substance is not.

**Descriptions.** `jekyll-seo-tag` falls back to the post excerpt (its first
paragraph) when `description` is absent, so posts always get *something*. Pages
using `layout: page` have no excerpt, so they need an explicit `description`.

## AI discoverability

This site is deliberately open to AI crawlers. Three files carry that:

- **`robots.txt`** — allows everything, names the major AI crawlers explicitly,
  and points at the sitemap. It has front matter and is rendered by Liquid so
  the URLs stay correct.
- **`llms.txt`** — a curated plain-text map of the site following
  <https://llmstxt.org>. Also Liquid-rendered: the post lists come from
  `site.posts`, so it stays current on its own. Edit the prose sections when
  the shape of the site changes; do not hand-maintain the post lists.
- **`sitemap.xml`** — generated by `jekyll-sitemap`. Do not write it by hand.

`_config.yml` also carries `social.links`, which becomes the `sameAs` array in
the JSON-LD `Person`, and `logo`, which becomes the publisher logo.

**`url:` in `_config.yml` must stay `https://mileszs.com`.** It is not
cosmetic — it drives canonical URLs, `og:url`, the JSON-LD, every link in
`feed.xml`, and every entry in `sitemap.xml`. It was wrong for years and
poisoned all of the above.

## Verifying a change

There are no tests. After a change that touches config, layouts, or the
generated text files:

```bash
rm -rf _site && bundle exec jekyll build
```

Then spot-check the build output — this catches the failure modes that matter,
none of which surface as build errors:

```bash
grep -c "<loc>" _site/sitemap.xml                      # sitemap still populated
grep 'rel="canonical"' _site/about/index.html          # canonical points at mileszs.com
head -30 _site/llms.txt                                # Liquid actually rendered
grep -o 'https://[^"]*' _site/feed.xml | head          # feed URLs use the real domain
```

A missing layout, a broken Liquid tag in `llms.txt`, or a wrong `url:` will all
build "successfully" and produce quietly wrong output.

Two more worth running after touching `exclude:` or the Gemfile:

```bash
ls _site/Gemfile _site/Gemfile.lock     # must NOT exist
find _site -path "*vendor*"             # must be empty
```

To watch a deploy: `gh run watch $(gh run list --workflow=pages.yml --limit 1 --json databaseId --jq '.[0].databaseId')`
