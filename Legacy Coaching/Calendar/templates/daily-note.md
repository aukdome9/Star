---
date: <% tp.file.title %>
tags:
  - daily
  - monastery-calendar
---
# <% tp.file.title %>

*You are not becoming. You are remembering.*

---

## 🗓️ Transmitting Today

```dataview
TABLE platform AS "Platform", content_type AS "Type", status AS "Status"
FROM "Content Pipeline"
WHERE publish_date = date(<% tp.file.title %>)
SORT platform ASC
```

---

## 🔗 Everything Connected to This Date

```dataview
LIST
FROM [[]]
WHERE file.name != this.file.name
```

---

## 📝 Notes / Energy / Ritual Context

- 

---

## ⚡ Quick Actions

- [ ] Review what's transmitting today
- [ ] Confirm nothing is colliding or crowding
- [ ] Log energy / observations above
