# Changelog

All notable changes to `atlas-news-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

### Schema 2.0 (strict schemas, checked versions; breaking)

Every record was converted by the consumer's `atlas migrate news-restructure`. A step only
moved, renamed or deleted: no value was added, and `updated` did not move.

- **Changed** `schema_version` to a constant of each schema (`2.0`); a record with any other
  value is refused. The records said `1.0` or `1.1`.
- **Changed** to strict: no unknown field at the top level of `newsletter-source`, in
  `sources[]`, or inside a block of fixed shape (`feed-item` and `atlas-weekly-issue` already
  refused unknown top-level fields).
- **Changed** `feed-item.fields` from an open object to the shape of its `source_tool` (`rss`:
  `categories`, and `author` when the feed names one). The shared part of the type is
  `feed_item` in `_shared.schema.json`, identical in `atlas-research-content`.
- **Removed** `relevance: 0` from the 67 newsletter issues. Breaking: an issue code copied
  states no `relevance`; only a model's score is stored.
- **Removed** the empty `fields.author` of 15 items whose feed names no author.
- **Changed** `feed-item` to state its two sorts by `kind`: a `news` item requires `relevance`,
  `quote`, `review` and a `generated_by` that names a model; a `newsletter` issue requires
  `newsletter_source`, a `generated_by` with the job and run only, and none of the other three.
- **Changed** `review` and `generated_by` to shared definitions in `_shared.schema.json`, used
  by `feed-item` and `atlas-weekly-issue`.
- **Changed** `newsletter-source.cadence` to an enum (`daily`, `weekly`, `monthly`) and
  **added** `cadence_note` for a rhythm one word does not say. Latent Space's sentence moved
  to `cadence_note` word for word; `weekly` stays on two records.
- **Changed** the 8 `newsletter-source` files to the writer's one text form (a block list for
  `tags`); no value changed.
- **Kept** `credit` on every item: the site prints each listed item's credit line, and one
  source's line changed over time, so it cannot be read back from the source entry.
- **Kept** `source_tool` on every item: it selects the shape of `fields`.

### Added
- Numbered series (schema 1.4): a `series` entry may give a `title_pattern` (literal text with one
  `{number}` placeholder) instead of a `title_prefix`, and a numbered issue carries `series_issue`,
  the number found in its title. Interconnects lists `Latest open artifacts`.
- Named series (schema 1.3): `newsletter-source` takes an optional `series` list of
  `{name, title_prefix}`, and a `feed-item` of kind `newsletter` an optional `series` (the series'
  name, copied by code when the issue title starts with a listed prefix). The title stays as
  published. Latent Space lists its `AINews` series.
- The `newsletter-source` type (folder `newsletters/`): one record per newsletter the feed follows,
  with its publisher, homepage, feed address, stated cadence and language.
- Four `newsletter-source` records: Import AI, Interconnects, Ahead of AI, Latent Space.
- `feed-item` records of kind `newsletter` carry `newsletter_source`, the ID of the source they
  belong to. A newsletter issue is copied from the publisher's feed by code: it has no `quote`, no
  `review`, no `Why It Matters` section, and its `generated_by` names the job and run only.

- The `atlas-weekly-issue` type (folder `issues/`, schema 1.2): one record per ISO week, written by
  the `atlas-weekly` job. `sections` holds the week's added library records, headlines, papers and
  newsletter issues as linked lists; an optional `## Introduction` is the only model-written text.

### Removed
- `feed-item` kinds `paper` and `repository`. Recent papers moved to `atlas-research-content`
  (Research shows them) with their files unchanged; the repository items were deleted, because
  GitHub repositories are not news. `kind` is now `news` or `newsletter`, and the code-copied paper
  rules are gone from the schema.

### Changed
- `feed-item`: `quote`, `review` and `generated_by.provider`, `model`, `prompt_version` are required
  only for kinds other than `newsletter`; the `Why It Matters` section is optional.

## Schema versions

- **2.0** — strict schemas and a fixed version per type; typed `feed-item.fields`; no
  `relevance` on a newsletter issue; `newsletter-source.cadence` an enum, with `cadence_note`.
  Breaking: every record was converted.
- **1.4** — adds `title_pattern` to a `series` entry and `series_issue` to newsletter `feed-item`
  records. Existing records stay valid unchanged.
- **1.3** — adds the optional `series` of `newsletter-source` and of newsletter `feed-item`
  records. Existing records stay valid unchanged.
- **1.2** — adds the `atlas-weekly-issue` type. Existing records stay valid unchanged.
- **1.1** — adds the `newsletter-source` type and the `newsletter_source` field; relaxes `feed-item`
  for code-written newsletter issues. Existing records stay valid unchanged.
- **1.0** — initial schema for the `feed-item` type.

### Added (initial)
- Repository scaffold to the content-repo standard: `meta.json`, `schema/registry.json`, the
  `_shared` and `feed-item` JSON Schemas, a template, and docs (`README`, `AGENTS`, `CONVENTIONS`).
- The `feed-item` type, filed by published date under `feed/<year>/<month>/`.
