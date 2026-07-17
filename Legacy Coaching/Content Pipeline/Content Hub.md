# 📅 Content Hub — Ground Zero

> [!NOTE]
> This is the central command center for all Legacy Coaching content across LinkedIn, X, Instagram, TikTok, YouTube, and Whop. Every content note's YAML frontmatter is the data layer — what you see below is a live query of your vault. This same data is what your n8n agent reads to auto-post.

**Quick Links:** [[n8n-manifest]] | [[Legacy Coaching Calendar]] | [[Posting Cadence]] | [[30-Day Content Cycle — Cycle 1 (Jul 19–Aug 17)]]

---

## 🗂️ By Series / Topic

The reverse path into the same data: pick a series first, then a platform. Today this is Tri-Union Series → X; more series/platform combinations land here as they're built.

| Topic | Platform | Content |
|---|---|---|
| [[Tri-Union Series]] | X (Twitter) | [[Tri-Union Series — X Hub]] |

---

## 🕸️ The Connective Tissue (Network Strategy)
This map ensures nothing leads to a dead end. Watch how your content chains together toward the community.

```dataview
TABLE WITHOUT ID
  file.link as "Post",
  marketing_stage as "Stage",
  relation_type as "Action",
  relates_to as "Leads Back To"
FROM "Content Pipeline"
WHERE relates_to != null AND relates_to != ""
SORT file.ctime desc
```

---

## 🔴 Today's Queue — Ready to Post

These notes have `post_status: ready` and `publish_date` set to today or earlier. **Your n8n agent is watching these.**

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  content_type as "Type",
  publish_time as "Time (CST)",
  post_status as "Status"
FROM "Content Pipeline"
WHERE post_status = "ready" AND publish_date <= date(today)
SORT publish_time asc
```

---

## ⚠️ Failed Posts — Needs Attention

Posts that n8n attempted but could not publish. Check `n8n_error` in the note's frontmatter.

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  publish_date as "Date",
  n8n_error as "Error"
FROM "Content Pipeline"
WHERE post_status = "failed"
SORT publish_date asc
```

---

## 🗓️ This Week's Calendar

Everything scheduled for the next 7 days, grouped by date.

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  content_type as "Type",
  publish_time as "Time (CST)",
  post_status as "Status"
FROM "Content Pipeline"
WHERE publish_date >= date(today) AND publish_date <= date(today) + dur(7 days) AND post_status != "posted" AND post_status != "idea"
SORT publish_date asc, publish_time asc
GROUP BY publish_date
```

---

## 📊 Pipeline Status

```dataviewjs
const pages = dv.pages('"Content Pipeline"').where(p => p.post_status);
const statuses = ["idea", "draft", "review", "ready", "posted", "failed"];
const rows = statuses.map(s => [
  s.toUpperCase(),
  pages.where(p => p.post_status === s).length
]);
dv.table(["Status", "Count"], rows);
```

---

## 📱 By Platform

### LinkedIn
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/LinkedIn"
WHERE post_status != "posted"
SORT publish_date asc
```

### X (Twitter)
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/X"
WHERE post_status != "posted"
SORT publish_date asc
```

### Instagram
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/Instagram"
WHERE post_status != "posted"
SORT publish_date asc
```

### TikTok
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/TikTok"
WHERE post_status != "posted"
SORT publish_date asc
```

### YouTube
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/YouTube"
WHERE post_status != "posted"
SORT publish_date asc
```

### Whop Community
```dataview
TABLE WITHOUT ID
  file.link as "Content",
  content_type as "Type",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline/Whop"
WHERE post_status != "posted"
SORT publish_date asc
```

---

## ✅ Recently Posted (Last 15)

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  publish_date as "Posted On",
  n8n_post_id as "Post ID/URL"
FROM "Content Pipeline"
WHERE post_status = "posted"
SORT n8n_posted_at desc
LIMIT 15
```

---

## 📈 Upcoming Horizon (Next 30 Days)

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  content_type as "Type",
  publish_date as "Date",
  post_status as "Status"
FROM "Content Pipeline"
WHERE publish_date >= date(today) AND publish_date <= date(today) + dur(30 days)
SORT publish_date asc
```
