# Legacy Coaching Calendar

> This is the scheduling nerve center for all Aukdom Ethnine Legacy Coaching content. Every platform, every post, every date — tracked here and executed by the n8n automation layer.

**Open the full dashboard →** [[Content Hub]]  
**n8n integration contract →** [[n8n-manifest]]  
**Parent document →** [[Aukdom Ethnine Legacy Coaching.]]

---

## Platforms

| Platform    | Folder                          | Content Types                        |
|-------------|----------------------------------|--------------------------------------|
| LinkedIn    | [[Content Pipeline/LinkedIn/]]   | Articles, Posts, Carousels, Polls    |
| X           | [[Content Pipeline/X/]]          | Singles, Threads, Quote-Tweets, Articles |
| Instagram   | [[Content Pipeline/Instagram/]]  | Images, Carousels, Reels, Stories    |
| TikTok      | [[Content Pipeline/TikTok/]]     | Videos, Slideshows                   |
| YouTube     | [[Content Pipeline/YouTube/]]    | Longform, Shorts                     |
| Whop        | [[Content Pipeline/Whop/]]       | Announcements, Resources, Discussions|

---

## Upcoming (Next 14 Days)

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  content_type as "Type",
  publish_date as "Date",
  publish_time as "Time",
  post_status as "Status"
FROM "Content Pipeline"
WHERE publish_date >= date(today) AND publish_date <= date(today) + dur(14 days) AND post_status != "idea"
SORT publish_date asc, publish_time asc
```

---

## Content Velocity (Last 30 Days Published)

```dataview
TABLE WITHOUT ID
  file.link as "Content",
  platform as "Platform",
  publish_date as "Posted",
  n8n_post_id as "Link"
FROM "Content Pipeline"
WHERE post_status = "posted" AND publish_date >= date(today) - dur(30 days)
SORT publish_date desc
```

---

## Workflow

1. **Create** → Use a platform template from `Content Pipeline/Templates/`
2. **Write** → Fill in the body sections and frontmatter metadata
3. **Schedule** → Set `publish_date`, `publish_time`, and move `post_status` to `ready`
4. **Automate** → n8n reads the vault, matches `ready` notes to today's date, and posts
5. **Verify** → Check the Content Hub for `posted` or `failed` status updates
