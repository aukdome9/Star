---
date: {{date}}
tags: [daily, star-operator]
type: daily
---

# {{date}} · Daily Command

> Opened by Star Operator / Obsidian Daily Notes. Edit tasks here — they drive the dashboard and calendar.

## Intent (what today accomplishes)
- 

## Tasks for today
- [ ] 📅 {{date}}  #star 

## Related projects / notes
- 

## Outcomes log
- 

---

### Task board (live)

```dataview
TASK
FROM "Calendar" OR "Second Brain" OR "Dashboard"
WHERE !completed AND (contains(text, "📅 {{date}}") OR contains(text, "{{date}}"))
```

### All open tasks (vault-wide)

```dataview
TASK
WHERE !completed
LIMIT 40
```
