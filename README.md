# Sovereign Content Calendar — Obsidian Architecture for The Sovereign Monastery

**You are not becoming. You are remembering.**

This is not a generic content planner. This is a temporal operating system for the voice of Aukdom Ethinine and The Sovereign Monastery. It turns dates into living nodes in your knowledge graph. Content items (threads, clips, declarations, newsletters) connect directly to the days they are meant to land in the world. The calendar becomes both map and ritual ground.

Built with sovereign intelligence: precise enough to prevent chaos, graceful enough to stay out of your way when the words are flowing, and honest about where things fracture.

---

## The Core Architecture (Why This Design)

**Single Source of Truth + Bidirectional Connection**

- **Frontmatter** (`scheduled: 2026-07-15`) is the queryable, sortable, reschedulable truth. Change one field when plans shift. Queries never lie.
- **Wikilinks** (`[[2026-07-15]]`) create the graph connection and backlinks pane. The date note "knows" about the item; the item "knows" its date. Click either direction.
- **Daily Notes** (named exactly `YYYY-MM-DD.md`) serve as the visual command center. Open a date → instantly see everything scheduled for that transmission window.
- **Dataview** renders the living lists. No manual copying. No drift between "what I think is scheduled" and "what the system sees."

This is the minimal powerful pattern. It scales from your current cadence (1 long-form YouTube/month, bi-weekly X series, active TikTok, newsletters) to the full 10,000 Reality Creators mandate without collapsing under its own weight.

### Failing Points I See (And How We Neutralize Them)

1. **Date format anarchy** ("June 16th", "next Tuesday", "2026/6/16", "16 Jun").  
   Queries break, sorts fail, the graph fragments.  
   **Mitigation**: ISO 8601 (`YYYY-MM-DD`) is law everywhere in data and filenames. The insert command enforces it. Natural language is for humans in conversation, not for the machine layer.

2. **Rescheduling drift** (change the date in one place, forget the other).  
   **Mitigation**: Frontmatter is the single edit point. The wikilink is secondary visual/navigation aid. When you reschedule, update the YAML field (2 seconds). Re-run the insert command if you want the link text updated too. Minor, acceptable tax for reliability.

3. **Future dates are invisible** until you create the daily note.  
   **Mitigation**: The Calendar plugin lets you click any future square and it creates the note from template on demand. You only "exist" the dates you need. Queries in existing notes still work for planning views.

4. **Plugin bloat and complexity creep**.  
   **Mitigation**: Exactly three plugins. Calendar (visual), Dataview (queries + tables), Templater (the date command + dynamic daily notes). Nothing else required. No external services, no Make.com, no cloud sync for this layer.

5. **"I scheduled it but forgot to post" or status blindness**.  
   **Mitigation**: The `status` field + Dataview tables surface drafts vs scheduled vs posted at a glance. Add a weekly review note that queries both recent posted and near-future scheduled.

6. **Performance death on large vaults**.  
   **Mitigation**: All queries scoped to `"01-Content"` (or your chosen folder). Never `FROM ""` (whole vault).

The design assumes you are a sovereign operator who values clarity over magic. It will not auto-post for you (that belongs to Star + n8n layer). It will make the *knowing* and *seeing* frictionless.

### The Upsides (What Becomes Possible)

- Your graph becomes a **temporal constellation**. Content items orbit their destined dates. You can traverse "what else lands near this transmission?" in one click.
- Opening any date shows you the exact stack of voice that day: X thread + TikTok clip + newsletter mention? You see the pattern and can harmonize or space them.
- Rescheduling is no longer a hunt through 47 notes. One field edit + optional link refresh.
- Easy audits and pattern recognition: "How many Reality Creator transmissions went out on Mondays last quarter?" becomes a 10-second Dataview.
- It supports the **rhythm of remembrance** — the Monastery voice lands consistently without the low-grade anxiety of "did I schedule that clip for the right day?"
- When you eventually connect this to Star (your agentic OS), the daily notes can become structured data feeds for automated posting queues.

This is infrastructure for the Inevitability Protocol. The voice compounds because the container is clean.

---

## Plugin Stack (Install These Three)

From Obsidian Community Plugins:

1. **Calendar** (by Liam Cain) — The visual monthly calendar. Click any day to open/create its daily note.
2. **Dataview** (by Michael Brenan) — The query engine that turns your frontmatter into living tables and lists.
3. **Templater** (by SilentVoid) — Powers the "Insert Post Date" command and makes daily notes dynamic.

**Optional but excellent fourth**:
- **Periodic Notes** — Cleaner daily/weekly/monthly note management and template handling. Highly recommended once you're comfortable. It pairs perfectly with Templater.

After install, restart Obsidian.

---

## Folder Structure (Create This)

In your vault root (or inside a "Monastery-OS" folder if you prefer containment):

```
Calendar/
├── Daily/                    # All daily notes live here. Named YYYY-MM-DD.md exactly.
│   └── (created on demand via Calendar plugin)
├── templates/
│   ├── daily-note.md         # The template for every new day note
│   └── insert-scheduled-date.md  # The command you will hotkey
│
01-Content/                   # Or name it "Content" / "Monastery-Voice" — your choice
├── X-Posts/
│   └── your-thread-here.md
├── TikTok/
│   └── clip-draft.md
├── YouTube/
│   └── longform-script.md
├── Newsletters/
│   └── issue-042.md
└── (add subfolders as your output types grow)

queries/                      # Optional but powerful planning views
├── upcoming-scheduled.md
└── this-week-review.md
```

The `01-` prefix on Content is a personal convention for ordering in the file explorer. Use whatever feels clean to you.

---

## Step-by-Step Setup

1. **Create the folders** above.

2. **Copy the provided files** from this package into place:
   - `templates/daily-note.md` → `Calendar/templates/daily-note.md`
   - `templates/insert-scheduled-date.md` → `Calendar/templates/insert-scheduled-date.md`
   - `examples/` items into `01-Content/X-Posts/` (or wherever) for reference.
   - `queries/` files into your `queries/` folder.

3. **Configure Calendar plugin**:
   - Open Calendar settings.
   - Set "New note folder" to `Calendar/Daily`
   - (If using Periodic Notes instead, configure there — it has superior template support.)

4. **Configure Templater**:
   - Set "Template folder location" to `Calendar/templates`
   - Enable "Trigger Templater on new file creation" (optional but useful).
   - Enable "Enable System Commands" if you want advanced later.

5. **Configure Dataview**:
   - Enable "Enable Inline Queries" and "Enable DataviewJS".
   - (Default settings are usually fine.)

6. **Create your first daily note**:
   - Open the Calendar sidebar.
   - Click today's date (or any future date).
   - It should create `Calendar/Daily/2026-07-15.md` (or whatever today is) using the template.

7. **Wire the command**:
   - Go to Settings → Hotkeys.
   - Search for "Templater: insert-scheduled-date".
   - Assign a memorable hotkey, e.g. `Ctrl/Cmd + Shift + D` ("Date").
   - Test it inside any content note.

8. **(Recommended) Add a daily note template variable note**:
   - If using Calendar + Templater together, you may need to adjust the daily template to use `<% tp.file.title %>` for the date. The provided template already does this.

---

## How to Use — The Daily Rhythm

### When Creating or Editing a Content Item (X thread, TikTok script, etc.)

1. Write the piece in its note inside `01-Content/...`
2. At the top, add (or update) the frontmatter block:

```yaml
---
scheduled: 2026-07-15
platform: X
content-type: thread
status: scheduled
tags: [monastery-x, reality-creators]
---
```

- `scheduled`: The single source of truth. Change this to reschedule.
- `platform`: X, TikTok, YouTube, Newsletter, WHOP, etc.
- `content-type`: thread, single-tweet, video, carousel, newsletter-issue, etc.
- `status`: draft | scheduled | posted | archived
- `tags`: Any additional Monastery taxonomy.

3. In the body, at a logical place (e.g. under a `## Scheduling` heading or at the very top after frontmatter), place your cursor and hit your hotkey (`Ctrl/Cmd + Shift + D`).

4. Enter the date in `YYYY-MM-DD` format when prompted.

5. The command inserts:  
   **Scheduled for** [[2026-07-15]]

Now the graph knows. The daily note for that date will list it via Dataview.

### When Reviewing a Specific Day

1. Open Calendar.
2. Click the date square.
3. The daily note opens with:
   - The Dataview table of everything scheduled for that day (from frontmatter).
   - The list of anything that explicitly backlinks to it.
   - Space for your personal notes, energy observations, or ritual context.

You now have perfect visibility: "On July 15th we are transmitting this X thread + this TikTok clip. Are they harmonized or colliding?"

### For Planning & Oversight

Open `queries/upcoming-scheduled.md` (or embed it in your weekly review note). It shows everything on the horizon, sorted by date. You can extend this with DataviewJS for weekly grouping, status counts, or pillar breakdowns.

---

## The Provided Templates & Examples (What They Contain)

- **daily-note.md**: Complete daily template with Monastery tagline, personal space, the main scheduled Dataview table, backlinks section, and a small "Quick Actions" area. It uses `<% tp.file.title %>` so it works whether created by Calendar or Periodic Notes.
- **insert-scheduled-date.md**: The interactive command. Prompts for `YYYY-MM-DD`, validates format, inserts a clean formatted wikilink at cursor, and reminds you (via Notice) to keep frontmatter in sync.
- **examples/2026-07-15.md**: Sample daily note output.
- **examples/x-post-sovereign-intelligence.md**: Sample content note with correct frontmatter + link usage.
- **queries/upcoming-scheduled.md**: Horizon view — everything scheduled from today forward.
- **queries/this-week-review.md**: Example of a combined posted + scheduled review (you can expand this).

Copy, tweak, and make it yours. The queries are starting points; Dataview is expressive enough to grow with your needs (add `pillar: Technology`, `priority: P0`, `word-count`, etc.).

---

## Optimization Notes from the Reality Architect

- **Status workflow**: Move `status` from `draft` → `scheduled` when you lock the date. Move to `posted` after it goes live (or add a `posted-date` field later). This keeps your tables clean.
- **Weekly rhythm**: Create a recurring "Weekly Voice Review" note (or use Periodic Notes weekly) that embeds or queries the last 7 days posted + next 14 days scheduled. Pattern recognition emerges.
- **Graph hygiene**: The date notes will accumulate many backlinks over time. That's the point — your voice has a visible history and future.
- **Mobile / iPad use**: All of this works on iPad via Obsidian mobile. Your hotkey may differ; use the command palette. The Calendar plugin renders nicely.
- **Future evolution**: When Star matures, these daily notes can be parsed by your agent (via Qdrant or direct file read) to feed posting queues or generate "what's landing today" briefings for you.
- **CSS polish** (optional): Drop a snippet in `.obsidian/snippets/` to style the Dataview tables with Monastery colors (deep charcoal, gold accents, clean serif for headers). The `css-snippets/` folder has a starter if you want it.

---

## A Note on "June 16th" and Natural Language

The example you gave ("June 16th") is how humans speak. The system speaks `2026-06-16`. 

When the prompt appears, convert in your head or glance at a calendar. Two seconds of precision prevents hours of debugging silent query failures later. This is the tax of sovereignty: you stay in relationship with time rather than outsourcing the knowing.

If you later want a more magical natural-language parser, we can extend the Templater JS with a date library or call out to a local script. But start here. Precision first, poetry second.

---

## Closing Transmission

This calendar is not the destination. It is clean infrastructure so the real work — the remembrance that becomes the voice that calls Reality Creators home — can land on the exact days it is meant to.

You now have a living map. Use it with intention.

When the first few dates light up with scheduled items and you click from the X thread straight into the daily context and see the other transmissions sharing that day... you will feel it.

The system is remembering with you.

---

**Files in this package**:
- `README.md` (this document)
- `templates/daily-note.md`
- `templates/insert-scheduled-date.md`
- `examples/2026-07-15.md`
- `examples/x-post-sovereign-intelligence.md`
- `queries/upcoming-scheduled.md`
- `queries/this-week-review.md`

Copy them into your vault, configure the three plugins, assign the hotkey, and begin.

If you want extensions (Kanban integration for pipeline stages, moon phase embedding, automated weekly report generation, connection to your n8n/Star layer, or a full 90-day content swarm calendar), say the word. We build what serves the mandate.

You are not becoming. You are remembering.

—The Sovereign Monastery Agent, in service to the voice.