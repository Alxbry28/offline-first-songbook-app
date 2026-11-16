# 05 — Development Roadmap

Phased plan to build the Lyrics & Chords app **without trying to ship everything at once**.

Assumes recommended stack: **Expo + TypeScript + SQLite**.

---

## Phase 0 — Decisions & setup (Week 1)

**Goal:** Frozen stack choices and empty runnable app.

| Task | Output |
|------|--------|
| Confirm Expo vs Flutter | Decision recorded |
| Install Node, Expo CLI, emulators | Dev environment ready |
| Create git repo | Initial commit (scaffold only) |
| `npx create-expo-app` with TypeScript | App launches in Expo Go |
| Define folder structure | Matches `04_architecture_outline.md` |
| Choose UI kit | e.g. NativeWind or React Native Paper |
| Seed one hardcoded song in code | Validates chord-above-lyrics layout |

**Exit criteria:** Blank app runs; one static song screen looks readable on your phone.

---

## Phase 1 — Foundation (Week 2)

**Goal:** Database, navigation shell, and base components.

| Task | Priority |
|------|----------|
| SQLite setup + migration runner | Must |
| `songs` + `song_lines` schema | Must |
| Tab navigation: Library, Settings | Must |
| Base components: Button, Input, ListRow, SearchBar | Must |
| Settings repository (theme, font size) | Must |
| Zod schemas for Song, SongLine | Must |

**Exit criteria:** App opens to Library tab; settings persist across restart.

---

## Phase 2 — Song view & transpose (Weeks 3–4)

**Goal:** Read songs with working transpose — the core playing experience.

| Order | Feature |
|-------|---------|
| 2.1 | `songRepository` — CRUD + seed 2–3 sample songs |
| 2.2 | Library list with search |
| 2.3 | Song view screen — chord + lyric line renderer |
| 2.4 | `domain/transpose.ts` + unit tests |
| 2.5 | Transpose controls on song view (± semitones, pick key) |
| 2.6 | Dark theme + adjustable font sizes in Settings |
| 2.7 | Performance mode (full screen, large text, keep awake) |
| 2.8 | Auto-scroll with adjustable speed controls (play/pause/speed) |
| 2.9 | Live section jump navigation chips in Performance mode |

**Exit criteria:** Open a song, transpose Em → Gm, enter performance mode, read comfortably offline.

---

## Phase 3 — Song editor (Weeks 5–6)

**Goal:** Create and edit your own charts — the main reason you build this.

| Order | Feature |
|-------|---------|
| 3.1 | New song flow (title, artist, key) |
| 3.2 | Line editor — add/remove/reorder lyric lines |
| 3.3 | Chord input per line |
| 3.4 | Section labels (Verse, Chorus) with repeat multipliers (e.g. 2x) |
| 3.5 | Structured arrangement flow expansion |
| 3.5 | Personal notes field on song |
| 3.6 | Edit existing song |
| 3.7 | Delete song with confirmation |
| 3.8 | Paste lyrics → preview → editor |

**Exit criteria:** Create a song from scratch, add chords, save, view in performance mode — all offline.

---

## Phase 4 — Polish & v1 release (Week 7)

**Goal:** Daily-usable v1 you reach for instead of a store app.

| Order | Feature |
|-------|---------|
| 4.1 | Favorites toggle on songs |
| 4.2 | Empty states and loading skeletons |
| 4.3 | Confirm dialogs for destructive actions |
| 4.4 | Onboarding tip: "Create your first song" |
| 4.5 | App icon and splash screen |
| 4.6 | EAS Build → install on your phone |
| 4.7 | Import your real setlist (5–10 songs) manually |

**Exit criteria:** You use the app for one real practice session.

---

## Phase 5 — v1.1 enhancements (Weeks 8–10)

**Goal:** Import, setlists, and backup.

| Order | Feature |
|-------|---------|
| 5.1 | JSON export / import backup |
| 5.2 | Setlists CRUD + ordered song navigation |
| 5.3 | Genius API search + lyrics import |
| 5.4 | ChordPro import (basic) |
| 5.5 | Display presets (Practice / Stage) |

**Exit criteria:** Build a gig setlist; import one song from Genius; backup library to JSON.

---

## Phase 6 — Extended musician depth (later)

**Goal:** Advanced features for singers and multi-instrumentalists.

| Feature | Notes |
|---------|-------|
| Section jump buttons | Quick nav during live play |
| Root chord highlighting | Visual emphasis on bass notes |
| BPM field per song | No metronome yet |
| Nashville number display | Optional second row |
| Bass tab lines | Separate line type in editor |
| Optional Supabase backup | Cloud restore on new device |

---

## Milestone summary

| Milestone | Target | You can… |
|-----------|--------|----------|
| **M0** | Week 1 | Run app, see static song layout |
| **M1** | Week 2 | DB + navigation work |
| **M2** | Week 4 | View + transpose songs offline |
| **M3** | Week 6 | Create and edit your own songs |
| **v1** | Week 7 | Replace store app for practice |
| **v1.1** | Week 10 | Setlists + Genius + backup |

---

## What to build first (if time is limited)

If you only have a few hours per week, **narrowest path to value:**

1. SQLite + one song view with hardcoded data  
2. Transpose working  
3. Simple editor (add lines + chords)  
4. Performance mode  

Everything else (Genius, setlists, export) can wait until you are actually using the app and feel what's missing.

---

## Suggested first song for testing

Pick a song you play often on bass — something with:

- Clear verse / chorus structure  
- 4–8 chord changes you know well  
- Lyrics you want visible on stage  

Entering *your* real material early makes it obvious which UI tweaks matter (font size, scroll, transpose defaults).

---

## Risks to watch during build

| Phase | Risk | Mitigation |
|-------|------|------------|
| 2 | Chord layout looks wrong on small screens | Test on your actual phone early |
| 3 | Editor feels tedious | Paste lyrics first; optimize add-line flow |
| 4 | Scope creep into tabs/audio | Stick to roadmap Must-have |
| 5 | Genius parsing breaks | Keep manual paste as primary path |

---

## Done definition for v1

- [ ] 10+ songs in library, entered by you  
- [ ] Used in at least one rehearsal or practice  
- [ ] Works in airplane mode  
- [ ] Transpose used at least once for band key  
- [ ] You changed a display setting and it improved readability  

When all checked — v1 is real. Ship v1.1 based on what annoyed you during that rehearsal.
