# Changelog

All notable changes to `atlas-news-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

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
