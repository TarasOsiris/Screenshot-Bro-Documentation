# Fastlane: Make and Ship App Store Screenshots (2026 Guide)

[fastlane](https://fastlane.tools) is a Ruby toolchain for automating the boring parts of shipping iOS and Android apps. Its three screenshot-related actions -- `snapshot`, `frameit`, and `deliver` -- cover the full pipeline: drive your app inside an XCUITest to capture raw images, wrap them in device frames with marketing copy, and push the result to App Store Connect. This guide walks through the pipeline end-to-end with the configuration files, lane definitions, and CI workflow you need to run it.

By the end you will have: a Ruby Bundler-pinned setup, an App Store Connect API key, a working `Snapfile` + `SnapshotHelper.swift`, a `Framefile.json` with per-screenshot keywords and titles, a `Deliverfile` tuned for screenshot-only uploads, a four-lane `Fastfile`, and a GitHub Actions workflow that runs the whole thing on a `macos-26` runner.

## 1. Prerequisites and Mental Model

Before any Ruby gets installed, get the conceptual picture clear. The screenshot pipeline has three independent stages, each owned by a separate fastlane action:

1. **Capture** -- an XCUITest runs against a simulator and calls `snapshot("01-Home")` at the moments you want recorded. fastlane's `snapshot` action launches the right simulators, switches each one to the right locale, runs the test, and pulls the resulting PNGs into `fastlane/screenshots/<locale>/`.
2. **Frame** -- `frameit` reads each PNG, picks a device frame based on the image resolution, optionally composites a background and marketing title, and writes a `_framed.png` alongside the original.
3. **Upload** -- `deliver` walks the screenshots folder, matches each image to an App Store Connect *display family* (e.g. [iPhone 6.9", iPad 13"](app-store-screenshot-sizes.md)), and replaces the screenshot set on the editable App Store version via the App Store Connect API.

Each stage is independently runnable. You can take screenshots without uploading them, frame screenshots produced by another tool, or upload pre-built screenshots without ever invoking the simulator.

## 2. Install fastlane the Sane Way: Bundler

Don't `brew install fastlane`. Pin fastlane per-project with [Bundler](https://bundler.io) so every machine that builds your project -- your laptop, a teammate's laptop, CI -- runs the same version. From the project root:

```bash
# system Ruby on macOS 14+ is fine, but rbenv/asdf is cleaner
gem install bundler
bundle init
echo 'gem "fastlane"' >> Gemfile
bundle install --path vendor/bundle

# from now on, run fastlane via:
bundle exec fastlane <lane>
```

Commit `Gemfile` and `Gemfile.lock`. Add `vendor/bundle` to your `.gitignore`. Now initialize fastlane in the project:

```bash
bundle exec fastlane init
# choose option 4: "Manual setup"
```

This creates a `fastlane/` directory with `Fastfile` and `Appfile`. Fill in your bundle identifier and team ID in `Appfile`:

```ruby
# fastlane/Appfile
app_identifier("com.example.myapp")
apple_id("you@example.com")     # only needed if you fall back to legacy auth
team_id("ABCDE12345")           # Developer Portal team ID
```

## 3. The App Store Connect API Key

Username/password auth is deprecated for new accounts and two-factor-protected for everyone else, which makes it useless on CI. Use an [App Store Connect API key](https://appstoreconnect.apple.com/access/integrations/api) instead. In App Store Connect -> **Users and Access** -> **Integrations** -> **App Store Connect API**:

1. **Generate API Key** (you only do this once per team; lost `.p8` files cannot be re-downloaded).
2. Give it the **App Manager** role. **Developer** is not enough to upload screenshots; **Admin** is more than you need.
3. Note the **Key ID** (10 chars, e.g. `ABCD1234EF`) and the team's **Issuer ID** (UUID at the top of the same page).
4. Download the `AuthKey_ABCD1234EF.p8` file.

fastlane reads the key from a JSON file. Save it as `fastlane/asc_api_key.json` (and put that path in `.gitignore`):

```json
{
  "key_id": "ABCD1234EF",
  "issuer_id": "57246542-96fe-1a63-e053-0824d011072a",
  "key": "-----BEGIN PRIVATE KEY-----\nMIGTAg...truncated...A==\n-----END PRIVATE KEY-----",
  "duration": 1200,
  "in_house": false
}
```

`duration` is the lifetime of each generated JWT in seconds; the maximum Apple accepts is `1200` (20 minutes). The `key` field is the full contents of the `.p8` file, including the `BEGIN/END PRIVATE KEY` lines, with literal `\n` for line breaks.

On CI, never check the JSON in. Store the JSON contents as a single secret (e.g. `ASC_API_KEY_JSON`) and write the file at run time -- there is an example in the GitHub Actions section below.

## 4. snapshot -- Capturing Screenshots in XCUITest

`snapshot` works by injecting a small Swift helper into your UI test target. The helper hooks into the test runtime so that every call to `snapshot("name")` from your test takes a screenshot of the simulator screen, names it, and writes it to a disk location fastlane already knows how to find.

### Generate the helper

```bash
bundle exec fastlane snapshot init
```

This creates `fastlane/Snapfile` and `fastlane/SnapshotHelper.swift`. Add `SnapshotHelper.swift` to your **UI testing target** in Xcode (drag it in, make sure "MyAppUITests" is the only checked target -- never include it in the app binary).

### Snapfile

`Snapfile` tells `snapshot` which devices to spin up, which locales to run each test in, and which UI test scheme to invoke:

```ruby
# fastlane/Snapfile

devices([
  "iPhone 17 Pro Max",     # 6.9"  -> 1320 x 2868 (required slot in 2026)
  "iPhone 17 Pro",         # 6.3"  -> 1206 x 2622 (optional, nicer in store listings)
  "iPad Pro 13-inch (M4)", # 13"   -> 2064 x 2752 (required if you ship an iPad build)
])

languages([
  "en-US",
  "de-DE",
  "fr-FR",
  "es-ES",
  "ja",
])

scheme("MyAppUITests")           # the UI-testing scheme that runs Snapshot tests
output_directory("./fastlane/screenshots")
clear_previous_screenshots(true)
override_status_bar(true)        # 9:41, full battery, full signal
concurrent_simulators(true)
stop_after_first_error(true)
number_of_retries(1)
```

Highlights:

- **`devices`** -- names must match exactly what `xcrun simctl list devices` prints. Apple changes names every year.
- **`languages`** -- pass either a region code like `"en-US"` or a bare language code like `"ja"`. Each entry generates a folder under `output_directory`.
- **`override_status_bar(true)`** -- uses `simctl status_bar override` to show 9:41, full battery, and full Wi-Fi/cell signal in every screenshot.
- **`concurrent_simulators`** -- runs multiple simulators in parallel. Cuts wall time roughly by the number of devices, but each simulator costs ~3 GB of RAM.
- **`clear_previous_screenshots`** -- deletes `fastlane/screenshots/<locale>/` at the start of each run.

### Wire snapshot into your XCUITest

`SnapshotHelper` exposes two free functions: `setupSnapshot(_:)` (call once per test) and `snapshot(_:)` (call wherever you want to record a frame).

```swift
// MyAppUITests/MyAppUITests.swift

import XCTest

final class MyAppUITests: XCTestCase {
    override func setUpWithError() throws {
        continueAfterFailure = false
        let app = XCUIApplication()
        setupSnapshot(app)
        app.launchArguments += [
            "-UITests",
            "-AppleLanguages", "(\(Snapshot.deviceLanguage))",
            "-AppleLocale", Snapshot.currentLocale,
        ]
        app.launch()
    }

    func testScreenshots() {
        let app = XCUIApplication()

        snapshot("01-Home")

        app.tabBars.buttons["Library"].tap()
        snapshot("02-Library")

        app.cells.element(boundBy: 0).tap()
        snapshot("03-Detail")

        app.navigationBars.buttons.element(boundBy: 0).tap()
        app.tabBars.buttons["Settings"].tap()
        snapshot("04-Settings")
    }
}
```

### Run snapshot

```bash
bundle exec fastlane snapshot
```

Output lands in `fastlane/screenshots/<locale>/iPhone 17 Pro Max-01-Home.png` and friends. Snapshot also generates `screenshots.html` -- open it to flip through all locales and devices in a single page.

## 5. frameit -- Adding Device Frames and Titles

Raw screenshots are bare device viewports. `frameit` wraps them in physical device frames, optionally adds a background, a marketing title, and a smaller "keyword" line above it.

```bash
# Download device frames once (cached under ~/.frameit/)
bundle exec fastlane frameit download_frames

# Frame everything in fastlane/screenshots, including subfolders
bundle exec fastlane frameit --use_platform IOS
```

### Framefile.json

Default frames look like a dev test -- black frame, no background, no text. Configure them with `fastlane/screenshots/Framefile.json`:

```json
{
  "device_frame_version": "latest",
  "default": {
    "keyword": {
      "font": "./fonts/Inter-SemiBold.ttf",
      "color": "#FFFFFF",
      "padding": 50
    },
    "title": {
      "font": "./fonts/Inter-Bold.ttf",
      "color": "#FFFFFF"
    },
    "background": "./background.png",
    "padding": 80,
    "show_complete_frame": false,
    "title_below_image": false,
    "stack_title": true
  },
  "data": [
    {
      "filter": "Home",
      "keyword": { "color": "#9F7AEA" },
      "frame": "BLACK"
    },
    {
      "filter": "Settings",
      "keyword": { "color": "#3B82F6" },
      "frame": "WHITE"
    }
  ]
}
```

### Per-locale title.strings and keyword.strings

Marketing copy comes from two parallel per-locale strings files:

```
/* fastlane/screenshots/en-US/title.strings */

"01-Home"     = "Track every match.\nIn one tap.";
"02-Library"  = "Your full history,\nalways with you.";
"03-Detail"   = "Drill into any session.";
"04-Settings" = "Sync across all devices.";
```

```
/* fastlane/screenshots/en-US/keyword.strings */

"01-Home"     = "FAST";
"02-Library"  = "ORGANIZED";
"03-Detail"   = "DEEP";
"04-Settings" = "EVERYWHERE";
```

Translations go in `fastlane/screenshots/de-DE/title.strings` and so on. Use `\n` for line breaks. Missing locales fall back to the untranslated screenshot (no title rendered) -- there is no automatic English fallback.

## 6. deliver -- Uploading to App Store Connect

`deliver` walks the screenshots folder, matches each PNG to an App Store Connect display family, then talks to the App Store Connect API to replace the screenshot set on the currently editable version.

### Deliverfile

```ruby
# fastlane/Deliverfile

app_identifier("com.example.myapp")
team_id("ABCDE12345")

api_key_path("./fastlane/asc_api_key.json")
screenshots_path("./fastlane/screenshots")
metadata_path("./fastlane/metadata")

skip_binary_upload(true)
skip_metadata(false)
skip_screenshots(false)
skip_app_version_update(true)

force(true)
overwrite_screenshots(true)
run_precheck_before_submit(false)
submit_for_review(false)

ignore_language_directory_validation(false)
```

### Dry-run, then ship

```bash
# verify which display families and locales will be touched, no upload
bundle exec fastlane deliver --verify_only

# real upload
bundle exec fastlane deliver
```

## 7. The Full Fastfile

```ruby
# fastlane/Fastfile

default_platform(:ios)

platform :ios do
  desc "Capture screenshots with snapshot"
  lane :screenshots do
    capture_ios_screenshots
  end

  desc "Frame screenshots with frameit"
  lane :frame do
    frame_screenshots(
      path: "./fastlane/screenshots",
      use_platform: "IOS"
    )
  end

  desc "Build framed screenshots end-to-end"
  lane :build_marketing do
    capture_ios_screenshots
    frame_screenshots(path: "./fastlane/screenshots", use_platform: "IOS")
  end

  desc "Upload screenshots to App Store Connect"
  lane :upload_screenshots do
    upload_to_app_store(
      skip_binary_upload: true,
      skip_metadata: true,
      skip_app_version_update: true,
      force: true,
      overwrite_screenshots: true,
      run_precheck_before_submit: false
    )
  end

  desc "Full release pipeline: capture, frame, upload"
  lane :ship_screenshots do
    capture_ios_screenshots
    frame_screenshots(path: "./fastlane/screenshots", use_platform: "IOS")
    upload_to_app_store(
      skip_binary_upload: true,
      skip_metadata: true,
      skip_app_version_update: true,
      force: true,
      overwrite_screenshots: true
    )
  end
end
```

## 8. CI on GitHub Actions

```yaml
# .github/workflows/screenshots.yml

name: Screenshots

on:
  workflow_dispatch:
  push:
    paths:
      - "fastlane/**"
      - "MyAppUITests/**"

jobs:
  screenshots:
    runs-on: macos-26
    timeout-minutes: 90
    env:
      LC_ALL: en_US.UTF-8
      LANG: en_US.UTF-8
      FASTLANE_SKIP_UPDATE_CHECK: "1"
      FASTLANE_HIDE_CHANGELOG: "1"

    steps:
      - uses: actions/checkout@v4

      - name: Select Xcode
        run: sudo xcode-select -s /Applications/Xcode_26.3.app

      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.3"
          bundler-cache: true

      - name: Write App Store Connect API key
        run: |
          mkdir -p fastlane
          echo "$ASC_API_KEY_JSON" > fastlane/asc_api_key.json
        env:
          ASC_API_KEY_JSON: ${{ secrets.ASC_API_KEY_JSON }}

      - name: Capture, frame, upload
        run: bundle exec fastlane ios ship_screenshots

      - name: Archive framed screenshots
        if: always()
        uses: actions/upload-artifact@v4
        with:
          name: screenshots
          path: fastlane/screenshots
```

## 9. Common Errors and Fixes

### "Could not find a device matching..."

Snapshot's device names must match `xcrun simctl list devicetypes` exactly. After every Xcode upgrade, re-run `xcrun simctl list devicetypes | grep iPhone` and update `Snapfile`.

### "Unable to verify upload" / 401 Unauthorized

Your JWT expired mid-upload, the API key has been revoked, or its role was downgraded. Check that `duration` in `asc_api_key.json` is at most `1200`, then re-issue the key.

### "App Store Connect is locked"

The version you are uploading to is in *In Review* or *Pending Developer Release*. Screenshots are read-only in those states. Create a new version and re-run.

### Screenshots end up in the wrong display family

`deliver` matches by image resolution. If you exported a 6.9" iPhone screenshot at 1320 x 2868 it will land in the 6.9" slot; if you scaled it to 1284 x 2778 it lands in the legacy 6.5" slot.

### "Screenshot has alpha channel"

App Store Connect has historically rejected PNGs with transparency. Flatten alpha by re-exporting without an alpha channel. `frameit`'s composited output is always opaque.

## 10. When fastlane Is Overkill

fastlane's screenshot pipeline is great when you already have a UI test target and your team is comfortable maintaining XCUITest fixtures. It is the wrong choice when you want full art direction over each screenshot, or you are a solo dev who would rather design once in a Mac app and click **Upload**.

For the design-first workflow, [Screenshot Bro](https://screenshotbro.app) covers the same ground -- [localized layouts](localize-app-store-screenshots.md), device frames, [one-click App Store Connect upload](upload-screenshots-to-app-store-connect.md) with the same API key -- without the XCUITest plumbing.

## TL;DR Cheat Sheet

```bash
# Setup (once)
bundle init && echo 'gem "fastlane"' >> Gemfile && bundle install
bundle exec fastlane init
bundle exec fastlane snapshot init
bundle exec fastlane frameit download_frames

# Per release
bundle exec fastlane ship_screenshots
```

Wondering whether to set this up at all, or to use a Mac app for the same job? See [Fastlane snapshot vs Screenshot Bro](../comparisons/vs-fastlane-snapshot.md) for the side-by-side and the "use both" workflow.
