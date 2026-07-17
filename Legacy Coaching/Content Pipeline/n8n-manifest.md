---
tags:
  - system
  - n8n
---
# n8n Content Pipeline Manifest

> [!IMPORTANT]
> **This file is the contract between your Obsidian vault and your n8n automation agent.** Your agent should read this file first to understand the schema, folder structure, and rules for parsing and posting content. Do not rename the frontmatter fields without updating your n8n workflow accordingly.

---

## Vault Path

```
Content Pipeline/
```

All content notes live inside this root folder, organized by platform subfolder.

---

## Folder-to-Platform Mapping

| Folder Path               | Platform         | API Target            |
|----------------------------|------------------|-----------------------|
| `Content Pipeline/LinkedIn/` | LinkedIn         | LinkedIn API          |
| `Content Pipeline/X/`        | X (Twitter)      | X API v2              |
| `Content Pipeline/Instagram/`| Instagram        | Instagram Graph API   |
| `Content Pipeline/TikTok/`   | TikTok           | TikTok Content API    |
| `Content Pipeline/YouTube/`  | YouTube          | YouTube Data API v3   |
| `Content Pipeline/Whop/`     | Whop             | Whop API              |

The `platform` field in frontmatter is the authoritative routing key. Folder location is a secondary organizational aid.

---

## Frontmatter Schema

Every content note MUST have this YAML frontmatter block. Fields marked `[AGENT-WRITE]` are written by the n8n agent, not by the user.

```yaml
---
# === IDENTITY (User sets these) ===
title: "string"                    # Post title or headline
platform: "enum"                   # LinkedIn | X | Instagram | TikTok | YouTube | YouTube Shorts | Whop
content_type: "enum"               # Platform-specific, see table below

# === SCHEDULING (User sets these) ===
publish_date: YYYY-MM-DD           # ISO 8601 date — SINGLE SOURCE OF TRUTH
publish_time: "HH:MM"             # 24-hour format, Central Time (CST/CDT)
post_status: "enum"                # idea | draft | review | ready | posted | failed

# === CONTENT METADATA (User sets these) ===
hook: "string"                     # Opening line / scroll-stopper
cta: "string"                      # Call to action text
hashtags: ["string"]               # List of hashtags with # prefix
media_files: ["string"]            # Relative vault paths or external URLs

# === NETWORK STRATEGY (Connective Tissue) ===
relates_to: "[[filename]]"         # Wikilink to the previous related post
relation_type: "enum"              # reply | quote | link | none
marketing_stage: "enum"            # awareness | value | conversion

# === AUTOMATION [AGENT-WRITE] ===
n8n_post_id: "string"             # Platform post ID or URL after successful post
n8n_posted_at: "datetime"         # ISO 8601 timestamp of actual post time
n8n_error: "string"               # Error message if posting failed

# === OBSIDIAN ===
tags: ["string"]                   # Obsidian tags for organization
---
```

---

## Platform-Specific `content_type` Values

| Platform         | Allowed `content_type` Values                     |
|-------------------|----------------------------------------------------|
| LinkedIn          | `article`, `post`, `carousel`, `poll`              |
| X                 | `single`, `thread`, `quote-tweet`, `article`       |
| Instagram         | `single-image`, `carousel`, `reel`, `story`        |
| TikTok            | `video`, `slideshow`                               |
| YouTube           | `longform`, `short`                                |
| Whop              | `announcement`, `resource`, `discussion`           |
| Multi-Platform    | `cross-post`                                       |

---

## `post_status` Lifecycle

```
idea → draft → review → ready → posted
                                  ↘ failed
```

| Status   | Meaning                                           | n8n Action                  |
|----------|---------------------------------------------------|-----------------------------|
| `idea`   | Concept only, no content written                  | IGNORE                      |
| `draft`  | Content in progress                               | IGNORE                      |
| `review` | Content complete, awaiting final approval          | IGNORE                      |
| `ready`  | Approved and scheduled — cleared for posting       | **PROCESS IF DATE MATCHES** |
| `posted` | Successfully published                             | IGNORE (already done)       |
| `failed` | Posting attempted but failed                       | RETRY or ALERT user         |

---

## n8n Agent Processing Rules

### Trigger Condition
```
post_status == "ready" AND publish_date <= TODAY AND publish_time <= CURRENT_TIME
```

### Processing Steps

1. **SCAN**: Read all `.md` files recursively under `Content Pipeline/`
2. **PARSE**: Extract YAML frontmatter from each file
3. **FILTER**: Select notes matching the trigger condition above
4. **RESOLVE RELATIONS (Connective Tissue)**:
   - If `relates_to` is populated (e.g., `[[some-post-name]]`):
     - Strip the brackets to get the filename.
     - Look up that file in the vault.
     - Extract its `n8n_post_id`.
     - Pass this ID to the posting node alongside the `relation_type` (`reply`, `quote`, `link`).
5. **ROUTE**: Use `platform` field to select the correct API posting node
6. **EXTRACT BODY**: Parse the markdown body for content payload:

| Platform  | Primary Body Section    | Additional Sections           |
|-----------|-------------------------|-------------------------------|
| LinkedIn  | `## Body`               | `## Hook`, `## Call to Action` |
| X         | `## Tweet 1`, `## Tweet 2`, ... | `## Hashtags`           |
| Instagram | `## Caption`            | `## Hashtags`                 |
| TikTok    | `## Caption`            | `## Script / Hook`            |
| YouTube   | `## Description`        | `## Script`, `## Tags`        |
| Whop      | `## Body`               | `## Attachments`              |

> **Exception — X `content_type: article`:** X's native long-form Articles feature is not tweet-chunked. These notes use a single `## Article Body` section instead of `## Tweet 1`/`## Tweet 2`. The agent should route `content_type: article` notes to the X Articles API endpoint rather than the standard tweet/thread endpoint. `## Hashtags` still applies if the platform supports tags on Articles.

6. **POST**: Send to platform API with extracted content + `media_files` + `hashtags`
7. **UPDATE FRONTMATTER** on success:
   ```yaml
   post_status: posted
   n8n_post_id: "<platform_post_id_or_url>"
   n8n_posted_at: "<ISO 8601 timestamp>"
   ```
8. **UPDATE FRONTMATTER** on failure:
   ```yaml
   post_status: failed
   n8n_error: "<error message from API>"
   ```

### Multi-Platform Notes

If a note uses `platforms` (plural, list) instead of `platform` (singular, string), the agent must:
- Post to EACH platform in the list
- Extract platform-specific content from `## LinkedIn Version`, `## X Version`, etc.
- Record each platform's post ID in `n8n_post_id_linkedin`, `n8n_post_id_x`, etc.
- Only set `post_status: posted` when ALL platforms succeed

---

## File Access Method

n8n can access vault files via:

1. **Local file system** (recommended): Use the "Read/Write Files from Disk" node to read `.md` files from the vault path
2. **Obsidian Local REST API plugin** (alternative): If installed, provides an HTTP API to read/write notes
3. **Git sync** (alternative): If vault is Git-synced, n8n can pull the repo and parse files

### Reading Frontmatter in n8n

```javascript
// In an n8n Function node — parse frontmatter from a .md file
const fileContent = $input.item.json.data; // raw file text
const frontmatterMatch = fileContent.match(/^---\n([\s\S]*?)\n---/);
if (frontmatterMatch) {
  const yaml = require('js-yaml');
  const metadata = yaml.load(frontmatterMatch[1]);
  return { json: metadata };
}
```

### Extracting Body Sections in n8n

```javascript
// Extract a specific ## section from markdown body
function extractSection(markdown, sectionName) {
  const regex = new RegExp(`## ${sectionName}\\n([\\s\\S]*?)(?=\\n## |$)`, 'i');
  const match = markdown.match(regex);
  if (match) {
    // Remove HTML comments
    return match[1].replace(/<!--[\s\S]*?-->/g, '').trim();
  }
  return '';
}

const body = extractSection(fileContent, 'Body');
const hook = extractSection(fileContent, 'Hook');
const caption = extractSection(fileContent, 'Caption');
```

### Resolving `relates_to` Wikilinks in n8n

```javascript
// Example logic to extract the target filename from a wikilink
const relatesToString = metadata.relates_to || "";
let targetFilename = "";

const linkMatch = relatesToString.match(/\\[\\[(.*?)\\]\\]/);
if (linkMatch) {
  targetFilename = linkMatch[1] + ".md";
  // The n8n workflow should now do a "Read File" node to find this target file
  // and parse its frontmatter to get the n8n_post_id to use as the reply/quote target.
}
```

### Writing Back to Frontmatter in n8n

```javascript
// Update a specific frontmatter field in a .md file
function updateFrontmatter(fileContent, field, value) {
  const regex = new RegExp(`(^${field}:).*$`, 'm');
  if (fileContent.match(regex)) {
    return fileContent.replace(regex, `$1 "${value}"`);
  }
  // If field doesn't exist, add it before the closing ---
  return fileContent.replace(/\n---\n/, `\n${field}: "${value}"\n---\n`);
}
```

---

## Timezone

All `publish_time` values are in **Central Time** (CST: UTC-6 / CDT: UTC-5). The n8n agent must convert to UTC or the target platform's expected timezone before posting.

---

## File Naming Convention

Content notes should be named descriptively:
```
<platform>/<short-descriptive-title>.md
```
Examples:
- `LinkedIn/sovereign-identity-leadership-post.md`
- `X/reality-architects-thread.md`
- `YouTube/legacy-coaching-ep-12.md`
- `TikTok/3-second-hook-discipline-clip.md`

---

*Last updated: 2026-07-15*
