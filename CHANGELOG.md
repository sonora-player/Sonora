# Changelog

## 1.0.8

### Multichannel output

- Mono files on a 6- or 8-channel interface play from L / R only, instead of every output
- Downmix folds 5.1 / 7.1 to L / R even when the device stays multichannel, and leaves LFE out of the fold
- Mono listen folds every channel the same way (centre and surrounds included), and stays on L / R
- Mono to center (toolbar C, or Preferences → Sound) sends mono files and Mono listen to the center speaker. The control is available only when the output has more than 2 channels

## 1.0.7

### Volume

- The volume slider can show dB attenuation instead of percent (right-click the slider, or Preferences → Sound → Volume)
- Presets, the drag readout and the ↑ / ↓ keys follow the chosen mode (5 % or 1 dB per step)

### File List

- After starting playback, scrolling away no longer snaps the list back to the playing row when loudness or duration values arrive
- While LUFS is still being computed under a loudness sort, the list no longer jumps away from the top or chases a row that was just re-sorted far away
- Locate (Ctrl+L) now also scrolls the File List to the file, centred in the viewport, in addition to revealing its folder in the Browser

## 1.0.6

### Selection fades

- Drag the square handles at the head and tail of a selection to set fade in / fade out; the fade is auditioned live and nothing is written until you export or drag the clip out
- Five fade curves (Linear, Equal power, Exponential, Logarithmic, S-curve) selectable in the selection bar and the Export Selection dialog
- Exported and dragged-out clips bake the same fades and curve

### Waveform zoom

- Ctrl + mouse wheel zooms the waveform around the cursor; plain wheel pans when zoomed in
- Hotkeys: Ctrl+= / Ctrl+- zoom, Ctrl+0 fit, Z zoom to selection (all remappable in Preferences → Hotkeys)
- Zoomed views re-read peaks from the file for full detail

### Clip naming

- Export Selection now has its own filename scheme (or can follow the drag-out scheme); the suggested filename follows it
- Naming chips can be reordered by dragging; free-text chips stay separate instead of merging into neighbours
- New Separator option ( `_` by default) is inserted automatically between adjacent attributes
- Time attributes now include their unit (1500ms, 1.500s); the default drag scheme is `name_1500ms-3200ms`
- Dragged-out clips no longer get a timestamp suffix — duplicates get " (2)", " (3)" …

### Meters

- Vectorscope axis labels (M / S / L / R) are placed clear of the axes and no longer disappear in narrow panels

## 1.0.5

### File List

- New Refresh button in the File List header picks up files added to the current folder
- Show in Explorer no longer opens Documents when the file name contains a space
- Locate and Show in Explorer now act on the selected row first, falling back to the playing file

## 1.0.4

### Browser

- Refresh now re-reads the Browser tree, so newly created folders show up
- Locate jumps to the current file’s folder in the tree and scrolls it into view

## 1.0.3

### Stability

- Pause, Resume, and Stop during a track switch now stick — the last action you took wins
- If a new file fails to load, the UI stays on the file that is still playing
- Keep Position no longer goes silent when switching to a very long file
- Scrubbing during a slow load is kept after the new file takes over

## 1.0.2

### Playback

- Switching tracks is now seamless: the current track keeps playing while the next one is decoded and prepared, so Keep Position A/B comparison has no gap
- A failed load no longer stops the track that is already playing

### Updates

- Update install no longer fails when the GitHub API is rate limited — the installer digest is read from a `.sha256` file published next to each asset

## 1.0.1

### Loudness

- Peak column (sample peak, dBFS) is shown by default — measured with every loudness analysis
- New `{peak}` token for Batch Rename
- True Peak (dBTP) uses BS.1770 4× oversampling and is analysed only when that column is shown
- Manual clarifies LUFS-I vs DAW dual-mono / partial-gate differences
- Added [REAPER loudness feedback notes](docs/REAPER-loudness-feedback.md) for reporting offline LUFS-I discrepancies

### Interface

- Dark theme: native title bar and scrollbars follow the app palette

## 1.0.0

### Performance

- Smoother browsing and playback in large folders
- Waveform and meters use less CPU while playing, with fewer UI stalls
- Very long tracks open and play without freezing or exhausting memory
- More reliable streaming: underruns no longer skip audio; unknown duration no longer ends playback immediately
- Long sessions use fewer resources, with fewer stutters and glitches
- More accurate waveform peaks; clearer errors for damaged files

### Stability & security

- Copy, move, delete, and drive listing are less likely to freeze the UI
- DirectSound failures show a clear message instead of silent output
- Safer auto-updates: installer URL and checks are decided in-app; missing signature or digest refuses install
- Rename, delete, and copy stay inside folders you have opened
- Dragging from network paths to local is less likely to crash

## 0.9.9

- Drag under the waveform stage to resize height (with min/max limits)
- Main / secondary waveform split ratio is now draggable
- Secondary wave mode button idle opacity raised to 38%
- Batch Rename: Capitalize Words
- Loudness / Peak / Correlation meters adapt when the stage is short (less stacked / clipped text)
- Spectrogram time/frequency resolution modestly improved; less mosaic when zoomed
- What's New after updates; status-bar Update requires confirmation before download/install
- Play mode (Sequential / Shuffle / Loop One / Stop After Current) and Keep Position persist across restarts

## 0.9.8

- **Secondary waveform**: optional split view (main on top, secondary below); off by default
- Secondary mode switch (spectrogram / waterfall / linear / ?) at bottom-right; fades in on hover
- Shared playhead and selection between main and secondary views
- **File List Copy / Cut**: writes Windows CF_HDROP so Explorer Paste works

## 0.9.7

- **Drag-out clips**: selection no longer writes clip files; clips are generated only when you click Drag
- **Preferences / Drag**: Nuendo-style attribute-button naming (chips + preview); default `{name}_{start_ms}-{end_ms}ms`
- **Export / Convert**: same naming UI; same folder + same format without overwrite appends `_export`
- **Export / Convert**: reuses the playing-file decode cache so conversion of the current track matches drag speed
- **Explorer**: installer adds **Open with Sonora** on files and folders (unsupported formats open the app empty)
- **Loop one**: seamless wrap in the audio callback (no seek/resume click at the end)
- **Startup**: no crash when the system has no usable audio output device

## 0.9.6

- **NCM**: read / play NetEase Cloud Music `.ncm` files (in-memory decrypt ? original MP3/FLAC/M4A/? via Symphonia)
- File association + icon for `.ncm`
- List duration uses NCM embedded metadata when present (no full decrypt)

## 0.9.5

- **File List**
  - Subtle column dividers in the header for easier column resize
- **Safety**
  - Delete confirmation warns that files are removed from this PC (Recycle Bin)
  - Rename refuses to overwrite an existing filename (inline + batch + backend)

## 0.9.4

- **Batch Rename**
  - Opens with the current File List selection checked (not the whole folder)
  - Preview: Select All / Select None / Invert
  - Open folder? / Add File List ? rename files outside the current folder
  - Toolbar Batch Rename stays enabled even when the folder is empty
- **Loudness safe range** (File List ? LUFS button)
  - Separate checkboxes for LUFS-I and LRA marking (default: off)
  - In range = green, out of range = red; filename yellow when a checked metric fails
  - Range separator `~`; negative values typeable; LRA minimum clamped to 0
  - Fixed LUFS column accent color covering green/red highlights
- **File List performance**
  - LUFS: Analyze Visible Rows (default) vs Analyze All Files
  - Faster folder open (durations filled in the background)
  - Fix scroll jump while LUFS visible-scan updates cells (scroll anchoring + observer)
- **Updates**
  - Quiet startup check; status-bar Update chip + download progress
  - Update check moved to Rust (`ureq`) to avoid WebView GitHub 403

## 0.9.3

- Taskbar playback indicator (toggle in Preferences ? Interface): title-bar frosted icon; Win11 combined taskbar uses native progress fill on a single button (no overlay badge)
- Colors: green play / amber pause / idle stop / red error / violet analyzing
- Preferences modal scrolls correctly in small windows
- Help/About: Check for Updates (one-click install), Website, GitHub
- Site media: lighter posters first + jsDelivr CDN fallback

## 0.9.2

- Taskbar progress bar: green while playing, red while paused (playback position)
- Help menu reorganized: online manual, Check for Updates, Website, GitHub, About
- In-app update check with one-click download and install from GitHub Releases
- About dialog: Website / Docs / GitHub links and Check for Updates

## 0.9.1

- File list context menu and toolbar hover tooltips localized (zh-Hans / zh-Hant)
- Folder history popup uses i18n for Open Folder / Clear history

## 0.9.0

- Right-click Refresh Loudness re-runs whole-file analysis for the selection
- Videos included in loudness analysis; clips longer than 10 minutes are skipped
- Version line moves to 0.9.x

## 0.8.10

- Fix Keep Position When Switching (per-call flag + keepPosRef)

## 0.8.9

- Compact inline volume % while dragging the slider

## 0.8.8

- Volume slider percentage bubble while dragging

## 0.8.7

- Context submenu flips left near the right edge of the window

## 0.8.6

- Volume slider right-click presets (Mute, 10??00%, Save / Restore)
- Default volume 100%

## 0.8.5

- Cursor readouts avoid colliding with the format badge
- Waterfall peak callout readability improvements

## 0.8.4

- Waveform cursor time / pause readouts
- Analyzer frequency readouts on spectrogram / waterfall / bars / freq views
- Interface option: Show cursor readouts
- NSIS installer branding artwork
