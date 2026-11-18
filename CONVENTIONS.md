# CONVENTIONS.md — authoring rules

The rules every file follows. Humans and agents follow the identical process. The authoritative
cross-repo spec is the
[content-repo standard](https://github.com/sudhir13s/TheAIAtlas/blob/main/docs/content-repo-standard.md);
this file is the `news`-domain instance.

## Slugs, file names and folders

- A feed item's slug is `<YYYY-MM-DD>-<title-words>-<8 hex>`:
  - the **UTC day the source published it** (`undated` when the source gave no date);
  - the title as lowercase ASCII words joined by hyphens, cut to 60 characters;
  - the first 8 hex characters of the SHA-256 of the item's canonical `url`.
- The same item therefore always gets the same slug and file; a rerun never duplicates it.
- The file name is `<slug>.md` and **must equal** the `slug` field.
- Files live at `feed/<year>/<month>/<slug>.md` by published date (`feed/undated/` without one).
  The registry marks the type `"layout": "dated"`; a month stays far below GitHub's advised
  limit of 3,000 entries per folder.

## IDs

- `id` = `feed-item/<slug>`. Globally unique, **immutable**.
- A feed item references no other record today, so it has no cross-reference fields.

## Frontmatter

Base fields on every file: `id`, `type`, `slug`, `schema_version`, `title`, `tags`, `updated`,
`sources`. Keys are `snake_case`; unknown keys fail validation.

| Field | Meaning |
|---|---|
| `kind` | `news`, `paper`, `repository` or `newsletter`; decided by the fetch tool, never by a model |
| `url` | Canonical address of the item; unique across the repo |
| `source` | Name of the entry in `config/sources.yaml` that delivered it |
| `source_tool` | The fetch tool that read it (`rss`, `arxiv`, `github`) |
| `published_at` | UTC timestamp the source published it; `null` if unknown |
| `relevance` | Integer 0 to 10 |
| `quote` | The passage of the source the summary rests on, word for word |
| `credit` | Licence or credit line the source requires |
| `fields` | Values copied from the source (authors, stars, categories) |
| `review` | `score` (0 to 10, worked out by code from the Critic's verdicts), `reviewed_by` (`provider`, `model`), `prompt_versions` (`writer`, `critic`) |
| `generated_by` | `job`, `run_id`, `provider`, `model`, `prompt_version` of the writer |

- `tags`: lowercase, hyphen-separated, from the closed list in `config/taxonomy/feed-tags.yaml`.
- `updated`: ISO date the file was written.
- `sources[]`: one entry, the item's own `url` and `title`.
- Facts (`url`, `title`, `published_at`, `kind`, `fields`, `credit`) are copied by code from the
  source; a model wrote only the tags, relevance and the two sections.

## Body sections

Two `##` sections, in this order:

1. `## Summary` — what the item says, one or two sentences.
2. `## Why It Matters` — why it matters to the AI ecosystem; the single word `unknown` is allowed
   when the source does not say.

## Sourcing

Open-source-first, zero plagiarism. The summary is original text; only the short `quote` is
verbatim, and it is checked against the source. Every item links back to its source and keeps its
credit line.
