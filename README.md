# Screenshot Bro

**Design and upload App Store and Google Play screenshots in minutes.**

Screenshot Bro is a native macOS app for creating polished, store-ready marketing screenshots. Drop in a screenshot, pick a device frame, add a headline, and export at exactly the resolution the App Store and Google Play expect.

[Get on the Mac App Store](https://apps.apple.com/us/app/screenshot-bro/id6760177675?ref=github-docs)

[Website](https://screenshotbro.app?ref=github-docs)

## Features

- **Multi-Template Editing** — Edit once, update every variant. Change a shape or text and it flows across all your screenshots simultaneously.
- **Device Frames** — iPhone 17 series, iPad Pro 11" & 13", MacBook, iMac, and Android phone & tablet frames with accurate bezels and configurable body colors.
- **Backgrounds & Spanning** — Solid colors, linear/radial/angular gradients with multi-stop editor, or images with fill, fit, stretch, and tile modes. Backgrounds can span across multiple templates.
- **Shape Tools + SVG** — Rectangles, circles, stars, text, images, SVGs, and device frames with inline text editing, outlines, fill modes, and full transform controls.
- **Smart Alignment** — Snap guides appear as you drag. Nudge with arrow keys, Option-drag to duplicate, Shift-drag to lock aspect ratio.
- **Localization Built In** — 30 language presets. Auto-translate missing copy on-device via Apple's Translation framework. Per-locale text overrides.
- **Localized Export** — Export PNG or JPEG at 1x–3x for multiple locales. Auto-organized folders by locale and row.
- **Upload to App Store Connect** — One-click upload with auto-detected display types, locale matching, and preflight validation.
- **iCloud Sync** — Opt-in iCloud Drive sync across all your Macs with last-writer-wins merge and tombstone-aware conflict resolution.
- **Custom Fonts** — Import your own .ttf, .otf, or .ttc font files.
- **Project Templates** — Start from built-in templates with pre-configured layouts.
- **Keyboard Shortcuts** — Nudge, duplicate, cut/copy/paste, z-order, zoom, locale cycling, and select.
- **Privacy-First** — Auto-translate runs on-device. No API keys, no servers, no analytics.
- **Batch Image Import** — Drop a folder of screenshots and let device size auto-detect.
- **Free Forever Tier** — 1 project, 3 rows, 5 templates per row with every device frame, all 30 locales, and every export format. No watermark, no trial expiry, no signup.

## Requirements

- macOS 15.0 (Sequoia) or later
- Apple Silicon or Intel

## Workflow

1. **Set Up Rows** — Pick your device sizes (iPhone 17, iPad Pro 11" or 13", MacBook, iMac). Add as many rows as you need.
2. **Design & Localize** — Drop in device frames, add text and shapes, choose a background. Add locales, auto-translate, and tune per-shape text overrides.
3. **Export All** — Hit export. Get organized folders by locale and row with every screenshot at your chosen scale.
4. **Upload to App Store Connect** — Connect your API key once, then push screenshots straight to the right app, version, display type, and locale.

## FAQ

**Is Screenshot Bro free?**
Yes. The free tier is unlimited in time and lets you keep 1 project with up to 3 rows and 5 templates per row — full access to every device frame, shape, and locale, watermark-free exports included. Pro lifts those limits and unlocks App Store Connect upload and iCloud sync.

**What do I need to run it?**
macOS 15 (Sequoia) or later, on Apple Silicon or Intel. No companion iPhone, no account, no internet connection required for everyday editing.

**Does my data leave my Mac?**
By default, no. Projects, screenshots, and fonts stay on disk. Auto-translation runs through Apple's on-device Translation framework — no API keys, no third-party servers, no analytics. Optional iCloud Drive sync uses your personal iCloud account; we don't operate any intermediate servers.

**How does localization work?**
Pick from 30 language presets, or define your own code. Auto-translate fills in missing copy on-device. Translations save as per-locale text overrides, so layout, color, and images stay shared across every locale. Exports are organized into locale folders App Store Connect can pick up directly.

**Can I make Google Play screenshots too?**
Yes. Android phone and tablet rows render alongside iPhone, iPad, and Mac in the same project. Each device category comes pre-set to the pixel dimensions the relevant store accepts.

**Can I upload to App Store Connect from inside the app?**
Yes. Configure your App Store Connect API key once (Issuer ID, Key ID, and .p8). Screenshot Bro auto-detects the right display type for each row, matches your project locales to App Store Connect localizations, and replaces existing screenshots in a single pass.

**Does it sync between Macs?**
Yes — opt-in iCloud Drive sync keeps projects, screenshots, and fonts available on every Mac signed into your Apple ID. Conflicts merge field-by-field with last-writer-wins, so editing the same project on two Macs converges cleanly.

## Documentation

- [Help & Documentation](docs/help.md) — Complete guide to projects, rows, templates, shapes, devices, backgrounds, locales, exporting, and keyboard shortcuts.
- [Project File Schema](docs/project-schema.md) — JSON Schema for generating, validating, or transforming project files with AI and scripts.
- [Changelog](docs/changelog.md) — Release notes for every version.

## Comparisons

- [Fastlane snapshot vs Screenshot Bro](comparisons/vs-fastlane-snapshot.md)

## Links

- [Website](https://screenshotbro.app?ref=github-docs)
- [Mac App Store](https://apps.apple.com/us/app/screenshot-bro/id6760177675?ref=github-docs)
- [Reddit Community](https://www.reddit.com/r/ScreenshotBro/)
- [X / Twitter](https://x.com/soycastic)
- [Threads](https://www.threads.com/@soycastic)

## Contact

**Nineva Studios**
[tleskiv@ninevastudios.com](mailto:tleskiv@ninevastudios.com)
