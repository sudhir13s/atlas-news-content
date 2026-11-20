# AGENTS.md — machine navigation guide

This repository is designed to be read and extended by AI agents with **no external context**.
Follow these steps.

## 1. Orient

1. Read [`meta.json`](meta.json) — the repo descriptor: `domain`, `schema_version`, the content
   `types`, and pointers to the docs and the registry.
2. Read [`schema/registry.json`](schema/registry.json) — the type map. For each `type`:
   - `folder` — where files of that type live
   - `layout` — `dated` types file records under `<folder>/<year>/<month>/`
   - `schema` — the JSON Schema that validates that type's frontmatter
   - `id_prefix` — the stable-ID namespace (`<type>/`)
   - `sections` — the required Markdown body sections, in order

## 2. Discover content

- **Glob**: read every `feed/**/*.md`, parse its YAML frontmatter, key on `id` / `type`.
- **By date**: list `feed/<year>/<month>/` for the period you need.
- **Newsletters**: read every `newsletters/*.md`; the issues of one are the `feed-item` records of
  kind `newsletter` whose `newsletter_source` is its `id`.

## 3. Read a file

- **Frontmatter** carries the structured data; parse it as YAML.
- **Body** of a feed item carries `Summary` and, except on a newsletter issue, `Why It Matters`;
  a newsletter source carries `Overview`.
- `url` is unique per feed item. A newsletter issue references its source through
  `newsletter_source`; no other record is referenced.

## 4. Validate

- Load the type's schema, `schema/feed-item.schema.json` or `schema/newsletter-source.schema.json`
  (shared `$defs` in `schema/_shared.schema.json`).
- Validate the frontmatter against it (any JSON-Schema draft-2020-12 validator).
- Check the file name equals `slug`, and `id` equals `<type>/<slug>`.
- Check `newsletter_source` names an existing `newsletter-source` record.

## 5. Add or edit content

1. Copy `templates/feed-item.template.md` or `templates/newsletter-source.template.md`.
2. Build a feed item's slug as described in [CONVENTIONS.md](CONVENTIONS.md) and place the file by
   its published date; a newsletter source's slug is its name in lowercase hyphenated ASCII.
3. Fill every frontmatter field and the required sections; validate.
4. Do not introduce code files — this repo is content-only.

## 6. Invariants (do not violate)

- `id` is immutable; renaming a slug is a breaking change (note it in `CHANGELOG.md`).
- No `.js` / `.ts` / `.mjs` / `package.json` / build scripts anywhere.
- Facts are copied from the source, never written by a model.
- Frontmatter must pass the type schema before a change is complete.
