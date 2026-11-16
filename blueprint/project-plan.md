# Project Plan

## 1. Problem - What problem are we solving?

Musicians and singers lack flexible control over how songs with lyrics and chords are displayed, formatted, and transposed during rehearsals, practice, and live performances. Mainstream store apps enforce rigid layouts with small or misaligned chord labels, gate essential features (like offline access) behind recurring subscriptions, add social and commercial clutter, and make it cumbersome to transpose songs to fit a singer's vocal range or a band's preferred key.

Furthermore, performers on stage need hands-free lyrics scrolling and the ability to manage song structure—both structured repeats (e.g., Chorus 2x) and impromptu navigation when a band improvises or loops a section on the fly.

This app is a dedicated, offline-first personal songbook providing musicians and singers complete control over layout, line-by-line chord positioning, instant key transposition, auto-scrolling, section repeats, and live stage navigation.

## 2. Users - Who is this for?

- **Singers & Musicians:** Solo performers, vocalists, and acoustic/band musicians managing their personal songbook who need high-contrast readable lyric and chord charts on stage, hands-free auto-scrolling, quick vocal key transposition, repeat indicators, and 100% offline reliability.
- **Access Model:** Single-user personal app; all data stored locally on-device. No accounts, authentication, or multi-tenant requirements for v1.

## 3. Features - What does the MVP need?

- **Local Song Library:** Fast searchable catalog of saved songs with filtering and favorites.
- **Song Viewer:** High-contrast display rendering chords aligned above lyric lines with customizable typography.
- **Transpose Engine:** Instant pitch transposition by semitones or target key to match vocal range, with sharp/flat preferences.
- **Performance Mode:** Full-screen stage mode with large text, dark theme, and screen wake-lock.
- **Auto-Scroll & Speed Control:** Smooth hands-free scrolling with adjustable speed controls (play/pause, speed up/down, tap-tempo).
- **Section Repeats & Arrangement Flow:** Support for repeat counters on sections (e.g. "Chorus 2x") and structured song flow.
- **Live Section Jump:** Interactive quick-jump bar (`[V1]`, `[Chorus]`, `[Bridge]`) to immediately jump to any section during live band improvisation.
- **Song Editor:** Line-by-line editor to input lyrics, assign chords per line, specify section markers with repeat counts, and add performance cues.
- **Paste Import:** Quick creation flow allowing raw lyrics pasted from clipboard to be parsed into editable lines.
- **Offline-First Storage:** Complete local availability of all songs, metadata, and settings without network dependency.

## 4. Data - What are we storing?

- **Song:** `id`, `title`, `artist`, `original_key`, `capo`, `scroll_speed`, `arrangement`, `notes`, `is_favorite`, `created_at`, `updated_at`.
- **SongLine:** `id`, `song_id`, `line_order`, `section`, `repeat_count`, `lyrics`, `chords`.
- **Settings:** `theme` (light/dark), `font_size`, `display_mode` (chords above vs inline), `prefer_sharps`, `default_scroll_speed`.

*(Setlists and Genius API cache deferred to v1.1).*

## 5. Tech - What stack are we using?

- **Framework:** React Native with Expo (TypeScript, Expo Router).
- **Local Database:** SQLite via `expo-sqlite`.
- **State Management:** Zustand.
- **Music Theory / Transposition:** `@tonaljs/tonal` or `chord-transposer`.
- **Styling:** React Native StyleSheet / NativeWind for responsive, themeable UI.
- **Target Platform:** Mobile (iOS and Android).

## 6. Monetize - How will this make money?

Not monetized. This is a personal utility app built for personal musical workflow, free of ads, trackers, and subscription paywalls.

## 7. UI/UX - How should this look and feel?

High-contrast, clean, and legible from a music stand or microphone stand:
- **Routes:**
  - `/(tabs)`: Library (`index.tsx`) and Settings (`settings.tsx`).
  - `/song/[id]`: Song view with transpose controls and section quick-jump.
  - `/song/new`: New song creation (manual entry or paste lyrics).
  - `/song/[id]/edit`: Song editor with section labeling, repeat tags, and chord inputs.
  - `/perform/[id]`: Full-screen Performance mode with auto-scroll speed controls and floating section jump buttons for improvisation.
- **Visuals:** Dark mode optimized for dim stage lighting, large adjustable fonts (16pt–32pt+), and uncluttered UI controls.

## 8. Deployment - Where and how will this ship?

- **Target:** Personal mobile devices (iOS & Android).
- **Build / Packaging:** Expo Application Services (EAS Build) for local release APK / iOS test build, and Expo Go for local development.
- **Backend / Hosting:** None required in v1; 100% on-device client application.

## 9. Usage model and constraints (optional)

- **Scale:** Personal library (~500 songs max).
- **Environment:** 100% offline; rehearsals and gigs often have zero connectivity.
- **Security & Trust:** Single-user trusted environment; all song content stays private on local device storage.
