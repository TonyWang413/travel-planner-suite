---
name: trip-notion-sync
description: >-
  Syncs local multi-page travel Markdown guides to a closed-loop Notion Wiki page.
  Use this skill whenever the user wants to push their itinerary to Notion, configure
  Notion credentials (.env), or update an existing Notion travel dashboard.
---

# 🔗 Notion Wiki Synchronizer (`trip-notion-sync`)

This skill handles syncing the multi-page travel guide located in `<workspace_root>/Notion_Itinerary/` directly into the user's Notion page using the universal synchronization engine.

---

## 🔒 Prerequisites & Security

1. Ensure `<workspace_root>/.gitignore` includes `.env`.
2. Check `<workspace_root>/.env` for `NOTION_TOKEN` and `NOTION_PAGE_URL`.
   * If missing, ask the user for their Notion Page URL and reuse/request their Integration Token.
   * **Rule**: Never print or leak `NOTION_TOKEN` in responses or logs.
3. Ensure `<workspace_root>/sync_to_notion.py` exists (copy from [sync_to_notion.py](./scripts/sync_to_notion.py) if not present).

---

## 🚀 Execution & Zero-Click Sync

Run the sync script in the workspace directory:
```bash
python3 sync_to_notion.py
```

* **If status 404**: Remind the user to click `...` on the Notion page ➔ `Connect to` ➔ Select their integration bot, then re-run automatically.
* **Auto-Sync on Edits**: During subsequent turns, whenever an itinerary file is modified, rerun `python3 sync_to_notion.py` in the background automatically.

---

## 📋 Session Handoff (`AGENTS.md`) Generation

After the initial successful sync, read the template [AGENTS.template.md](./resources/AGENTS.template.md) and generate `<workspace_root>/AGENTS.md` tailored to this specific trip (filling in title, dates, flights, hotels, and daily highlights).

### 🚨 Mandatory Rules in `AGENTS.md`:
* **Use Relative Paths Only**: Never use absolute paths (such as `/Users/...` or `file:///...`) in any project documentation or files; always use relative paths (e.g., `./Notion_Itinerary/Day1_Arriving.md`).
* **Zero-Click Sync**: Future agent sessions must automatically run `python3 sync_to_notion.py` on any itinerary edit without asking the user.
