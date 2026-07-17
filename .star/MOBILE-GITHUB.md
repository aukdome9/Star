# Phone + GitHub access

## Goal
View and interact with the **entire Master vault** from your phone, while the continuous agent runs on your PC.

## Local (PC)
```text
start star
```
Opens the Star dashboard + continuous Grok with full vault tools.

## Online mirror (GitHub)

1. Install Git if needed: `winget install Git.Git`
2. From this vault (or run `star-integration/scripts/publish-vault.ps1`):
   ```powershell
   cd "C:\Users\aukdo\Downloads\master\Master vault"
   git init
   git add .
   git commit -m "Star Operator vault"
   # create empty private repo on GitHub, then:
   git remote add origin https://github.com/<you>/<repo>.git
   git branch -M main
   git push -u origin main
   ```
3. On phone:
   - **GitHub app** — browse/edit markdown  
   - **Obsidian** + Git plugin / Obsidian Sync — full vault UX  
   - **Working Copy** (iOS) or **MGit** (Android) — commit from phone  

## Privacy
Use a **private** repository. Do not commit secrets, API keys, or `.env` files.
This vault's `.gitignore` already excludes common secret patterns.
