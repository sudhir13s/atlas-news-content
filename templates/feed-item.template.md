---
id: feed-item/<YYYY-MM-DD>-<title-words>-<8-hex-of-url-hash>
type: feed-item
slug: <YYYY-MM-DD>-<title-words>-<8-hex-of-url-hash>
schema_version: 2.0               # the schema's current version; the schema refuses any other
title: "<Title of the item as the source published it>"
kind: news                        # news | newsletter
url: "<canonical address>"
source: "<name from config/sources.yaml>"
source_tool: rss                  # the fetch tool that read it; it decides the shape of `fields`
newsletter_source: newsletter-source/<slug>   # only on kind newsletter: the source the issue belongs to
series: "<series name>"           # only on kind newsletter, when the title matches a series the source lists; the title stays as published
series_issue: 25                  # only with a numbered (title_pattern) series: the number found in the title
published_at: "<YYYY-MM-DDTHH:MM:SS+00:00>"   # UTC; null when the source gave no date
relevance: 7                      # 0 to 10, a model's score; left out on a newsletter issue, which no one scored
quote: "<the passage of the source the summary rests on, word for word>"   # left out on a newsletter issue
credit: "<licence or credit line the source requires>"
fields:                           # what the rss tool read from the feed entry
  author: "<author>"              # left out when the feed names none
  categories: ["<category>"]      # [] when the feed lists none
review:                           # left out on a newsletter issue
  score: 10.0                     # the Critic's score, 0 to 10
  reviewed_by: { provider: "<provider>", model: "<model>" }
  prompt_versions: { writer: "<version>", critic: "<version>" }
generated_by:
  job: news-digest
  run_id: 1
  provider: "<provider>"          # provider, model and prompt_version are all left out when code wrote the record
  model: "<model>"
  prompt_version: "<version>"
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<canonical address>", title: "<title>" }
---

## Summary
<What the item says, in one or two sentences, every claim supported by the quote. On a newsletter issue: the feed's own short description, empty when the feed gives none.>

## Why It Matters
<Why a reader of the AI ecosystem should care, in one or two sentences. Left out on a newsletter issue.>
