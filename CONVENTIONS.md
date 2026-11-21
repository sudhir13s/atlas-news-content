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

- A newsletter source is a flat file `newsletters/<slug>.md`; its slug is the newsletter's name as
  lowercase ASCII words joined by hyphens.

## IDs

- `id` = `feed-item/<slug>` or `newsletter-source/<slug>`. Globally unique, **immutable**.
- A feed item of kind `newsletter` references its source in `newsletter_source`
  (`newsletter-source/<slug>`); the reference must resolve. No other feed item references a record.

## Frontmatter

Base fields on every file: `id`, `type`, `slug`, `schema_version`, `title`, `tags`, `updated`,
`sources`. Keys are `snake_case`; unknown keys fail validation.

| Field | Meaning |
|---|---|
| `kind` | `news`, `paper`, `repository` or `newsletter`; decided by the fetch tool, never by a model |
| `url` | Canonical address of the item; unique across the repo |
| `source` | Name of the entry in `config/sources.yaml` that delivered it |
| `source_tool` | The fetch tool that read it (`rss`, `arxiv`, `github`) |
| `newsletter_source` | Only on kind `newsletter`: `newsletter-source/<slug>`, the source the issue belongs to |
| `published_at` | UTC timestamp the source published it; `null` if unknown |
| `relevance` | Integer 0 to 10; `0` on a newsletter issue, which no one scored |
| `quote` | The passage of the source the summary rests on, word for word; absent on a newsletter issue |
| `credit` | Licence or credit line the source requires |
| `fields` | Values copied from the source (authors, stars, categories) |
| `review` | `score` (0 to 10, worked out by code from the Critic's verdicts), `reviewed_by` (`provider`, `model`), `prompt_versions` (`writer`, `critic`); absent on a newsletter issue |
| `generated_by` | `job`, `run_id`, and the writer's `provider`, `model`, `prompt_version`; a newsletter issue names the job and run only, because code wrote it |

- `tags`: lowercase, hyphen-separated, from the closed list in `config/taxonomy/feed-tags.yaml`.
- `updated`: ISO date the file was written.
- `sources[]`: one entry, the item's own `url` and `title`.
- Facts (`url`, `title`, `published_at`, `kind`, `fields`, `credit`) are copied by code from the
  source; a model wrote only the tags, relevance and the two sections.

## Newsletter issues

- An issue is a `feed-item` of kind `newsletter`, copied from the publisher's feed by code with no
  model: `title`, `url`, `published_at` and the feed's own short description as the `Summary`.
- The `Summary` is cut at 300 characters on a word boundary and is empty when the feed gives no
  description. The full issue text is never stored.
- `tags` is empty and `relevance` is `0`: no one ranked or tagged the issue.

## Papers copied by code

- A `feed-item` of kind `paper` whose `generated_by` names no model was copied from the paper
  source (arXiv, Hugging Face Papers) by code: `title`, `url`, `published_at`, `fields` (including
  the paper's `arxiv_id`, `authors` and any counts the source gives) and the abstract as the source
  gives it as the `Summary`.
- It has no `quote`, `review` or `Why It Matters`; `tags` is empty and `relevance` is `0`.
- Papers written by a model, which older files hold, keep every field of the other kinds.

## Newsletter sources

| Field | Meaning |
|---|---|
| `publisher` | The person or organisation that publishes the newsletter |
| `homepage` | The publisher's own page for the newsletter |
| `feed_url` | The public feed the publisher advertises; the job matches it to its `config/sources.yaml` entry |
| `cadence` | How often the publisher says it publishes; left out when it does not say |
| `language` | ISO 639-1 code of the issues |

- A source is added only when the publisher advertises a public feed, `robots.txt` allows reading
  it and the publisher's terms do not bar automated reading. The `Overview` is the author's own
  words, taken from the publisher's pages; anything the publisher does not state stays out.

## Body sections

A feed item has `##` sections, in this order:

1. `## Summary` — what the item says, one or two sentences.
2. `## Why It Matters` — why it matters to the AI ecosystem; the single word `unknown` is allowed
   when the source does not say. Optional: a newsletter issue has none.

A newsletter source has one section, `## Overview`.

## Sourcing

Open-source-first, zero plagiarism. The summary is original text; only the short `quote` is
verbatim, and it is checked against the source. Every item links back to its source and keeps its
credit line.
