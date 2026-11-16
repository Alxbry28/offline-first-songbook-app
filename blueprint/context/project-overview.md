# Lyrics & Chords - Project Overview

<!-- blueprint:source-hash 6c838ca00ff2b79cc571c3e2767b31e1b93e4531a7577ea22baf61e265fee4ac -->

> An offline-first personal mobile songbook for singers and musicians to view, edit, transpose, auto-scroll, and perform songs with customized lyric, chord, and repeat layouts.

## Problem

Singers and musicians lack mobile control over their chord charts and lyric sheets on stage and during rehearsals. Mainstream store apps feature fixed and cluttered layouts, poor chord alignment for reading lyrics and chords simultaneously, aggressive monetization, and brittle offline support. Transposing to fit a singer's vocal range is often cumbersome, and reading during live performance is interrupted by manual page scrolling and rigid song structures that cannot handle section repeats (e.g. Chorus 2x) or live band improvisation.

This app provides full ownership over song layout, line-by-line chord positioning, instant key transposition, hands-free auto-scrolling, section repeats, and live stage navigation.

## Users

- **Singers & Musicians:** Solo performers, vocalists, and acoustic/band musicians needing high-contrast readable chord charts on stage, hands-free auto-scrolling, rapid vocal key transposition during rehearsals, repeat indicators, and 100% offline access at gigs.
- **Access Tiers:** Single user only. All data lives on the device with no authentication, remote server, or multi-tenant complexity.

## Usage model

- **Scale:** Personal songbook of ~500 songs.
- **Connectivity:** 100% offline-first. All core features (viewing, transposing, editing, auto-scrolling, library search) operate with zero network connection.
- **Trust & Privacy:** Trusted single-user execution environment. Song lyrics and chord arrangements remain strictly local on-device.

## Features

The MVP feature set in build-plan order:

1. **Database and migrations** - SQLite schema setup for songs, song_lines, and settings with migration runner.
2. **Tab navigation shell** - Expo Router navigation for Library and Settings tabs with base UI components.
3. **Song repository and seed data** - SQLite CRUD operations for songs and initial sample song seeding.
4. **Library song list** - Searchable song list screen with artist and title filtering.
5. **Chord and lyric renderer** - Song view component displaying chords aligned above lyric lines (*Headline feature*).
6. **Transpose engine** - Pure domain transpose logic with unit tests for semitone and target key shifts.
7. **Transpose controls** - Interactive pitch adjustment (± semitones and key selector) on song view.
8. **Display settings and themes** - Dark mode and adjustable font sizes persisting in settings.
9. **Performance mode** - Full-screen distraction-free song display with keep-awake enabled.
10. **Auto-scroll and speed controls** - Hands-free smooth scrolling with play, pause, and adjustable speed controls.
11. **Section jump navigation** - Floating section quick-jump chips for live improvisation during performance.
12. **Song creation flow** - New song screen to enter title, artist, default key, and capo.
13. **Lyric and chord editor** - Line-by-line editor to add, edit, and reorder lyric lines with chords.
14. **Section headers, repeats, and song notes** - Section tagging with repeat multipliers (e.g. 2x) and performance notes.
15. **Arrangement flow expansion** - Unroll structured song flow repeats for seamless linear reading.
16. **Paste lyrics import** - Paste raw text from clipboard and auto-split into editable lyric lines.
17. **Song editing and deletion** - Edit existing songs and delete with confirmation dialogs.
18. **Song favorites** - Quick favorite toggle in library and filter by favorites.

## Data model

### Song
- `id` (string, primary key, uuid) - Unique identifier
- `title` (string, required) - Song title
- `artist` (string, optional) - Artist or songwriter name
- `original_key` (string, optional) - Base musical key (e.g. `C`, `Em`, `F#m`)
- `capo` (integer, default 0) - Capo fret position indicator
- `scroll_speed` (integer, default 3) - Saved auto-scroll speed for this song
- `arrangement` (string, optional) - JSON string array of section order (e.g. `["V1", "Ch", "V2", "Ch", "Br", "Ch"]`)
- `notes` (string, optional) - Performance or vocal arrangement cues
- `is_favorite` (boolean, default false) - Starred status in library
- `created_at` (string, ISO timestamp) - Creation time
- `updated_at` (string, ISO timestamp) - Last updated time
- **Relationship:** One-to-many with `SongLine` (cascade delete)

### SongLine
- `id` (string, primary key, uuid) - Unique identifier
- `song_id` (string, foreign key -> Song.id) - Parent song reference
- `line_order` (integer, required) - Zero-based sequence order in song
- `section` (string, optional) - Section label (e.g. `Verse 1`, `Chorus`, `Bridge`, `Outro`)
- `repeat_count` (integer, default 1) - Section repetition multiplier (e.g. 2 for 2x repeat)
- `lyrics` (string, optional) - Lyric line text
- `chords` (string, optional) - Chord annotations corresponding to this lyric line
- **Relationship:** Many-to-one with `Song`

### Settings
- `theme` (string, enum: `'light' | 'dark' | 'system'`) - Active theme mode
- `font_size` (integer, default 18) - Base font size in points for lyrics
- `display_mode` (string, enum: `'above' | 'inline'`) - Chord positioning relative to lyric line
- `prefer_sharps` (boolean, default true) - Accidental preference when transposing
- `default_scroll_speed` (integer, default 3) - Default auto-scroll speed

## Tech stack

- **React Native with Expo** - Cross-platform mobile framework (TypeScript, Expo Router)
- **SQLite (`expo-sqlite`)** - Local relational storage engine for offline-first data
- **Zustand** - Client state management for active song, UI preferences, and filters
- **`@tonaljs/tonal` or `chord-transposer`** - Music theory library for key transposition and chord normalization
- **React Native StyleSheet / NativeWind** - Responsive, high-contrast, theme-aware layout

## Monetization

Not in v1. This is a personal utility app without advertisements, paywalls, or subscriptions.

## UI/UX

High-contrast, distraction-free stage ergonomics:
- `/(tabs)/index` - Song library list with real-time search, sorting, and favorites toggle
- `/(tabs)/settings` - Preferences for theme, typography size, chord display modes, and default scroll speed
- `/song/[id]` - Primary chart reader with inline transpose bar, repeat indicators, and live section jump
- `/song/new` - Creation screen with manual line input or clipboard paste importer
- `/song/[id]/edit` - Line-by-line editor with section tagging, repeat multiplier (`2x`), and chord inputs
- `/perform/[id]` - Stage-optimized view: edge-to-edge full screen, keep-awake, floating play/pause/speed controls for auto-scroll, and quick-jump section chips for live improvisation

## Deployment

- **Platform:** iOS and Android
- **Build / Packaging:** Expo Application Services (EAS Build) for production APK / IPA; Expo Go for local development
- **Backend / Servers:** None; zero hosting costs and complete offline autonomy
