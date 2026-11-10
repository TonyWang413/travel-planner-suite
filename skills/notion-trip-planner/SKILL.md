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
4. **Use Relative Paths Only**: Every link, image, or file reference MUST use relative paths (e.g., `./Itinerary/Day1_Arriving.md` or `Trip_Dashboard.md`). NEVER use absolute paths (`/Users/...` or `file:///...`).
5. **Zero-Click Notion Sync**: Once the Notion connection is configured, YOU (the agent) must run the sync script automatically in the background after every itinerary edit. Never ask the user to run terminal commands manually.

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
   * Confirm with the user if the pacing and grouping look good, or if they want adjustments before generating the full files.
   * Ask if they plan to sync to Notion (optional): if yes, request their target Notion blank page URL.

---

## 📂 Phase 2: Generate Local Multi-Page Markdown & `.ics` Calendar

Once the outline is agreed upon, create the following structure in the current workspace root (`<workspace_root>`):

```text
<workspace_root>/
├── Itinerary/                       # Hub & daily detailed schedules (platform-agnostic)
│   ├── Trip_Dashboard.md            # Main hub (Overview, quick links, Mermaid Gantt chart)
│   ├── Day1_<Theme>.md              # Day 1 detailed schedule, transit guide, dining Plan A/B
│   ├── Day2_<Theme>.md              # Day 2 detailed schedule...
│   ├── ...                          # Up to DayN_<Theme>.md
│   ├── Travel_Tips_and_Apps.md      # Local navigation apps, taxi apps, IC cards, tax refund tips
│   └── Gourmet_and_Spots.md         # Bilingual/Trilingual table & addresses for map apps
├── Full_Itinerary.md                # Single-page complete backup of the itinerary
├── Trip_Itinerary.ics               # Standard iCalendar file with VEVENTs for all scheduled items
├── sync_to_notion.py                # Copied from this skill's scripts/sync_to_notion.py
├── .env                             # Stores NOTION_TOKEN and NOTION_PAGE_URL (if syncing)
└── .gitignore                       # Must ignore .env, __pycache__/, *.pyc
```

### Markdown Formatting Rules for `Itinerary/*.md`
* **Top Heading (`# Title`)**: Every file in `Itinerary/` MUST start with a level-1 heading `# ...`. The script uses this as the Notion subpage title.
* **Dashboard Links**: In `Trip_Dashboard.md`, link to each subpage using its exact filename:
  ```markdown
  * 🔗 **[Day 1：5/27 (三) | 抵達與漫步](Day1_Arriving.md)**
  ```
* **Back-Links in Subpages**: Every subpage MUST include `[⬅️ 返回行程儀表板](Trip_Dashboard.md)` right below the `# Title`.
* **Callouts**: Use single-line GitHub alerts for important tips so the parser converts them into Notion Callouts (`> [!NOTE]`, `> [!IMPORTANT]`).
* **Tables**: Include a structured timeline table (`| 時間 | 行程項目 | 詳細內容與說明 |`) at the top of each Day file.

---

## 🔗 Phase 3: Notion Setup & Zero-Click Sync (Optional)

If the user wants to sync to Notion:

1. Copy [sync_to_notion.py](./scripts/sync_to_notion.py) into `<workspace_root>/sync_to_notion.py`.
2. Configure `<workspace_root>/.env` with `NOTION_TOKEN` and `NOTION_PAGE_URL`. Ensure `.gitignore` ignores `.env`.
3. Execute in background:
   ```bash
   python3 sync_to_notion.py
   ```
4. If status 404, remind the user to authorize the integration bot via `...` ➔ `Connect to` on the Notion page.

---

## 🔄 Phase 4: Iterative Refinement Loop

Whenever the user asks to change, add, or swap any part of the itinerary:
1. Update the relevant file(s) inside `Itinerary/` (using relative links only).
2. If Notion sync is configured, immediately and automatically run `python3 sync_to_notion.py` in the background.
3. Tell the user the changes are live!
