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

- `id` = `feed-item/<slug>`, `newsletter-source/<slug>` or `atlas-weekly-issue/<slug>`. Globally
  unique, **immutable**.
- A feed item of kind `newsletter` references its source in `newsletter_source`
  (`newsletter-source/<slug>`); the reference must resolve. No other feed item references a record.

## Frontmatter

Base fields on every file: `id`, `type`, `slug`, `schema_version`, `title`, `tags`, `updated`,
`sources`. Keys are `snake_case`.

- `schema_version`: the version of the type's schema. Every schema fixes the value (`2.0`) and a
  record with any other is refused; a breaking change raises it and converts every record.
- **No unknown fields** on any type, at the top level or inside a block of fixed shape.
- **A value that is not known is absent.** No empty text, zero or placeholder stands in for it.
- `review` and `generated_by` are defined once, in `schema/_shared.schema.json`, for every type
  that carries them.

A feed item's own fields:

| Field | Meaning |
|---|---|
| `kind` | `news` or `newsletter`; decided by the fetch tool, never by a model |
| `url` | Canonical address of the item; unique across the repo |
| `source` | Name of the entry in `config/sources.yaml` that delivered it |
| `source_tool` | The fetch tool that read it (`rss`); it decides the shape of `fields` |
| `newsletter_source` | Only on kind `newsletter`: `newsletter-source/<slug>`, the source the issue belongs to |
| `series` | Only on kind `newsletter`, and only when the title starts with a `title_prefix` of the source's `series` list or matches its `title_pattern`: that series' `name`, copied by code; the `title` itself stays as published |
| `series_issue` | Only with `series`, and only for a `title_pattern` series: the issue number the pattern found in the title, copied by code |
| `published_at` | UTC timestamp the source published it; `null` if unknown |
| `relevance` | Integer 0 to 10, a model's score; absent on a newsletter issue, which no one scored |
| `quote` | The passage of the source the summary rests on, word for word; absent on a newsletter issue |
| `credit` | Licence or credit line the source requires, as its `config/sources.yaml` entry stated it when the item was read |
| `fields` | What the `rss` tool read from the feed entry: `categories` (a list, empty when the feed lists none) and `author` (absent when the feed names none); no other key |
| `review` | `score` (0 to 10, worked out by code from the Critic's verdicts), `reviewed_by` (`provider`, `model`), `prompt_versions` (`writer`, `critic`); absent on a newsletter issue |
| `generated_by` | `job` and `run_id`; a news item adds the writer's `provider`, `model` and `prompt_version` (all three), a newsletter issue none of them, because code wrote it |

- **The type is shared with `atlas-research-content`**, which holds the papers. Everything but
  `kind` and the newsletter fields is defined by `feed_item` in `schema/_shared.schema.json`, and
  that definition and those it refers to are kept word for word the same in both repositories.
- `kind` decides the rest: a `news` item states `relevance`, `quote` and `review` and names a
  model in `generated_by`; a `newsletter` issue states none of them and names its
  `newsletter_source`.

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
- `tags` is empty and there is no `relevance`: no one ranked or tagged the issue.

## Papers and repositories

- This repository holds no papers and no repositories: recent papers live in
  `atlas-research-content` (Research shows them), and GitHub repositories are not news.

## Newsletter sources

| Field | Meaning |
|---|---|
| `publisher` | The person or organisation that publishes the newsletter |
| `homepage` | The publisher's own page for the newsletter |
| `feed_url` | The public feed the publisher advertises; the job matches it to its `config/sources.yaml` entry |
| `cadence` | How often the publisher says it publishes, as one word: `daily`, `weekly` or `monthly`; left out when it does not say or one word does not say it |
| `cadence_note` | The publisher's stated rhythm in a sentence, only when one cadence word does not say it (a daily briefing beside irregular articles); `cadence` is then left out |
| `language` | ISO 639-1 code of the issues |
| `series` | Optional list of `{name, title_prefix}` or `{name, title_pattern}` (exactly one of the two): a named series the newsletter publishes under a fixed title prefix (`{name: AINews, title_prefix: "[AINews]"}`) or under a numbered title (`{name: Latest open artifacts, title_pattern: "Latest open artifacts (#{number}):"}`); left out when it has none |

- A `title_pattern` is literal text with exactly one `{number}` placeholder, matched from the start
  of the title; the number is digits. It is not a regular expression and nothing else in it is
  special.

- A `series` entry is added only for a recurring, named series the publisher itself runs, seen in
  the issue titles of its own feed. A newsletter's own name or issue number (`Import AI 480:`) and
  a one-off label (`[Special]`, `Podcast:`) are not series.

- A source is added only when the publisher advertises a public feed, `robots.txt` allows reading
  it and the publisher's terms do not bar automated reading. The `Overview` is the author's own
  words, taken from the publisher's pages; anything the publisher does not state stays out.

## Atlas Weekly issues

- An issue is a flat file `issues/<YYYY>-w<WW>.md`; its slug names the ISO week it covers, so one
  week always maps to one file and a rerun never duplicates it. `id` = `atlas-weekly-issue/<slug>`.
- `week_start` (Monday) and `week_end` (Sunday) are dates. `sections` is a list of `{title, total,
  items}`; `total` is how many the week held before the list was cut, and each item is `{title,
  url or record, source, note, group}` copied by code from the feed or the library.
- An item with `record` points at a library record (`<type>/<slug>`) in another repository; one
  with `url` points outside.
- A model writes only the optional `## Introduction`. Its `generated_by` then adds `provider`,
  `model` and `prompt_version`; without an introduction `generated_by` names the job and run only.
- An issue is written once for its week and not rewritten; a week with no additions and no items
  has no issue.

## Body sections

A feed item has `##` sections, in this order:

1. `## Summary` — what the item says, one or two sentences.
2. `## Why It Matters` — why it matters to the AI ecosystem; the single word `unknown` is allowed
   when the source does not say. Optional: a newsletter issue has none.

A newsletter source has one section, `## Overview`. An Atlas Weekly issue has the optional
`## Introduction` and nothing else.

## Sourcing

Open-source-first, zero plagiarism. The summary is original text; only the short `quote` is
verbatim, and it is checked against the source. Every item links back to its source and keeps its
credit line.
