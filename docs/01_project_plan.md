# 01 — Project Plan

## 1. Vision

Build a **personal mobile app** for singers and musicians to view, create, and perform songs with **lyrics and chord progressions** in a layout you control.

Unlike apps from the store, this is **yours to customize**: how chords sit on lyrics, how text looks on stage, how you transpose keys, and how your setlists are organized.

---

## 2. Problem statement

| Pain today | What the app should solve |
|------------|---------------------------|
| Chord apps use layouts that don't match how you read while singing or playing | Chords above lyrics (or inline), large readable text, scroll or auto-scroll you configure |
| You can't edit chord charts to match how *you* hear the song | Full song editor: lyrics + per-line chords, sections, notes |
| Finding lyrics means copy-paste across apps and browsers | Import from Genius API or paste; save once to your library |
| Gigs and rehearsals often have poor or no internet | **Offline-first** — entire library on device |
| Store apps add features you don't need (tabs, social, subscriptions) | Lean personal tool focused on lyrics + chords + setlists |
| Transposing for vocal range or band arrangement is clunky | One-tap transpose; remember preferred key per song |
| Generic chord apps bury what vocalists and musicians care about | Clear key/scale info, section markers, lyric cues, and vocal arrangement notes |

---

## 3. Product shape

### Single-purpose musician app (recommended)

One install focused on **your songbook**:

```
Home (Library)
├── Songs (search, filter, favorites)
├── Song view (lyrics + chords, transpose, performance mode)
├── Song editor (create / edit)
├── Setlists (gig / rehearsal lists)
└── Settings (display, import, backup, theme)
```

**Not in v1:** social sharing, public song marketplace, audio playback, full tab notation, band collaboration.

**Why single-purpose:** Faster to ship, easier to customize, matches "I just want my charts my way."

---

## 4. Users and scope

| | |
|---|---|
| **Primary user** | You (solo singer / musician) |
| **Platform** | Mobile only (iOS + Android) |
| **v1 audience** | Personal use — no multi-user accounts required |
| **Out of scope for now** | Web app, desktop, DAW integration, MIDI, real-time band sync |

Design for **single-user** first. That simplifies storage, privacy, and licensing (lyrics stay on your device).

---

## 5. Core principles

1. **Readable while playing** — large fonts, high contrast, minimal UI in performance mode  
2. **Chords on every line** — each lyric line can have its own chord(s); chords align visually above syllables where possible  
3. **Your library, your edits** — imported lyrics are starting points; you own the final chart  
4. **Offline by default** — network only for import/search; everything else works without internet  
5. **Customize without coding** — display presets (font, spacing, theme) you can change in settings, not locked by a publisher  
6. **Start small** — view + create + offline before Genius API, setlists, or advanced musician extras  

---

## 6. Feature scope tiers

Use these labels in `03_app_outline.md`:

| Tier | Meaning |
|------|---------|
| **Must-have (v1)** | Minimum useful product for practice and gigs |
| **Should-have (v1.1)** | High value, build soon after v1 |
| **Nice-to-have** | Later enhancements |
| **Future** | Explicitly deferred |

---

## 7. Genius API — high-level plan

Genius can **search and fetch lyrics** to reduce manual typing.

### Intended flow

1. User searches song title + artist in app  
2. App calls Genius API → returns song metadata + lyrics URL  
3. App fetches/parses lyrics → user reviews in editor  
4. User adds chords per line → saves to local library  

### Important constraints

- Genius API provides **lyrics access**, not chords — chords are always **your input**  
- Lyrics are **copyrighted** — this is a personal tool; do not redistribute lyrics publicly  
- API may require attribution per Genius terms — show "Lyrics from Genius" where required  
- **Fallback:** manual paste from any source if API fails or song is missing  

### v1 recommendation

Ship **manual create + paste** first. Add Genius import in v1.1 once core editor and offline storage work.

---

## 8. Singer & musician focused customization (your edge)

These are reasons *you* build this instead of using generic chord apps:

| Customization | Why it matters for singers & musicians |
|---------------|-----------------------------------------|
| Transpose / capo display | Quickly match vocal range; see guitar capo equivalents |
| Section labels (Verse, Chorus, Bridge, Outro) | Navigate songs effortlessly during live performance |
| Vocal cues and arrangement notes | Keep cues, harmonies, dynamic changes, or breathing reminders |
| Performance mode | Full-screen lyrics, tap to scroll, no accidental edits mid-gig |
| Setlists with key per song | Quick reference for a night's repertoire |
| Display presets | "Rehearsal" vs "Stage" font sizes and contrast |

**Future (not v1):** Nashville number system, metronome BPM per song, audio backing track links, optional root note emphasis.

---

## 9. Non-functional requirements

| Requirement | Target |
|-------------|--------|
| Offline | 100% of saved songs viewable without network |
| Performance | Song list search < 200ms for ~500 songs |
| Readability | Adjustable font 16pt–32pt+; dark theme for dim stages |
| Data safety | Local DB + optional export (JSON) so charts aren't lost |
| Privacy | Personal lyrics library stays on device unless you enable backup |
| Cost | Free tier stack where possible (see `02_technology_recommendations.md`) |

---

## 10. Open decisions (resolve before coding)

| # | Decision | Options | Notes |
|---|----------|---------|-------|
| 1 | Framework | React Native (Expo) vs Flutter | See tech doc; same debate as fitness/finance project |
| 2 | Storage | SQLite local-only vs SQLite + optional Supabase backup | Recommend local-first |
| 3 | Genius in v1? | Yes vs defer to v1.1 | Defer reduces legal/API setup friction |
| 4 | Chord format | ChordPro vs custom JSON lines | ChordPro is portable; custom may be simpler for editor |
| 5 | Transpose engine | Full chord parsing library vs limited set | Start with common chords (maj, min, 7, sus, dim, aug) |
| 6 | Import format | Genius only vs also ChordPro / Ultimate Guitar paste | ChordPro import is high value for v1.1 |

---

## 11. Success criteria (personal)

You will know v1 is successful when:

- [ ] You can create a song with lyrics and chords on each line from scratch  
- [ ] You can open any saved song and read it clearly while performing or rehearsing  
- [ ] You can transpose a song to a new key and chords update correctly  
- [ ] All saved songs work with airplane mode on  
- [ ] You prefer this over your current store app for at least one regular setlist  

---

## 12. Risks and mitigations

| Risk | Mitigation |
|------|------------|
| Scope creep (tabs, audio, social) | Strict v1 Must-have list; advanced extras are v1.1+ |
| Genius API changes or blocks scraping | Manual paste always available; don't depend on API for core flow |
| Chord transpose bugs (C# vs Db) | Use established chord library; test common keys |
| Lyrics licensing anxiety | Personal use only; no public sharing feature in v1 |
| Editor UX is hard (chord alignment) | Start with chord-per-line (not per-syllable); refine later |
| Framework regret | Prototype one song view + one editor screen in week 1 |

---

## 13. Document map

This plan is the anchor. Details live in:

- **Tech choices** → `02_technology_recommendations.md`  
- **Features** → `03_app_outline.md`  
- **Structure** → `04_architecture_outline.md`  
- **Build order** → `05_development_roadmap.md`
