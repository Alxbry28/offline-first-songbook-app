# 02 — Technology Recommendations

This document answers open questions from [project_idea.md](../project_idea.md): **React Native vs Flutter**, **free database**, **Genius API**, and **offline strategy** for a mobile-only musician app.

---

## 1. Executive recommendation

| Layer | Recommendation | Why |
|-------|----------------|-----|
| **Mobile framework** | **React Native with Expo** | Matches your other personal app direction; great for text-heavy CRUD screens, offline SQLite, and rapid iteration |
| **Local database** | **SQLite** (via `expo-sqlite` or `op-sqlite`) | Offline-first song library; no network required for daily use |
| **Optional cloud backup** | **Supabase free tier** or export JSON to device/files | Personal backup without making cloud mandatory |
| **Chord parsing / transpose** | **`chord-transposer`** or **`@tonaljs/tonal`** (npm) | Handles key changes reliably |
| **Lyrics import** | **Genius API** (v1.1) + manual paste (v1) | API for convenience; paste for reliability |
| **State management** | **Zustand** + **TanStack Query** (for API calls only) | Simple local state; cache Genius responses briefly |
| **Navigation** | **Expo Router** | File-based routes fit Library → Song → Editor flow |
| **Rich text / layout** | Custom React Native views (not WebView) | Chord-above-lyrics layout needs precise control |

**Runner-up stack:** Flutter + `sqflite` + `flutter_genius_api` — excellent if you prefer Dart and want pixel-perfect typography.

---

## 2. React Native vs Flutter

### Comparison (musician app context)

| Criteria | React Native (Expo) | Flutter |
|----------|---------------------|---------|
| Language | TypeScript | Dart |
| Offline SQLite | `expo-sqlite`, mature | `sqflite`, mature |
| Text layout (chords above lyrics) | Flexbox; may need custom line renderer | `RichText` / custom painters; strong control |
| Genius / HTTP | `fetch` + axios patterns | `http` / `dio` |
| Export / share file | `expo-file-system` + `expo-sharing` | `path_provider` + `share_plus` |
| Learning curve (if learning React anyway) | Lower — aligns with fitness/finance app | New language |
| Performance for 500+ songs | More than enough | More than enough |

### Recommendation

**React Native (Expo)** — especially if you are already leaning Expo for your other personal apps. One toolchain across projects.

**Prototype in week 1:** One screen that displays a hardcoded song (lyrics + chords) with transpose button working.

---

## 3. Database — free and offline-first

### Primary: SQLite on device (recommended)

| Aspect | Detail |
|--------|--------|
| Cost | Free |
| Offline | Native — no sync required |
| Fit | Perfect for personal song library |
| Expo | `expo-sqlite` with simple migration scripts |

### Optional: Supabase (free tier) for backup only

| Aspect | Detail |
|--------|--------|
| Use case | Backup songs + setlists to cloud; restore on new phone |
| Not for | Real-time collaboration or public song sharing |
| Free tier | Enough for single-user personal library |

### Data you store locally

- Songs (title, artist, key, capo, lyrics lines, chords per line, sections)  
- Setlists (ordered song IDs, notes, event date)  
- Display preferences (font size, theme, scroll speed)  
- Genius cache (optional, TTL) — song metadata to reduce API calls  

**Recommendation:** **SQLite only for v1.** Add export-to-JSON and optional Supabase sync in v1.1.

---

## 4. Genius API

### What it provides

- Song search by title/artist  
- Metadata (title, artist, album art, URL)  
- Path to lyrics content (often requires fetching the Genius page and parsing — check current API docs and terms)

### What it does *not* provide

- Chord progressions  
- Guaranteed clean lyric text (may need HTML cleanup)  
- Commercial redistribution rights  

### Integration approach

```
App → Genius Search API → song hit
App → fetch lyrics page / API field → parse to plain text
App → Editor (user adds chords) → save to SQLite
```

### Legal / practical notes

1. **Personal use** — building a private songbook for yourself is the intended use case  
2. **Attribution** — Genius terms may require "Powered by Genius" or similar; add to import screen  
3. **Rate limits** — cache search results; don't re-fetch on every app open  
4. **Fallback** — always allow paste lyrics manually  

### v1 recommendation

**Defer Genius to v1.1.** v1 ships with manual song creation and paste. This unblocks you without API keys or parsing complexity on day one.

---

## 5. Chord and lyric format

### Option A — ChordPro (recommended long-term)

```
{title: Wonderwall}
{artist: Oasis}
{key: Em}
[Em7]Today is gonna be the day
[G]That they're gonna throw it back to you
```

**Pros:** portable, import/export compatible with other tools  
**Cons:** editor UX must parse bracket notation  

### Option B — Custom JSON lines (recommended for v1 editor)

```json
{
  "lines": [
    { "lyrics": "Today is gonna be the day", "chords": [{ "chord": "Em7", "position": 0 }] },
    { "lyrics": "That they're gonna throw it back to you", "chords": [{ "chord": "G", "position": 0 }] }
  ]
}
```

**Pros:** easiest to build editor and transpose logic  
**Cons:** need export converter for ChordPro later  

**Recommendation:** Store as **JSON lines internally**; add **ChordPro import/export** in v1.1.

---

## 6. Transposition

Use a library rather than rolling your own:

| Library | Notes |
|---------|-------|
| `@tonaljs/tonal` | Music theory primitives; chord parsing and intervals |
| `chord-transposer` | Simple semitone shift for chord symbols |
| `react-chords` | Reference for display (web-oriented; adapt patterns) |

**v1 scope:** transpose all chords in a song by ± semitones or to a target key. Handle sharps/flats based on user preference (e.g. prefer flats for Bb instruments).

---

## 7. Offline strategy

| Layer | Strategy |
|-------|----------|
| Song library | 100% SQLite — always available |
| Genius search | Online only; graceful "offline — use manual entry" message |
| Images (album art) | Cache to filesystem after first fetch; optional disable for storage |
| Backup | Manual JSON export to Files app / Google Drive (user-initiated) |

**Performance mode** must never require network.

---

## 8. UI and display tooling

| Need | Tool |
|------|------|
| Theme / dark mode | NativeWind or React Native Paper |
| Fonts | System monospace for chords; sans-serif for lyrics — or bundled **JetBrains Mono** + **Inter** |
| Full-screen performance | Hide status bar / nav; `expo-screen-orientation` if you want landscape |
| Auto-scroll | `ScrollView` + `Animated` timer; speed in settings |
| Haptics (optional) | `expo-haptics` on section change during scroll |

---

## 9. Development tooling

| Tool | Purpose |
|------|---------|
| TypeScript | Type-safe song model |
| Zod | Validate song JSON on import/export |
| ESLint + Prettier | Consistent code style |
| EAS Build | Personal iOS/Android installs without store |
| Expo Go | Fast dev iteration for UI |

---

## 10. Cost estimate (personal use)

| Item | Cost |
|------|------|
| Expo / React Native | Free |
| SQLite | Free |
| Supabase (optional) | Free tier |
| Genius API | Free tier with limits (verify current Genius developer terms) |
| Apple Developer (iOS sideload / TestFlight) | $99/year if you want iPhone install |
| Google Play (Android) | $25 one-time if you publish (optional for personal APK) |

**Expected monthly cost for v1:** **$0** if local-only and no paid AI features.

---

## 11. Security and privacy

- No accounts required for v1  
- Lyrics and setlists stay on device  
- If adding Supabase backup: row-level security with single user ID  
- Do not embed Genius client secret in app — use a thin backend or client-access-token flow per Genius docs  

---

## 12. Decision summary

| Decision | Recommendation |
|----------|----------------|
| Framework | **Expo (React Native) + TypeScript** |
| Database | **SQLite local-first** |
| Genius API | **v1.1** (after editor works) |
| Chord storage | **JSON lines** internally; ChordPro later |
| Cloud | **Optional backup later**; export JSON in v1.1 |
