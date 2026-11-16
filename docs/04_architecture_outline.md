# 04 — Architecture Outline

High-level structure for the Lyrics & Chords app: navigation, layers, data flow, and customization system.

Parent context: [01_project_plan.md](./01_project_plan.md) · Features: [03_app_outline.md](./03_app_outline.md)

---

## 1. App layers

```
┌─────────────────────────────────────────────┐
│  UI (screens, components, performance mode) │
├─────────────────────────────────────────────┤
│  State (Zustand stores, TanStack Query)     │
├─────────────────────────────────────────────┤
│  Domain (transpose, chord parse, import)    │
├─────────────────────────────────────────────┤
│  Data (SQLite repos, migrations, export)    │
└─────────────────────────────────────────────┘
```

| Layer | Responsibility |
|-------|----------------|
| **UI** | Screens, chord/lyric renderer, gestures, themes |
| **State** | Current song, transpose offset, UI preferences |
| **Domain** | Pure functions: transpose chords, parse paste, validate song |
| **Data** | CRUD for songs, lines, setlists; migrations |

---

## 2. Navigation structure (Expo Router)

```
app/
├── (tabs)/
│   ├── index.tsx          → Library (song list)
│   ├── setlists.tsx       → Setlists (v1.1; placeholder in v1)
│   └── settings.tsx       → Settings
├── song/
│   └── [id].tsx           → Song view
├── song/
│   └── [id]/
│       └── edit.tsx       → Song editor
├── song/
│   └── new.tsx            → Create song (blank / paste)
└── perform/
    └── [id].tsx           → Performance mode (full screen)
```

**v1:** Hide or disable Setlists tab until v1.1; two tabs (Library, Settings) is fine for early builds.

---

## 3. Folder structure (source)

```
src/
├── components/
│   ├── chords/
│   │   ├── ChordLine.tsx       # Renders chord row above lyrics
│   │   ├── ChordInput.tsx      # Editor chord field
│   │   └── TransposeControls.tsx
│   ├── lyrics/
│   │   ├── LyricLine.tsx
│   │   └── SectionHeader.tsx
│   ├── song/
│   │   ├── SongCard.tsx
│   │   └── SongHeader.tsx
│   └── ui/                     # Button, Input, SearchBar, etc.
├── domain/
│   ├── transpose.ts            # Chord transposition logic
│   ├── chordParser.ts          # Validate chord symbols
│   ├── lyricsParser.ts         # Paste → lines
│   └── chordPro.ts             # Import/export (v1.1)
├── data/
│   ├── db.ts                   # SQLite connection
│   ├── migrations/
│   ├── repositories/
│   │   ├── songRepository.ts
│   │   ├── setlistRepository.ts
│   │   └── settingsRepository.ts
│   └── export.ts               # JSON backup
├── services/
│   └── geniusApi.ts            # v1.1 — search + lyrics fetch
├── stores/
│   ├── songStore.ts
│   └── settingsStore.ts
├── hooks/
│   ├── useSong.ts
│   └── useTranspose.ts
└── types/
    └── song.ts
```

---

## 4. Core data flow

### View song

```
User taps song in Library
  → songRepository.getById(id)
  → load song_lines ordered by sort_order
  → songStore.setCurrentSong(song, lines)
  → Song view renders ChordLine + LyricLine per row
  → User taps transpose +2
  → transpose.apply(lines, +2) in domain layer (non-destructive — display only)
  → UI re-renders transposed chords; original saved in DB unchanged
```

### Edit song

```
User taps Edit
  → Navigate to editor with song id
  → Editable list of lines (lyrics TextInput + chord TextInput)
  → Save → songRepository.update + replace song_lines in transaction
  → updated_at refreshed
```

### Paste lyrics (v1)

```
User chooses "Paste lyrics"
  → Text area input
  → lyricsParser.splitToLines(text)
  → Preview screen (editable line list)
  → Confirm → create song + empty chords → open editor to add chords
```

---

## 5. Chord rendering component

The hardest UI piece — keep v1 simple:

```tsx
// Conceptual structure
<LineRow>
  <ChordRow>
    {line.chords.map(c => (
      <Text key={c.position} style={{ marginLeft: c.position }}>
        {transposedChord(c.chord, offset)}
      </Text>
    ))}
  </ChordRow>
  <LyricRow>{line.lyrics}</LyricRow>
</LineRow>
```

**v1:** `position` is 0 for most lines (chord at start).  
**v1.2+:** Character-index positioning for syllable alignment.

---

## 6. Transpose architecture

| Concern | Approach |
|---------|----------|
| Storage | Always save chords in **original key** |
| Display | Apply transpose offset in memory (Zustand: `transposeSemitones`) |
| Setlist override | `performance_key` on setlist entry → compute offset from `original_key` |
| Enharmonics | User setting `prefer_flats` passed to transpose engine |

```typescript
// domain/transpose.ts — pure function
function transposeChord(chord: string, semitones: number, preferFlats: boolean): string
function transposeSongLines(lines: SongLine[], semitones: number, preferFlats: boolean): DisplayLine[]
```

---

## 7. Offline-first data strategy

| Operation | Network needed? |
|-----------|-----------------|
| List / view / edit songs | No |
| Transpose | No |
| Performance mode | No |
| Genius search | Yes |
| Album art fetch | Yes (first time); cached after |
| JSON export | No |
| Cloud backup (future) | Yes |

**SQLite is source of truth.** No "loading from cloud" on app start for v1.

---

## 8. Customization system

Settings stored in SQLite `settings` table and loaded into `settingsStore` on app launch.

| Setting | Affects |
|---------|---------|
| `lyric_font_size` | Song view, performance mode |
| `chord_font_size` | Chord row |
| `theme` | Global colors |
| `chord_display_mode` | `above` vs `inline` renderer |
| `prefer_flats` | Transpose output |
| `performance_brightness` | Keep screen awake in perform mode (`expo-keep-awake`) |

**Display presets (v1.1):** Named bundles of settings — "Stage" = dark, 28pt lyrics, keep awake.

---

## 9. Genius integration (v1.1) — architecture

```
App (search query)
  → geniusApi.search(query)        // Client or edge function
  → User picks result
  → geniusApi.fetchLyrics(songId)  // Parse to plain text
  → lyricsParser → preview screen
  → User confirms → songRepository.create
```

**Security:** If Genius requires a client secret, route through Supabase Edge Function or minimal ASP.NET endpoint — do not ship secrets in the mobile binary.

---

## 10. Error handling patterns

| Scenario | UX |
|----------|-----|
| Song not found | Empty state in library with "Add song" CTA |
| Invalid chord in editor | Inline warning; still allow save (don't block creativity) |
| Genius offline | Toast: "Offline — paste lyrics instead" |
| DB migration failure | Block app with clear error + export attempt |
| Empty song | Prompt to add lines before performance mode |

---

## 11. Testing strategy (lightweight, personal project)

| Area | Approach |
|------|----------|
| Transpose logic | Unit tests for `transposeChord` — C, C#, Db edge cases |
| Lyrics parser | Unit tests for split, blank lines, section markers |
| Repositories | Optional integration tests with in-memory SQLite |
| UI | Manual testing on one Android + one iOS device |

---

## 12. Relationship to other personal apps

This app is **separate** from the Fitness / Finance Life Hub:

| | Life Hub | Songbook |
|---|----------|----------|
| Domain | Health + money | Music / performance |
| Shared stack? | Can reuse Expo + SQLite patterns | Same skills, different repo recommended |
| Merge apps? | No — keep installs focused |

You may reuse **boilerplate** (Expo setup, UI components, SQLite migration pattern) from future Life Hub work, but song data stays isolated.
