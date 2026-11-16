# 03 — App Outline

Feature breakdown for the **Lyrics & Chords** musician app — view, create, import, and perform songs offline.

Parent context: [01_project_plan.md](./01_project_plan.md) · Tech: [02_technology_recommendations.md](./02_technology_recommendations.md)

---

## 1. Module overview

```
Songbook
├── Library (all songs, search, favorites)
├── Song view (read, transpose, performance mode)
├── Song editor (create / edit lyrics + chords)
├── Setlists (ordered lists for gigs / rehearsals)
├── Import (Genius search — v1.1; paste — v1)
└── Settings (display, theme, backup, about)
```

---

## 2. User stories

### Library & viewing

| ID | Story | Priority |
|----|-------|----------|
| L1 | As a singer/musician, I browse my song library alphabetically or by artist | Must-have |
| L2 | As a singer/musician, I search songs by title or artist | Must-have |
| L3 | As a singer/musician, I open a song and see lyrics with chords above each line | Must-have |
| L4 | As a singer/musician, I mark songs as favorites for quick access | Should-have |
| L5 | As a singer/musician, I filter by key or tag (e.g. "worship", "covers") | Nice-to-have |
| L6 | As a singer/musician, I see song metadata: title, artist, original key, capo | Must-have |

### Song editor (create / customize)

| ID | Story | Priority |
|----|-------|----------|
| E1 | As a singer/musician, I create a new song with title and artist | Must-have |
| E2 | As a singer/musician, I add lyric lines and assign a chord to each line | Must-have |
| E3 | As a singer/musician, I edit existing lyrics and chords | Must-have |
| E4 | As a singer/musician, I add section headers (Verse 1, Chorus, Bridge) | Should-have |
| E5 | As a singer/musician, I set the song's default key and capo | Must-have |
| E6 | As a singer/musician, I add personal notes (e.g. "walk up on bridge", "stop after chorus") | Should-have |
| E7 | As a singer/musician, I duplicate a song to make a variant (different key arrangement) | Nice-to-have |
| E8 | As a singer/musician, I paste lyrics from clipboard and then add chords | Must-have |

### Transpose & performance

| ID | Story | Priority |
|----|-------|----------|
| P1 | As a singer/musician, I transpose a song up or down by semitones | Must-have |
| P2 | As a singer/musician, I transpose to a target key (e.g. Em → Gm) | Must-have |
| P3 | As a singer/musician, I use performance mode: large text, minimal chrome, easy scroll | Must-have |
| P4 | As a singer/musician, I enable auto-scroll with adjustable speed controls (play/pause/speed) | Must-have |
| P5 | As a singer/musician, I tap floating section jump buttons to jump to any section during live play | Must-have |
| P6 | As a singer/musician, I define section repeat counts (e.g. Chorus 2x) or structured song flow | Must-have |
| P7 | As a singer/musician, I lock the screen from accidental touches during performance | Should-have |
| P8 | As a singer/musician, I see both guitar chord and Nashville number (future) | Future |

### Import (Genius & paste)

| ID | Story | Priority |
|----|-------|----------|
| I1 | As a singer/musician, I paste plain lyrics and save as a new song | Must-have |
| I2 | As a singer/musician, I search Genius for a song and import lyrics (v1.1) | Should-have |
| I3 | As a singer/musician, I review imported lyrics before saving | Should-have |
| I4 | As a singer/musician, I import ChordPro format files (v1.1) | Nice-to-have |
| I5 | As a singer/musician, I export a song as ChordPro or JSON | Nice-to-have |

### Setlists

| ID | Story | Priority |
|----|-------|----------|
| S1 | As a singer/musician, I create a setlist with an ordered list of songs | Should-have |
| S2 | As a singer/musician, I open a setlist and tap through songs in order during a gig | Should-have |
| S3 | As a singer/musician, I override the key for a song within a setlist | Nice-to-have |
| S4 | As a singer/musician, I add setlist notes (venue, date, tempo reminders) | Nice-to-have |

### Offline & data

| ID | Story | Priority |
|----|-------|----------|
| D1 | As a singer/musician, I view all saved songs without internet | Must-have |
| D2 | As a singer/musician, I export my library to a JSON file | Should-have |
| D3 | As a singer/musician, I import a backup JSON file on a new phone | Should-have |
| D4 | As a singer/musician, I see when a song was last edited | Nice-to-have |

### Settings & customization

| ID | Story | Priority |
|----|-------|----------|
| C1 | As a singer/musician, I change font size for lyrics and chords | Must-have |
| C2 | As a singer/musician, I switch dark / light theme | Must-have |
| C3 | As a singer/musician, I choose chord display: above lyrics vs inline suffix | Should-have |
| C4 | As a singer/musician, I save display presets ("Practice" vs "Stage") | Nice-to-have |
| C5 | As a singer/musician, I prefer sharps or flats when transposing | Should-have |

---

## 3. Screens (wireframe-level)

| Screen | Purpose | v1? |
|--------|---------|-----|
| Library home | Song list, search bar, favorites filter | Yes |
| Song view | Lyrics + chords, transpose controls, edit button | Yes |
| Performance mode | Full-screen song view, scroll, exit gesture | Yes |
| Song editor | Title, artist, key, lines with chord inputs | Yes |
| Add song | Choose: blank / paste lyrics | Yes |
| Paste lyrics | Text area → preview lines → editor | Yes |
| Setlist list | All setlists | v1.1 |
| Setlist detail | Ordered songs, tap to open | v1.1 |
| Setlist editor | Reorder songs, add/remove | v1.1 |
| Genius search | Search, pick result, import (v1.1) | v1.1 |
| Settings | Font, theme, transpose prefs, export | Yes |
| Export / backup | Export JSON, share sheet | v1.1 |

---

## 4. Song view layout (core UX)

The main screen you will use while playing:

```
┌─────────────────────────────────────┐
│  Wonderwall — Oasis          [♥][⚙] │
│  Key: Em  ▼  [−] [+]  [▶ Perform]   │
├─────────────────────────────────────┤
│                                     │
│  [Verse 1]                          │
│  Em7        G                       │
│  Today is gonna be the day          │
│  Dsus4      A7sus4                  │
│  That they're gonna throw it back   │
│                                     │
│  [Chorus]                           │
│  Cadd9    Dsus4   Em7               │
│  And all the roads we have to walk  │
│  ...                                │
│                                     │
└─────────────────────────────────────┘
```

**v1 simplification:** One chord (or chord sequence) per line, aligned at line start. Per-syllable alignment is **Nice-to-have** later.

---

## 5. Data model sketch

### `songs`

| Column | Type | Notes |
|--------|------|-------|
| id | UUID | Primary key |
| title | text | Required |
| artist | text | Optional |
| original_key | text | e.g. `Em` |
| capo | integer | 0 = none |
| notes | text | Personal performance notes |
| is_favorite | boolean | Default false |
| created_at | datetime | |
| updated_at | datetime | |

### `song_lines`

| Column | Type | Notes |
|--------|------|-------|
| id | UUID | Primary key |
| song_id | UUID | FK → songs |
| sort_order | integer | Line order |
| section_label | text | Nullable — "Verse 1", "Chorus" |
| lyrics | text | One line of lyrics |
| chords | text (JSON) | `[{ "chord": "Em7", "position": 0 }]` |

### `setlists` (v1.1)

| Column | Type | Notes |
|--------|------|-------|
| id | UUID | |
| name | text | e.g. "Friday gig" |
| event_date | date | Optional |
| notes | text | |

### `setlist_songs` (v1.1)

| Column | Type | Notes |
|--------|------|-------|
| setlist_id | UUID | |
| song_id | UUID | |
| sort_order | integer | |
| performance_key | text | Optional override |

### `settings` (key-value)

| Key examples | Value |
|--------------|-------|
| `theme` | `dark` / `light` |
| `lyric_font_size` | `20` |
| `chord_font_size` | `16` |
| `prefer_flats` | `true` / `false` |
| `chord_display_mode` | `above` / `inline` |

---

## 6. Singer & musician specific features (prioritized)

| Feature | Priority | Notes |
|---------|----------|-------|
| Clear key + transpose | Must-have | Essential to match vocal range and band keys |
| Section navigation | Should-have | Jump to Chorus or Bridge during live play |
| Large performance text | Must-have | Read lyrics and chords clearly on stage |
| Personal notes per song | Should-have | Vocal cues, dynamic shifts, breathing reminders |
| Chord display options | Should-have | Chords above lyrics or inline |
| BPM / tempo field | Nice-to-have | Tempo cues (no metronome in v1) |
| Nashville numbers | Future | For jazz/worship contexts |

---

## 7. v1 scope cut (recommended)

### Must ship in v1

- Library (list, search, open song)  
- Song view with chords above lyrics  
- Transpose (semitone + target key)  
- Song editor (create, edit, delete)  
- Section labels & repeat multipliers (e.g. Chorus 2x)  
- Structured arrangement flow & live section jump chips  
- Auto-scroll with speed controls in Performance mode  
- Paste lyrics flow  
- Performance mode (large, dark-friendly, keep awake)  
- Font size, theme & scroll speed settings  
- SQLite persistence, full offline  

### Defer to v1.1

- Genius API import  
- Setlists  
- JSON export/import backup  
- ChordPro import/export  
- Tags / categories  

### Future

- Bass tab support  
- Audio / backing tracks  
- Cloud sync  
- Widget for quick setlist access  

---

## 8. Competitive reference (what to borrow, what to avoid)

| App | Borrow | Avoid |
|-----|--------|-------|
| Ultimate Guitar | Chord-above-lyrics layout, transpose | Ads, subscription walls, crowded UI |
| OnSong / Planning Center | Setlist flow, performance mode | Complexity, price, worship-only focus |
| iReal Pro | Clean charts, setlists | Jazz-only, no lyrics focus |
| Chordify | — | Not editable enough |

**Your differentiator:** Full control, musician-first simplicity, offline-first, no paywalls — because you own the code.
