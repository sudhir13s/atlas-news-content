# Changelog

All notable changes to `atlas-news-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

### Added
- The `newsletter-source` type (folder `newsletters/`): one record per newsletter the feed follows,
  with its publisher, homepage, feed address, stated cadence and language.
- Four `newsletter-source` records: Import AI, Interconnects, Ahead of AI, Latent Space.
- `feed-item` records of kind `newsletter` carry `newsletter_source`, the ID of the source they
  belong to. A newsletter issue is copied from the publisher's feed by code: it has no `quote`, no
  `review`, no `Why It Matters` section, and its `generated_by` names the job and run only.

### Removed
- `feed-item` kinds `paper` and `repository`. Recent papers moved to `atlas-research-content`
  (Research shows them) with their files unchanged; the repository items were deleted, because
  GitHub repositories are not news. `kind` is now `news` or `newsletter`, and the code-copied paper
  rules are gone from the schema.

### Changed
- `feed-item`: `quote`, `review` and `generated_by.provider`, `model`, `prompt_version` are required
  only for kinds other than `newsletter`; the `Why It Matters` section is optional.

## Schema versions

- **1.1** — adds the `newsletter-source` type and the `newsletter_source` field; relaxes `feed-item`
  for code-written newsletter issues. Existing records stay valid unchanged.
- **1.0** — initial schema for the `feed-item` type.

### Added (initial)
- Repository scaffold to the content-repo standard: `meta.json`, `schema/registry.json`, the
  `_shared` and `feed-item` JSON Schemas, a template, and docs (`README`, `AGENTS`, `CONVENTIONS`).
- The `feed-item` type, filed by published date under `feed/<year>/<month>/`.
