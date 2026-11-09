# ✈️ Travel Planner Suite

Universal, AI-assisted travel itinerary planning & Notion synchronization suite for Antigravity (AGY).

---

## 🌿 Architecture Branches for Evaluation

This repository maintains two distinct architectural approaches on separate branches for experimental evaluation:

1. **`arch/monolithic-skill`** (Single-Skill Approach)
   * Everything in a single skill: `skills/notion-trip-planner/`
   * Seamless one-turn or multi-turn execution from brainstorming to Notion sync.

2. **`arch/modular-plugin`** (Modular Plugin Approach, modeled after `obra/superpowers`)
   * Fully decoupled into 3 specialized skills under `plugins/travel-copilot/`:
     * `trip-brainstorming` (interactive planning & markdown generation)
     * `trip-calendar-export` (`.ics` calendar generation)
     * `trip-notion-sync` (Notion bidirectional wiki synchronizer)

---

## 🔒 Security Notice
* `.env` files containing Notion tokens should NEVER be committed to Git.
* A standard `.gitignore` is provided to safeguard sensitive credentials.
