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

## 3. Read a file

- **Frontmatter** carries the structured data; parse it as YAML.
- **Body** carries two sections: `Summary` and `Why It Matters`.
- `url` is unique per item. There are no cross-references to other records.

## 4. Validate

- Load `schema/feed-item.schema.json` (shared `$defs` in `schema/_shared.schema.json`).
- Validate the frontmatter against it (any JSON-Schema draft-2020-12 validator).
- Check the file name equals `slug`, and `id` equals `feed-item/<slug>`.

## 5. Add or edit content

1. Copy `templates/feed-item.template.md`.
2. Build the slug as described in [CONVENTIONS.md](CONVENTIONS.md) and place the file by its
   published date.
3. Fill every frontmatter field and both sections; validate.
4. Do not introduce code files — this repo is content-only.

## 6. Invariants (do not violate)

- `id` is immutable; renaming a slug is a breaking change (note it in `CHANGELOG.md`).
- No `.js` / `.ts` / `.mjs` / `package.json` / build scripts anywhere.
- Facts are copied from the source, never written by a model.
- Frontmatter must pass the type schema before a change is complete.
