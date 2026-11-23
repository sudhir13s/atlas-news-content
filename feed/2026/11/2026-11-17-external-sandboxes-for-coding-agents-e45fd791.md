---
id: feed-item/2026-11-17-external-sandboxes-for-coding-agents-e45fd791
type: feed-item
slug: 2026-11-17-external-sandboxes-for-coding-agents-e45fd791
schema_version: 2.0
title: External sandboxes for coding agents
kind: news
url: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents
source: Simon Willison
source_tool: rss
published_at: '2026-11-17T22:52:29+00:00'
relevance: 7
quote: My post on the Prime Radiant blog introduces the external sandbox pattern, where code executed by the agent runs inside a dedicated sandbox while the agent itself lives outside of that sandbox.
credit: Public feed; link back to the original post and credit Simon Willison.
fields:
  categories:
  - sandboxing
  - security
  - ai
  - webassembly
  - generative-ai
  - llms
  - coding-agents
  - claude-code
  - codex
  - pi
review:
  score: 10.0
  reviewed_by:
    provider: nvidia-gemma-4-31b
    model: nvidia_nim/google/gemma-4-31b-it
  prompt_versions:
    writer: 2.1.0
    critic: 1.1.0
generated_by:
  job: news-digest
  run_id: 4
  provider: gemini-flash-lite
  model: gemini/gemini-3.5-flash-lite
  prompt_version: 2.1.0
tags:
- agents
- tooling
- safety
updated: '2026-11-19'
sources:
- url: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents
  title: External sandboxes for coding agents
---

## Summary

The post introduces the external sandbox pattern for coding agents, where code executed by the agent runs in a dedicated sandbox while the agent remains outside. Three sandbox server implementations have been released using v86, Apple Containers, and Quicksand. These sandboxes include an optional HTTP/HTTPS allow-list proxy and credential replacement mechanism.

## Why It Matters

unknown
