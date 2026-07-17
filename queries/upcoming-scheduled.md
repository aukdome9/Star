# 🗓️ Upcoming — Horizon View

Everything scheduled from today forward, across every platform, sorted by date.

```dataview
TABLE platform AS "Platform", content_type AS "Type", status AS "Status", publish_date AS "Date"
FROM "Content Pipeline"
WHERE publish_date >= date(today) AND status != "Idea"
SORT publish_date ASC
```
