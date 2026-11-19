# atlas-news-content

The **AI news feed** of TheAIAtlas as portable records: news items, papers and repositories, each
ranked, tagged and summarized, with the quote its summary rests on and the score of its review.
Authored as Markdown with structured YAML frontmatter, validated by JSON Schema, readable by
humans and AI agents. This repo is **content only**: no code, no build step.

- **Written by a scheduled job**, the `news-digest` job of TheAIAtlas. It commits and pushes
  here; no one approves items by hand.
- **Rebuildable:** a consumer database is a copy of these files and can always be rebuilt from them.
- **Traceable:** every file names the job run, model and prompts that produced it, and every
  job commit carries the same facts as trailers, so a run can be reverted as one commit.

It is the `news` domain of a multi-repo reference library
(`atlas-models-content`, `atlas-evaluation-content`, `atlas-frameworks-content`,
`atlas-hardware-content`, `atlas-news-content`), all following the same
[content-repo standard](CONTENT-STANDARD.md).

## What's inside

| Type | Folder | What it captures |
|------|--------|------------------|
| `feed-item` | `feed/<year>/<month>/` | One news item, paper, repository (later: newsletter issue): source facts copied by code, plus a summary, why it matters, the supporting quote, relevance, review score and provenance |

## Quick start

```bash
# Vendor it
git clone https://github.com/sudhir13s/atlas-news-content.git
# or as a submodule of your project
git submodule add https://github.com/sudhir13s/atlas-news-content.git content/news-src
```

Every file is Markdown with YAML frontmatter (the structured fields) and a body of two sections
(`Summary`, `Why It Matters`). Enumerate the feed by globbing `feed/**/*.md`, or start from
[`meta.json`](meta.json) and [`schema/registry.json`](schema/registry.json).

## Structure

```
meta.json                 # repo descriptor (machine entrypoint 1)
schema/registry.json      # type → folder/layout/schema/sections map (machine entrypoint 2)
schema/*.schema.json      # JSON Schema per content type
templates/*.template.md   # copy these to author a record by hand
feed/<year>/<month>/      # one file per item, filed by published date; `feed/undated/` if none
assets/                   # SVG diagrams
```

## Contributing

See [CONVENTIONS.md](CONVENTIONS.md) for the rules and [AGENTS.md](AGENTS.md) for the
machine-navigation guide. Items come from the job; a hand-written file follows the same
template and must pass the same schema.

## License

Content is licensed **CC-BY-4.0** (see [LICENSE](LICENSE)) — reuse freely with attribution. Each
item keeps its own `credit` line, which names what its original source requires.
