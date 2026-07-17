---
title: "Tri-Union Series — X Hub"
type: platform-content-hub
topic: "[[Tri-Union Series]]"
platform: X
status: active
filed: 2026-07-16
tags: [platform-content-hub, legacy-coaching, tri-union-series, x]
---

# Tri-Union Series — X (Twitter) Hub

> [!NOTE]
> This is the **single node** for every piece of X content that belongs to the [[Tri-Union Series]] — every article, thread, and tweet, across all three books and nine principles, in one place. Every note below carries the `tri-union-series` tag; this page is just a live query of that tag, scoped to the X folder. Nothing here needs to be manually maintained — write a new note with the tag, and it appears below automatically.

**Path to here:** [[Tri-Union Series]] → **X** → *(this page is "Content")*
**Other platforms:** see [[Content Hub]] for LinkedIn, Instagram, TikTok, YouTube, and Whop.

---

## At a Glance

```dataviewjs
const pages = dv.pages('"Content Pipeline/X"').where(p => p.tags && p.tags.includes("tri-union-series"));
const articles = pages.where(p => p.content_type === "article");
const threads = pages.where(p => p.content_type === "thread");
const xp = pages.where(p => p.tags && p.tags.includes("cross-pollination"));
const standalone = pages.where(p => p.content_type === "single" && !(p.tags && p.tags.includes("cross-pollination")));

dv.table(
  ["Type", "Count"],
  [
    ["Articles", articles.length],
    ["Threads", threads.length],
    ["Cross-Pollination Tweets", xp.length],
    ["Standalone Tweets", standalone.length],
    ["**Total**", pages.length],
  ]
);
```

---

## Articles

Long-form doctrine pieces — the root node for each principle's cluster.

```dataview
TABLE WITHOUT ID
  file.link as "Article",
  principle as "Principle",
  publish_date as "Date",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE contains(tags, "tri-union-series") AND content_type = "article"
SORT publish_date asc
```

---

## Threads

Awareness-stage threads, including the two series-level capstone threads that span all nine principles.

```dataview
TABLE WITHOUT ID
  file.link as "Thread",
  principle as "Principle",
  publish_date as "Date",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE contains(tags, "tri-union-series") AND content_type = "thread"
SORT publish_date asc
```

---

## Cross-Pollination Tweets

Every tweet that deliberately bridges two principles (or two books) rather than staying inside one. This is the connective tissue of the whole series — grouped by home principle so you can see each principle's full set of outward bridges at a glance.

```dataview
TABLE WITHOUT ID
  file.link as "Tweet",
  principle as "Home",
  secondary_principle as "Bridges To",
  publish_date as "Date",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE contains(tags, "cross-pollination")
SORT principle asc, publish_date asc
```

---

## Standalone Tweets (by Principle)

Single tweets that belong to one principle only — reframes, self-audit prompts, key quotes, and conversion closes.

```dataview
TABLE WITHOUT ID
  file.link as "Tweet",
  principle as "Principle",
  publish_date as "Date",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE contains(tags, "tri-union-series") AND content_type = "single" AND !contains(tags, "cross-pollination")
SORT principle asc, publish_date asc
```

---

## Everything, Chronologically

The full feed — every article, thread, and tweet in the series, in posting order. This is the "show me all of it" view.

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  principle as "Principle",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE contains(tags, "tri-union-series")
SORT publish_date asc, publish_time asc
```

---

## By Book

### Dimensional Walking

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  publish_date as "Date"
FROM "Content Pipeline/X"
WHERE contains(tags, "dimensional-walking")
SORT publish_date asc
```

### Dimensional Reconstructing

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  publish_date as "Date"
FROM "Content Pipeline/X"
WHERE contains(tags, "dimensional-reconstructing")
SORT publish_date asc
```

### Reality Architecting

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  publish_date as "Date"
FROM "Content Pipeline/X"
WHERE contains(tags, "reality-architecting")
SORT publish_date asc
```

---

## By Cycle (90-Day Sprint)

```dataviewjs
const pages = dv.pages('"Content Pipeline/X"').where(p => p.tags && p.tags.includes("tri-union-series"));
const c1 = pages.where(p => p.publish_date && p.publish_date >= dv.date("2026-07-19") && p.publish_date <= dv.date("2026-08-17"));
const c2 = pages.where(p => p.publish_date && p.publish_date >= dv.date("2026-08-18") && p.publish_date <= dv.date("2026-09-16"));
const c3 = pages.where(p => p.publish_date && p.publish_date >= dv.date("2026-09-17") && p.publish_date <= dv.date("2026-10-16"));

dv.table(
  ["Cycle", "Window", "Count"],
  [
    ["Cycle 1", "Jul 19 – Aug 17", c1.length],
    ["Cycle 2", "Aug 18 – Sep 16", c2.length],
    ["Cycle 3", "Sep 17 – Oct 16", c3.length],
  ]
);
```

Full weekly breakdowns: [[30-Day Content Cycle — Cycle 1 (Jul 19–Aug 17)]] · [[30-Day Content Cycle — Cycle 2 (Aug 18–Sep 16)]] · [[30-Day Content Cycle — Cycle 3 (Sep 17–Oct 16)]]

---

## Status Board

```dataviewjs
const pages = dv.pages('"Content Pipeline/X"').where(p => p.tags && p.tags.includes("tri-union-series"));
const statuses = ["idea", "draft", "review", "ready", "posted", "failed"];
const rows = statuses.map(s => [s.toUpperCase(), pages.where(p => p.post_status === s).length]);
dv.table(["Status", "Count"], rows);
```

---
## Notes
Built as the "single node" navigation layer requested for the series: [[Tri-Union Series]] → **X** → this page → every article/thread/tweet. Every content note in `Content Pipeline/X` that carries the `tri-union-series` tag surfaces here automatically — no manual list-keeping. The 50 pre-existing X notes were retrofitted with the tag; every new Cycle 2/3 note carries it from creation. If a future note is written for this series and forgets the tag, it simply won't appear here — the tag is the single point of failure by design, matching the vault's "frontmatter is the single source of truth" philosophy from [[README]].
