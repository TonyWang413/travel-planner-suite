---
name: trip-calendar-export
description: >-
  Generates standard iCalendar (.ics) files from local Markdown travel itineraries.
  Use this skill whenever the user wants to export their trip schedule to Apple Calendar,
  Google Calendar, or generate a .ics event file.
---

# 📅 Trip Calendar Exporter (`trip-calendar-export`)

This skill extracts scheduled events, flights, dining reservations, and activities from the local trip itinerary and produces a clean, standard `.ics` file.

---

## 🛠️ Execution Procedure

1. Read the local itinerary files (`Full_Itinerary.md` or files inside `Itinerary/`).
2. Identify all timed events (Flight arrival, check-in, reservations, key stops).
3. Generate `<workspace_root>/Trip_Itinerary.ics` using standard iCalendar VEVENT blocks:
   * `DTSTART` / `DTEND` (Formatted as `YYYYMMDDTHHMMSS` in local trip timezone)
   * `SUMMARY` (With emoji icons, e.g., `✈️ 大韓航空 KE2022`, `🍽️ 預約 CHOI DOT`)
   * `LOCATION` (Name and address if available)
   * `DESCRIPTION` (Detailed instructions, booking refs, or Plan B notes)
4. Tell the user how to import:
   * **Mac/iPhone**: Double-click the `.ics` file to import directly into Apple Calendar.
   * **Google Calendar**: Go to Google Calendar Settings ➔ Import & Export ➔ Upload `.ics`.
