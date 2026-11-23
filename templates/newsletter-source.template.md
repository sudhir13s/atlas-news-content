---
id: newsletter-source/<slug>
type: newsletter-source
slug: <slug>
schema_version: 2.0               # the schema's current version; the schema refuses any other
title: "<Name of the newsletter as the publisher writes it>"
publisher: "<person or organisation that publishes it>"
homepage: "<the publisher's own page for the newsletter>"
feed_url: "<the public RSS or Atom feed the publisher advertises>"
cadence: weekly                   # daily | weekly | monthly, as the publisher says; leave the line out when it does not say
cadence_note: "<the publisher's stated rhythm in a sentence>"   # only when one cadence word does not say it; then leave `cadence` out
language: en
series:                           # leave the block out when the newsletter publishes no named series
  - { name: "<series name as the publisher writes it>", title_prefix: "<the exact text its issue titles start with>" }
  # or, for a numbered series: { name: "<series name>", title_pattern: "<literal text with one {number}, e.g. Weekly digest #{number}:>" }
tags: [newsletter]
updated: <YYYY-MM-DD>
sources:
  - { url: "<the publisher's page the facts come from>", title: "<page title>" }
---

## Overview
<What the newsletter covers and what its feed lists, in your own words, every statement taken from the publisher's own pages. No copied marketing text.>
