---
id: atlas-weekly-issue/<YYYY>-w<WW>
type: atlas-weekly-issue
slug: <YYYY>-w<WW>
schema_version: 2.0               # the schema's current version; the schema refuses any other
title: "Atlas Weekly, week <WW> of <YYYY>"
week_start: '<Monday, YYYY-MM-DD>'
week_end: '<Sunday, YYYY-MM-DD>'
sections:
  - title: New in the Atlas
    total: <how many records the week added>
    items:
      - { title: "<record title>", record: "<type>/<slug>", group: "<library tab>" }
  - title: Headlines
    total: <how many items the week held>
    items:
      - { title: "<item title>", url: "<the item's page>", source: "<feed source>", note: "<the stored one-line summary>" }
generated_by:
  job: atlas-weekly
  run_id: <run number>
  provider: <provider>        # provider, model and prompt_version are left out when no introduction was written
  model: <model>
  prompt_version: <version>
tags: [atlas-weekly]
updated: <YYYY-MM-DD>
sources:
  - { url: "<an item's page>", title: "<item title>" }
---

## Introduction
<Three to five sentences from the week's facts, written by the Writer and passed by the Critic. Leave the whole section out when none was written.>
