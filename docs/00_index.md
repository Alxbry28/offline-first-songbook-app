# Lyrics & Chords App — Planning Documents

**Date:** July 10, 2026  
**Status:** Draft / planning only — no implementation yet  
**Source:** [project_idea.md](../project_idea.md) (item 3)

---

## What this folder contains

| # | Document | Purpose |
|---|----------|---------|
| 01 | [project_plan.md](./01_project_plan.md) | Vision, problem statement, goals, and key decisions |
| 02 | [technology_recommendations.md](./02_technology_recommendations.md) | React Native vs Flutter, database, Genius API, offline strategy |
| 03 | [app_outline.md](./03_app_outline.md) | Feature breakdown, user stories, screens, data model sketch |
| 04 | [architecture_outline.md](./04_architecture_outline.md) | App structure, navigation, sync, and customization system |
| 05 | [development_roadmap.md](./05_development_roadmap.md) | Phased build order and milestones |

---

## Quick summary

You are building a **mobile-only personal app for singers and musicians** who want full control over how songs are displayed, edited, transposed, and used during practice and gigs.

**Core idea:** Own the app so you can customize what store apps won't let you change — chord placement per lyric line, lyrics readability, vocal key transposition, display layout, offline access, and your personal songbook.

**v1 pillars:**

1. **View** songs with chords aligned to lyrics  
2. **Create / edit** songs with chord progressions on every line  
3. **Import** lyrics from Genius API (with manual fallback)  
4. **Offline** — full library available without network  

---

## Why build your own?

| Pain with store apps | What your app does differently |
|----------------------|--------------------------------|
| Fixed layout — chords too small, wrong position, or hard to read while playing | You control font size, spacing, chord-above-lyrics vs inline, dark mode for stage |
| Can't edit chord charts the way *you* think | Full editor: move chords, rename, transpose, add sections |
| Subscription / ads / clutter | Personal app, your data, no bloat |
| Offline locked behind paywall or broken sync | Offline-first by design |
| Singer & musician needs differ from generic apps | Quick vocal key transposition, lyrics clarity, stage performance mode, setlists for gigs |

---

## Suggested reading order

1. `01_project_plan.md` — align on what you are building and why  
2. `02_technology_recommendations.md` — pick your stack  
3. `03_app_outline.md` — review features and trim scope for v1  
4. `04_architecture_outline.md` — see how pieces fit together  
5. `05_development_roadmap.md` — decide what to build first  

---

## Next steps (when you are ready to build)

- [ ] Confirm: React Native (Expo) or Flutter  
- [ ] Confirm: local-only vs local + optional cloud backup  
- [ ] Pick v1 scope from `03_app_outline.md` (Must-have vs Later)  
- [ ] Decide Genius API usage vs manual paste for v1 (licensing note in tech doc)  
- [ ] Set up repo and project scaffold (not started yet)
