---
title: Star Dashboard
tags: [dashboard, star-operator, moc]
cssclasses: [dashboard]
updated: 2026-07-17
---

# Star Operator · Dashboard

> **Home base.** Open this note (or today's daily note) every session.  
> Continuous Grok edits this vault. You can also edit everything in Obsidian on desktop or via the GitHub mirror on phone.

## Today's focus — 2026-07-17

**Daily note:** [[Calendar/Daily/2026-07-17]]

### Tasks due today

```dataview
TASK
FROM "Calendar" OR "Second Brain" OR "The Sovereign" OR "Projects" OR "Dashboard"
WHERE !completed AND contains(text, "2026-07-17")
```

### All incomplete tasks

```dataview
TASK
WHERE !completed
SORT text ASC
LIMIT 50
```

### Recent daily notes

```dataview
TABLE WITHOUT ID
  file.link AS Day,
  file.mtime AS Updated
FROM "Calendar/Daily"
SORT file.name DESC
LIMIT 10
```

---

## Navigate

| Area | Link |
|------|------|
| Calendar system | [[Calendar/Calendar]] |
| Task inbox | [[Calendar/Tasks]] |
| Second Brain | [[Second Brain]] |
| Agent data | [[Second Brain/agents/README]] |
| Operator manual | [[STAR OPERATOR]] |
| Master map | [[Master Documentation]] |

## How the agent works

1. **Local continuous:** `start star` → dashboard chat → Grok with full tools on this vault  
2. **Obsidian:** edit any note; Daily Notes + Calendar plugin show scheduled work  
3. **Phone / online:** GitHub mirror of this vault (see `.star/MOBILE-GITHUB.md`)

### Task syntax (Obsidian Tasks-compatible)

```markdown
- [ ] Clear task title 📅 2026-07-17 🔗 [[Related Note]] 🎯 Outcome you unlock
```

- `📅 YYYY-MM-DD` — scheduled / due day (shows on calendar dots + dashboard)  
- `🔗 [[Note]]` — what it relates to  
- `🎯 …` — what completing it accomplishes  
