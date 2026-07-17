# 📊 This Week — Posted + Scheduled

## Published in the last 7 days

```dataview
TABLE platform AS "Platform", publish_date AS "Published"
FROM "Content Pipeline"
WHERE status = "Published" AND publish_date >= date(today) - dur(7 days) AND publish_date <= date(today)
SORT publish_date DESC
```

## Landing in the next 14 days

```dataview
TABLE platform AS "Platform", publish_date AS "Landing"
FROM "Content Pipeline"
WHERE status = "Scheduled" AND publish_date >= date(today) AND publish_date <= date(today) + dur(14 days)
SORT publish_date ASC
```
