---
name: trip-brainstorming
description: >-
  Interactive travel itinerary brainstorming and local multi-page Markdown generator.
  Use this skill whenever the user wants to discuss where to go, plan a new travel schedule,
  organize daily routes, or generate a structured Markdown itinerary.
---

# 🗺️ Trip Brainstorming & Itinerary Architect (`trip-brainstorming`)

This skill focuses strictly on **interactive planning and generating high-quality local Markdown files**. It does NOT require Notion and runs completely offline.

---

## 🌟 Conversational Principles

1. **Lightweight & Engaging**: Never ask more than 1–2 key questions per turn.
2. **Missing Anchors Discovery**: Find out destination, duration/dates, flights, hotel location, and top priority spots/activities.
3. **Smart Logistics**: Group spots geographically to minimize travel time; warn about early queuing or reservations.
4. **Always Offer Plan A & Plan B**: Proactively provide alternative options for popular restaurants or outdoor sights.

---

## 📝 Generation Workflow

Once the daily outline is confirmed with the user, create the following directory and files in the current project root (`<workspace_root>`):

```text
<workspace_root>/
├── Itinerary/                       # Hub & daily detailed schedules (platform-agnostic)
│   ├── Trip_Dashboard.md            # Hub: Overview, Mermaid Gantt chart, subpage links
│   ├── Day1_<Theme>.md              # Day 1 detailed schedule & transit guide
│   ├── Day2_<Theme>.md              # Day 2 detailed schedule...
│   ├── ...                          # Up to DayN_<Theme>.md
│   ├── Travel_Tips_and_Apps.md      # Local navigation, taxi apps, IC cards, packing tips
│   └── Gourmet_and_Spots.md         # Multilingual spot/restaurant address tables for navigation
└── Full_Itinerary.md                # Single-page complete backup of the trip
```

### 🚨 Mandatory Path & Formatting Rules
* **Use Relative Paths Only**: Every link, image, or file reference MUST use relative paths (e.g., `./Itinerary/Day1_Arriving.md` or `Trip_Dashboard.md`). NEVER use absolute paths (`/Users/...` or `file:///...`).
* **Level 1 Heading**: Every subpage MUST begin with `# 📅 Day X...` or `# 💡...` as line 1.
* **Back-Link**: Every subpage MUST include `[⬅️ 返回行程儀表板](Trip_Dashboard.md)` right below the heading.
* **Dashboard Links**: In `Trip_Dashboard.md`, use exact filenames: `* 🔗 **[Day 1](Day1_Arriving.md)**`.
* **Alerts & Tables**: Use single-line `> [!NOTE]` / `> [!IMPORTANT]` alerts and Markdown tables for timelines.

---

## 🤝 Next Step Handoff (交棒指引)

When you finish generating or editing the `.md` itinerary files, prompt the user with clear next steps:
> *"本地行程 Markdown 檔案已為您建立/更新完成！請問您接下來想要：*
> 1. *繼續調整與討論行程內容？*
> 2. *使用 `trip-calendar-export` 匯出手機/Google日曆檔 (`.ics`)？*
> 3. *使用 `trip-notion-sync` 將行程同步至 Notion Wiki 頁面？"*
