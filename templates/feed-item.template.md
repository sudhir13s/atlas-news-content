---
id: feed-item/<YYYY-MM-DD>-<title-words>-<8-hex-of-url-hash>
type: feed-item
slug: <YYYY-MM-DD>-<title-words>-<8-hex-of-url-hash>
schema_version: 1.0
title: "<Title of the item as the source published it>"
kind: news                        # news | paper | repository | newsletter
url: "<canonical address>"
source: "<name from config/sources.yaml>"
source_tool: rss                  # the fetch tool that read it: rss | arxiv | github
published_at: "<YYYY-MM-DDTHH:MM:SS+00:00>"   # UTC; null when the source gave no date
relevance: 7                      # 0 to 10
quote: "<the passage of the source the summary rests on, word for word>"
credit: "<licence or credit line the source requires>"
fields: {}                        # values copied from the source (authors, stars, categories)
review:
  score: 10.0                     # the Critic's score, 0 to 10
  reviewed_by: { provider: "<provider>", model: "<model>" }
  prompt_versions: { writer: "<version>", critic: "<version>" }
generated_by:
  job: news-digest
  run_id: 1
  provider: "<provider>"
  model: "<model>"
  prompt_version: "<version>"
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<canonical address>", title: "<title>" }
---

## Summary
<What the item says, in one or two sentences, every claim supported by the quote.>

## Why It Matters
<Why a reader of the AI ecosystem should care, in one or two sentences.>
