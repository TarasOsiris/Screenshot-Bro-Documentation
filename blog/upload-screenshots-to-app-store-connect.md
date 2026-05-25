# How to Upload Screenshots to App Store Connect (2026 Guide)

Uploading App Store screenshots is still one of the clunkiest parts of shipping an iOS, iPadOS, or macOS app. Apple supports [multiple display types per platform](screenshot-sizes-app-store-google-play.md) and every locale has its own slot, so even a small app can end up with 80-200 files to push per release. This guide covers four practical ways to get screenshots into App Store Connect in 2026, when to pick each, and the gotchas that waste an afternoon if you do not know about them in advance.

## Option 1: The App Store Connect Web Uploader

The default path. Open your app in [App Store Connect](https://appstoreconnect.apple.com), pick the version you want to edit, scroll to **Screenshots**, and drag your PNG or JPEG files into the correct display-type bucket (6.9", 6.5", 13" iPad, Mac, etc.). Repeat for every locale.

- **Good for:** a first submission, a small app with one or two locales, or when you need to preview exactly what the reviewer will see.
- **Bad for:** anything multilingual, anything with more than a couple of display types, or any workflow you need to repeat every release. 30 minutes of drag-and-drop per locale adds up fast.

**Common gotchas:**

- Wrong display type. If your exported file is 1320 x 2868 (iPhone 16/17 Pro Max, 6.9") and you drop it into the 6.5" bucket, App Store Connect rejects the upload.
- Locked versions. Once a version is "In Review" or "Pending Developer Release", screenshots are read-only. Create a new version first.
- Partial uploads. If a replacement workflow fails midway, verify the full display-type set before submitting instead of assuming the old set is still intact.

## Option 2: Transporter or Fastlane Deliver

Both tools automate App Store Connect delivery, but with different packaging and setup tradeoffs.

**Transporter** is a free Apple utility for shipping builds and metadata packages. It is scriptable but assumes you already have an iTMSTransporter-compatible folder structure with metadata XML. Great if you already build IPA packages with xcodebuild; clunky if you only want to push screenshots.

**[fastlane deliver](https://docs.fastlane.tools/actions/deliver/)** is the community standard. You keep your screenshots in a folder structure like `fastlane/screenshots/en-US/iPhone 6.9 - 01.png` and run `fastlane deliver`. It uploads screenshots, metadata, and keywords in one pass.

- **Good for:** teams that already have a CI pipeline, want screenshot uploads in git, and do not mind Ruby.
- **Bad for:** designers who do not want to maintain a Ruby toolchain, and anyone who wants a GUI that shows what will be uploaded before it happens.

**Common gotchas:**

- Resolution and naming matter. fastlane can infer display targets from image resolution, and ambiguous iPad families may need Apple's display-family name in the filename to land in the right screenshot slot.
- App Store Connect API keys must be generated once and stored securely (Key ID, Issuer ID, and .p8 file). Losing the .p8 means regenerating.
- Replacement is all-or-nothing. Every existing screenshot in the target display type is deleted before the new ones upload.

## Option 3: The App Store Connect API Directly

If you are building a tool yourself, the [App Store Connect API](https://developer.apple.com/documentation/appstoreconnectapi) exposes screenshot upload via three endpoints:

1. `POST /v1/appScreenshotSets` -- create a screenshot set for a specific display type and localization.
2. `POST /v1/appScreenshots` -- create a screenshot reservation, returning upload operation metadata (chunked PUT URLs).
3. `PATCH /v1/appScreenshots/{id}` with `uploaded: true` -- commit the upload after all chunks are pushed.

You authenticate with a JWT signed by your .p8 key. The auth JWT is short-lived (20 minutes max) and scoped to the App Store Connect API audience. Apple also rate-limits aggressive uploads, so chunked concurrent uploads need a retry/backoff strategy.

**Good for:** building custom automation or tools. You get full control, typed responses, and can build UX around the upload flow.

**Bad for:** anyone who just wants to ship, not maintain. Expect to spend a weekend on auth, chunked uploads, and error handling before it is reliable.

## Option 4: A Design Tool That Uploads For You

This is the workflow built into [Screenshot Bro](https://screenshotbro.app). You design your screenshots, add locales, auto-translate the copy, and then click **Upload to App Store Connect**. The app:

- Auto-detects the right display type from your row size (1320 x 2868 -> iPhone 6.9", 2064 x 2752 -> iPad 13", etc.).
- Matches your project locales against the App Store Connect localizations on the selected version. Mismatches are flagged up front.
- Runs a preflight -- oversized files, missing locales, locked versions, or platform conflicts surface before Apple sees anything.
- Replaces each matching set atomically. No half-replaced state, no rename-the-folder dance.

The API key (Issuer ID, Key ID, .p8) is stored once in the macOS Keychain. After setup, every release is one click.

## Which Option Should You Pick?

A rough decision tree:

- **One locale, one app, rarely update.** The web uploader is fine.
- **CI pipeline, git-tracked screenshots, Ruby team.** fastlane deliver.
- **Custom tool / in-house automation.** The API directly.
- **Indie dev or small team, designing + shipping screenshots yourself.** Use a Mac app that collapses design and upload into one flow.

## Before You Upload: A Checklist

Whichever route you pick, these are the mistakes that cost the most time:

- Your screenshots are exactly the [supported dimensions](app-store-screenshot-sizes.md) -- not one pixel off. Apple can reject mismatches during upload or processing.
- The selected App Store version is editable (not "In Review" or "Pending Developer Release").
- Every locale you plan to upload has a matching App Store Connect localization enabled on that version.
- PNG or JPEG only. No HEIC, no WebP, no progressive JPEGs.
- RGB color, with no alpha channel for screenshots.

## TL;DR

The web uploader is the least fun option for anything repeated. fastlane is the default for teams that ship often. The API is powerful but a weekend of work. If you want one-click upload straight from a design app, that is exactly what Screenshot Bro's **Upload to App Store Connect** feature was built for -- auto-detected display types, locale matching, preflight, and one Keychain-stored API key.
