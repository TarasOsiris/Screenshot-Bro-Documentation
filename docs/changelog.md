# Changelog

New features, improvements, and bug fixes shipped with each release.

---

## v2.6 — May 13, 2026

**Shape Lock, Inline Locale Bar, and Export Filename Suffixes**

### New

- Freeform-style shape Lock with pixel-perfect selection handles
- "Match Size to Selected Devices" right-click action to resize a row to its device shapes
- "Translate Selected to All Languages" action, with unified language terminology across the app
- Optional locale code and custom suffix appended to exported screenshot filenames
- SF Symbol icons in canvas and row context menus

### Improved

- Locale switcher moved out of the toolbar into an inline flag-chip bar
- Shape properties bar split into per-control sections, with a rotation reset
- Font picker renders each entry in its own typeface
- Locked shapes are skipped during bulk operations and multi-select drags
- Screenshot drops honor the row's Android default
- RevenueCat user ID shown in the Purchase settings tab

### Fixed

- Shapes that span into neighboring templates are preserved when a template is deleted

---

## v2.5 — May 9, 2026

**Onboarding Coach Marks, Help Window, and 3D Device Polish**

### New

- Interactive onboarding coach marks after the welcome modal
- Comprehensive Help window covering core editor features
- iPhone 17 Pro Max 3D device frame
- Editable text field for shape rotation degrees
- Post-purchase celebration sheet
- Subscription tier details on the paywall, with a consolidated App Store Connect API Key UI
- Periodic App Store review prompt after sustained use

### Improved

- 3D device controls unified into a single popover; size is preserved across orientation flips
- 3D device pitch and yaw range widened to ±90°
- Generic Android device frames flex to match the dropped screenshot's aspect
- Refactored oversized views and centralized logging for better stability

---

## v2.4 — May 1, 2026

**Spanish UI, Showcase Export, and Pro Tier**

### New

- Spanish in-app localization with a language picker in Settings
- 38 additional language presets in the locale catalog, with unified flags and presets
- Showcase export sheet with configurable aspect presets and per-row previews
- Settings backup action — one-click zip of all app data
- Pro tier with an Upgrade to Pro entry point in the toolbar for free users
- iOS Simulator capture from the device shape context menu
- Remove Background action on the image shape context menu
- Show in Finder option in the project menu
- Weight and italic controls for imported custom fonts, with auto-import of family siblings
- Notifications when exports and App Store Connect uploads complete
- App Store Connect demo mode for App Review

### Improved

- Showcase export sheet UX overhaul
- App Store Connect setup flow with Clear Credentials moved into the API Key section
- Refreshed Indigo and Amethyst templates
- Native NSAlert for project rename and duplicate, prefilled with the current name
- SVG insertion scales up to a minimum dimension; Restore Aspect Ratio is available for every SVG
- Device shape image actions grouped under a single Image submenu
- Translation table scrolling polish

### Fixed

- Screenshot now bleeds past the frame aperture to eliminate the halo seam
- Template artifacts no longer carry over when instantiating from a template
- Project rename text field caret no longer jumps
- Main window reopens reliably when creating a project; app quits on last window close
- Hardened App Store Connect upload by safely decoding legacy fields

---

## v2.3 — April 17, 2026

**Upload to App Store Connect**

### New

- One-click upload of rendered screenshots straight to App Store Connect via the official API
- Auto-detected display types from row size, with override support for iPhone, iPad, and Mac
- Locale matching between project locales and App Store version localizations
- Preflight validation — flags oversized images, missing locales, version lock states, and platform conflicts before anything is uploaded
- Metadata editing step in the upload wizard — adjust App Store copy before pushing
- Per-project memory of the selected App Store Connect app and version
- API key management: Issuer ID, Key ID, and .p8 stored securely in the macOS Keychain

---

## v2.2 — April 15, 2026

**Smarter Batch Import & Template Cleanup**

### New

- Reset All Images action — clear every device screenshot in a project in one go

### Improved

- Batch import now fits images to the closest matching row and skips duplicates
- Saved templates only include the fonts they actually use; preview assets now render at 1x for smaller project files

### Fixed

- Refreshed Sports template rendering and background fidelity

---

## v2.1 — March 31, 2026

**Rich Text, Per-Locale Export, New Templates**

### New

- Rich text editing for text shapes with per-character styling (weight, color, size)
- Per-locale export menu — export any single locale without running the full batch
- "Duplicate to All Screenshots" context action; renamed "Clip to Screenshot" to "Clip to Frame"
- Directional duplicate (⌥-drag with arrow direction) and flag emojis in locale labels
- Apple Watch Ultra 3 device frames and Abstract Pixel 9 template
- Colorful, Forest, and Sports project templates with release metadata

### Improved

- Rendered template previews in the New Project window for faster picking

### Fixed

- Arrow keys no longer hijack cursor navigation inside text fields
- New shapes no longer land off-screen after deleting templates

---

## v2.0 — March 10, 2026

**Initial Release — Screenshot Bro on the Mac App Store**

### New

- Multi-template editing — change a shape once, see it across every screenshot in the row
- Device frames for iPhone 17 series, iPad Pro 11" & 13", MacBook, iMac, and Android phones & tablets
- Background editor with solid colors, linear/radial/angular gradients, and image fill/fit/stretch/tile modes — backgrounds can span across templates
- Shape tools: rectangles, circles, stars, text, images, SVGs, and device frames with full transform controls
- Smart alignment with snap guides, Shift-nudge for 10px jumps, Option-drag to clone
- 30 language localization with per-shape text, position, and image overrides; auto-translate for missing copy
- Batch screenshot import — drag multiple shots in and they auto-wrap in device frames
- Custom font import (.ttf, .otf, .ttc) with per-project registration
- Opt-in iCloud Drive sync with tombstone-aware conflict resolution across Macs
- PNG & JPEG export at 1x, 2x, and 3x into folders organized by locale and row
- Keyboard shortcuts for nudge, duplicate, cut/copy/paste, z-order, zoom, and locale cycling
