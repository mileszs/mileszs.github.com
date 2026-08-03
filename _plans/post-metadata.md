# Plan: post front-matter metadata

Deferred follow-up work on the 99 files in `_posts/`. Intentionally kept out of
the AI-readiness commit so the diff stays reviewable.

Directories starting with `_` that Jekyll does not recognise are ignored by the
build, so this file is not published.

## Hard rules

1. **Never rename a post file.** `permalink: /:title` means the filename slug
   *is* the public URL. Renaming breaks every inbound link and every entry
   already in Google's index. All fixes below are front-matter-only.
2. **Do not revise post bodies.** These are historical. Fixing metadata is in
   scope; rewriting 2008 opinions about Ubuntu is not.
3. Rebuild and diff `_site/sitemap.xml` before and after. The URL list must be
   **identical**. If it changed, a filename moved and the change is wrong.

---

## Phase 1 — Repair 5 truncated titles (do this first)

An old Octopress→Jekyll migration truncated titles at the first `.` character.
The full title survives in the filename slug, where `.` was encoded as `-dot-`.
This is recoverable data loss and it is the highest-value fix here: these five
posts currently show meaningless titles in search results, in the archive, in
`feed.xml`, and in `llms.txt`.

| File | Current `title:` | Proposed `title:` |
|---|---|---|
| `2008-01-03-bikesofindy-dot-com-launched.md` | `BikesOfIndy` | `BikesOfIndy.com Launched` |
| `2008-01-03-stripegenerator-dot-com:-good-tool-for-a-non-designer.md` | `StripeGenerator` | `StripeGenerator.com: Good Tool for a Non-Designer` |
| `2008-01-08-ubuntu-8-dot-04-alpha-vmware-image.md` | `Ubuntu 8` | `Ubuntu 8.04 Alpha VMware Image` |
| `2008-01-10-torrent-available-ubuntu-8-dot-04-alpha-2-vmware-image.md` | `Torrent Available for Ubuntu 8` | `Torrent Available for Ubuntu 8.04 Alpha 2 VMware Image` |
| `2008-04-29-vim-rails-dot-vim-seg-faults-in-ubuntu-8-dot-04-hardy-heron-.md` | `Vim + rails.vim Seg-Faults in Ubuntu 8` | `Vim + rails.vim Seg-Faults in Ubuntu 8.04 (Hardy Heron)` |

Proposed titles are reconstructed from the slugs and should be sanity-checked
against each post's body before applying — the body usually names the software
version explicitly.

Titles containing `:` **must be quoted** in YAML.

Detection script (re-run afterwards; it should report 1 remaining, the benign
`2014-03-21-update-on-my-goals-for-2014` wording difference):

```bash
ruby -ryaml -e '
Dir["_posts/*"].sort.each do |f|
  raw = File.read(f); next unless raw =~ /\A---\s*\n(.*?)\n---\s*\n/m
  fm = YAML.safe_load($1, permitted_classes: [Date, Time]) rescue nil
  next unless fm && fm["title"]
  slug = File.basename(f).sub(/\A\d{4}-\d{2}-\d{2}-/, "").sub(/\.(md|markdown)\z/, "")
  t = fm["title"].to_s.downcase.gsub(/[^a-z0-9]+/, " ").split
  s = slug.downcase.gsub(/-dot-/, " ").gsub(/[^a-z0-9]+/, " ").split
  extra = s - t
  puts "#{File.basename(f)}: #{fm["title"].inspect} dropped #{extra.join(",")}" if extra.size >= 2
end'
```

**Commit on its own**, separate from Phases 2 and 3.

---

## Phase 2 — `description:` front matter

### Why it is not urgent

`jekyll-seo-tag` already falls back to the post's excerpt (first paragraph), so
every post gets a `<meta name="description">` and a JSON-LD `description`
today. This phase raises quality, it does not fix a break.

### Where it actually matters

Ten posts have a first paragraph shorter than 80 characters, four of which are
effectively empty because the post opens with an image or a code block. These
are the posts whose snippets are currently useless:

```
2015-09-02-opening-the-gold-rush-dungeon.markdown          0 chars
2008-07-10-too-many-choices.md                             0 chars
2008-06-10-thor-sake-and-rails-log-stats.md                0 chars
2008-06-10-rake-rails-log-stats.md                         0 chars
2008-06-07-my-favorite-rails-tips.md                       0 chars
2008-01-09-please-just-stop-it.md                         47 chars
2011-07-21-deprecation-warnings-for-proxyoner-...          52 chars
2009-06-17-wicked-pdf-plugin.markdown                     71 chars
2017-08-28-gencon-2017-recap.markdown                     73 chars
2020-08-18-handful-of-books.markdown                      76 chars
```

### Recommended scope

**Tier 1 (10 posts)** — hand-write a `description:` for the list above. Small,
high value, quick to review.

**Tier 2 (~25 posts)** — the posts that still get traffic or that Miles would
point someone at: the management/leadership posts, the books recaps, the Gen Con
recaps, `my-racism`, `back-to-blogging`. Hand-written.

**Tier 3 (remaining ~64)** — leave to the excerpt fallback. Auto-generating
descriptions for 2008 Rails posts is churn with no readership behind it.

Recommendation: **do Tier 1 and Tier 2, skip Tier 3.** A 99-file diff of
machine-written summaries is hard to review and easy to get subtly wrong.

Style: one or two sentences, under ~160 characters, written in Miles's voice,
not a summary of the summary. Quote any value containing `:`.

---

## Phase 3 — Tags and categories

The largest and least certain phase. Do it only if the goal is browsable topic
pages, not merely "more metadata".

### Current state

Old posts carry an empty `categories:` key inherited from Octopress. Newer posts
have no taxonomy at all. Nothing in the site renders tags today.

### Proposed taxonomy

Deliberately small — a dozen tags that each cover several posts beats forty that
cover one each:

`ruby`, `rails`, `emacs`, `vim`, `git`, `tools`, `leadership`, `books`,
`tabletop`, `gencon`, `music`, `personal`, `goals`, `meta`

### Work required

Tags alone change nothing user-visible. To be worth doing, this phase also needs:

1. `_layouts/tag.html` and one stub page per tag under `tags/` — GitHub Pages
   runs Jekyll in `--safe` mode, so **`jekyll-tagging` and similar generator
   plugins will not run**. Tag pages must be checked in as real files, or built
   by iterating `site.tags` on a single page.
2. Tag links in `_layouts/post.html`.
3. Styling in `_sass/tale/`.
4. A line in `CLAUDE.md` telling future contributors to add the stub file when
   they introduce a new tag.

### Recommendation

Defer. The `/archive/` page added in the AI-readiness commit already solves the
practical discovery problem — every post reachable in one click, grouped by
year. Tag pages are a nice-to-have on a blog that publishes roughly once a year,
and step 1 is a genuine maintenance burden under `--safe` mode.

---

## Suggested commit sequence

1. `Restore post titles truncated by the Octopress migration` — Phase 1.
2. `Add descriptions to posts with unusable excerpts` — Phase 2, Tier 1.
3. `Add descriptions to frequently-linked posts` — Phase 2, Tier 2.
4. Phase 3 only if tag pages are actually wanted.

After each: `rm -rf _site && bundle exec jekyll build`, then confirm
`grep -c "<loc>" _site/sitemap.xml` still reports the same count.
