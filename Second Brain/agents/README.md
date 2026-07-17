---
id: agents-root
title: Agent Connective Data
type: custom
namespace: second-brain
visibility: vault
summary: Pattern for binding agents to their relative content, cadence, skills, and memory inside Master.
tags: [agents, pattern, second-brain]
---

# Agent Connective Data

Each agent that lives in the Star System should own a folder under `Second Brain/agents/<agent-id>/`
so its knowledge is **part of Master**, editable in Obsidian or the dashboard, and discoverable
alongside the typed second-brain graph.

## Folder contract

```
agents/
  <agent-id>/
    README.md          # purpose, status, links into Master
    content/           # domain material (e.g. posts, scripts, research)
    cadence/           # schedule, posting rhythm, checklists
    skills/            # markdown skill briefs this agent uses
    memory/            # durable decisions, preferences, outcomes
    outputs/           # generated drafts / run logs
```

## Examples (not pre-built)

| Agent id   | Connective data points                          |
|------------|--------------------------------------------------|
| `x`        | posts, cadence, voice, analytics snapshots       |
| `ashley`   | debriefs, contacts, follow-ups                   |
| `monastery`| video pipeline notes, clip briefs                |

When you create an agent, copy `_template/` → `agents/<agent-id>/` and fill the folders.
Star Integration will treat those paths as first-class Master knowledge (same vault).

## Linking to typed nodes

Prefer wikilinks and typed `edges` from Infinite Brain nodes to agent folders:

- A `playbook` node may `related_to` `agents/x/cadence/weekly.md`
- A `contact` may `related_to` outreach content under an agent

This keeps **core knowledge** (pillars, decisions, concepts) and **agent operations** in one Master graph.
