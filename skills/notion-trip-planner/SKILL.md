---
name: notion-trip-planner
description: >-
  End-to-end interactive travel itinerary planner and Notion Wiki synchronizer.
  Use this skill whenever the user wants to plan a new trip, discuss travel itineraries,
  organize daily travel schedules, generate iCalendar (.ics) files for a trip, or sync
  multi-page travel markdown guides to Notion.
---

# ✈️ Interactive Travel Planner & Notion Wiki Synchronizer (`notion-trip-planner`)

This skill guides you through collaboratively planning a trip with the user from scratch, generating a rich multi-page Markdown itinerary + `.ics` calendar, and automatically syncing it into a closed-loop Notion Wiki with bidirectional links.

---

## 🌟 Core Philosophy (Lightweight Brainstorming)

Do **NOT** overwhelm the user with massive questionnaires or rigid software specs. Instead:
1. **Conversational & Focused Discovery**: Ask only 1–2 key questions at a time to gather missing essentials (destination, dates/duration, flight times, hotel location, and must-visit spots/restaurants).
2. **Proactive Route Optimization**: Group attractions geographically, estimate realistic transit times, and warn about early-morning queues or reservation requirements.
3. **Always Offer Plan A / Plan B**: For high-demand restaurants or weather-dependent activities, proactively include a backup option (Plan B) in the same area.
4. **Zero-Click Notion Sync**: Once the Notion connection is configured, YOU (the agent) must run the sync script automatically in the background after every itinerary edit. Never ask the user to run terminal commands manually.

---

## 🚀 Phase 1: Interactive Trip Planning (From Scratch)

When the user says they want to plan a trip (e.g., *"幫我規劃東京五天四夜"*):

1. **Gather Core Anchors**:
   * Check what the user has already provided:
     * **Destination & Dates** (e.g., 5 days 4 nights, specific dates or month)
     * **Flights / Arrival & Departure Times** (Crucial for Day 1 and Final Day scheduling)
     * **Accommodation** (Area or specific hotel name)
     * **Must-Haves** (Specific pop-up stores, clinics, Michelin restaurants, shopping districts)
   * If any core anchor is missing, ask concisely (or offer sensible recommendations if they haven't booked yet).
2. **Draft & Align on the Daily Outline**:
   * Present a clear, day-by-day summary outline in chat first.
   * Confirm with the user if the pacing and grouping look good, or if they want adjustments before generating the full files and syncing to Notion.
   * Also ask for their **target Notion blank page URL** (if not yet provided) so you can sync immediately.

---

## 📂 Phase 2: Generate Local Multi-Page Markdown & `.ics` Calendar

Once the initial outline is agreed upon (or if the user asks to generate/sync directly), create the following structure in the current workspace root (`<workspace_root>`):

```text
<workspace_root>/
├── Notion_Itinerary/
│   ├── Trip_Dashboard.md            # Main hub (Overview, quick links, Mermaid Gantt chart)
│   ├── Day1_<Theme>.md              # Day 1 detailed schedule, transit guide, dining Plan A/B
│   ├── Day2_<Theme>.md              # Day 2 detailed schedule...
│   ├── ...                          # Up to DayN_<Theme>.md
│   ├── Travel_Tips_and_Apps.md      # Local navigation apps, taxi apps, IC cards, tax refund tips
│   └── Gourmet_and_Spots.md         # Bilingual/Trilingual table & addresses (Local language + Chinese/English) for copy-pasting into map apps
├── Full_Itinerary.md                # Single-page complete backup of the itinerary
├── Trip_Itinerary.ics               # Standard iCalendar file with VEVENTs for all scheduled items
├── sync_to_notion.py                # Copied from this skill's scripts/sync_to_notion.py
├── .env                             # Stores NOTION_TOKEN and NOTION_PAGE_URL (never commit!)
├── .gitignore                       # Must ignore .env, __pycache__/, *.pyc
└── AGENTS.md                        # Project memory file generated in Phase 3
```

### Markdown Formatting Rules for `Notion_Itinerary/*.md`
To ensure `sync_to_notion.py` renders a premium Notion Wiki with working bidirectional links:
1. **Top Heading (`# Title`)**: Every file in `Notion_Itinerary/` MUST start with a level-1 heading `# ...` (e.g., `# 📅 Day 1：5/27 (三) | 抵達與漫步`). The script uses this as the Notion subpage title.
2. **Dashboard Links**: In `Trip_Dashboard.md`, link to each subpage using its exact filename:
   ```markdown
   * 🔗 **[Day 1：5/27 (三) | 抵達與漫步](Day1_Arriving.md)**
   ```
3. **Back-Links in Subpages**: Every subpage (`Day1_*.md`, `Travel_Tips_and_Apps.md`, `Gourmet_and_Spots.md`) MUST include a back-link right below the `# Title`:
   ```markdown
   [⬅️ 返回行程儀表板](Trip_Dashboard.md)
   ```
4. **Callouts**: Use single-line GitHub alerts for important tips so the parser converts them into Notion Callout blocks:
   ```markdown
   > [!IMPORTANT] 關鍵提醒內容寫在同一行或下一行開頭帶 >
   > [!TIP] 實用貼士內容
   ```
5. **Tables**: Include a structured timeline table (`| 時間 | 行程項目 | 詳細內容與說明 |`) at the top of each Day file.

---

## 🔗 Phase 3: Notion Setup, Sync & `AGENTS.md` Generation

### 1. Copy the Universal Sync Script
Copy [sync_to_notion.py](./scripts/sync_to_notion.py) from this skill folder into `<workspace_root>/sync_to_notion.py`.

### 2. Configure `.env` and `.gitignore`
* Ensure `<workspace_root>/.gitignore` contains `.env`.
* Check if the user already has a `NOTION_TOKEN` from a previous trip (e.g., in another trip folder or provided in chat).
  * **Note**: The same Notion Integration Token (e.g., `首爾行程小助手` or `旅遊行程小助手`) can be reused across all future trips! The user only needs to:
    1. Create a new blank page in Notion for the new trip and give you its URL (`NOTION_PAGE_URL`).
    2. Click `...` (top right of the new Notion page) ➔ `Connect to` / `Add connections` ➔ Select their integration bot.
* Write `NOTION_TOKEN=...` and `NOTION_PAGE_URL=...` into `<workspace_root>/.env`.
* **Security Rule**: NEVER print or expose `NOTION_TOKEN` in chat or logs.

### 3. Run the Sync Automatically
Execute in `<workspace_root>`:
```bash
python3 sync_to_notion.py
```
If it returns a `404` error, politely remind the user to open the Notion page's top-right `...` menu ➔ `Connect to` and authorize the integration bot, then re-run automatically once they confirm.

### 4. Generate Workspace `AGENTS.md`
Once the initial files and Notion sync are set up, read the template [AGENTS.template.md](./resources/AGENTS.template.md) and create `<workspace_root>/AGENTS.md` containing:
* **Trip Context**: Destination, dates, flights, hotel, and daily highlights for THIS specific trip.
* **Workspace Architecture**: Overview of `Notion_Itinerary/`, `.ics`, and `sync_to_notion.py`.
* **Mandatory Rules in `AGENTS.md`**:
  * **Use Relative Paths Only**: Never use absolute paths (such as `/Users/...` or `file:///...`) in any project documentation or files; always use relative paths (e.g., `./Notion_Itinerary/Day1_Arriving.md`).
  * **Zero-Click Sync**: Future agent sessions must automatically run `python3 sync_to_notion.py` on any itinerary edit without asking the user.

---

## 🔄 Phase 4: Iterative Refinement Loop

Whenever the user asks to change, add, or swap any part of the itinerary:
1. Update the relevant file(s) inside `Notion_Itinerary/` (and `Trip_Dashboard.md` / `Full_Itinerary.md` / `.ics` if times or major spots changed).
2. Immediately and automatically run `python3 sync_to_notion.py` in the background.
3. Tell the user the changes are live and they can refresh their Notion page to view the updated itinerary!
