# Google Play Screenshot Sizes and Requirements

Google Play screenshot requirements are more flexible than the App Store, but they are not optional. Your listing needs screenshots that match Google's format rules, and larger surfaces such as tablets, Chromebooks, Wear OS, TV, Automotive, and Android XR each have their own expectations.

This guide focuses on the practical version: what to export, what Google Play Console accepts, and how to organize screenshots so updating your listing does not become a manual file chase.

## Quick Requirements

| Requirement | Google Play rule |
|---|---|
| Screenshot count | At least 2 screenshots across device types; up to 8 per supported device type |
| Format | JPEG or 24-bit PNG with no alpha channel |
| General dimensions | Minimum 320 px, maximum 3840 px |
| General aspect ratio | The longest side cannot be more than twice the shortest side |
| Large screens | For Chromebook and tablets, add at least 4 screenshots; use 1080-7680 px and 16:9 or 9:16 |

Sources: Google's [preview asset requirements](https://support.google.com/googleplay/android-developer/answer/9866151) and Google Play Console help.

## Phone Screenshots

Phone screenshots are the default Play Store screenshot type for most Android apps. Google accepts flexible dimensions, but a practical export size is **1080 x 1920** for portrait screenshots or **1920 x 1080** for landscape screenshots. Stay within the 320-3840 px bounds and keep the longest side no more than twice the shortest side.

For marketing screenshots, avoid tiny captions. Play surfaces can crop, resize, or show screenshots in different contexts, so the app UI and headline need to survive downscaling.

## Tablet and Chromebook Screenshots

Google treats large screens as their own store surface. For Chromebooks and tablets, Google says you can add a minimum of four screenshots to demonstrate the in-app experience. The recommended large-screen constraints are 1080-7680 px with 16:9 landscape or 9:16 portrait.

If your app supports tablets or ChromeOS, do not just stretch phone screenshots. Show the actual large-screen layout: split views, sidebars, wider charts, keyboard workflows, or whatever makes the larger surface useful.

## Wear OS, TV, Automotive, and XR

These surfaces have stricter content expectations than phone screenshots:

- **Wear OS:** at least one screenshot that accurately depicts the current Wear OS app; screenshots should show only the app interface, use a 1:1 aspect ratio, be at least 384 x 384 px, and avoid device frames, extra text, masks, and transparent backgrounds.
- **Android TV:** if you distribute on Android TV, you need at least one TV screenshot before publishing, and TV screenshots only display on Android TV devices.
- **Android Automotive OS:** requirements vary by app category; when provided, screenshots must accurately show the car app experience.
- **Android XR:** Google lists 4 to 8 screenshots, PNG or JPEG up to 8 MB each, with an 8:5 aspect ratio.

## Do You Need a Feature Graphic?

Yes, for most serious listings you should treat the feature graphic as part of the same screenshot production workflow. Google uses it in multiple places, including as a preview-video cover image when a video is present and in large-format app or game placements.

The feature graphic is not a screenshot, so design it separately. Use it for brand, promise, and visual identity; use screenshots for proof that the app experience matches the promise.

## Recommended Export Workflow

1. Create separate rows or folders for phone, tablet, Chromebook, and any specialty surfaces.
2. Export flat JPEG or 24-bit PNG files with no alpha channel.
3. Keep file names ordered: `01_main.png`, `02_feature.png`, and so on.
4. Review screenshots at small sizes before uploading.
5. Keep localized screenshots in separate locale folders.

## App Store vs Google Play

The App Store is more pixel-specific: Apple lists exact screenshot sizes for each display family. Google Play is more constraint-based: it accepts a range of dimensions and aspect ratios. If you ship on both stores, design from a shared visual system but export separate files for each store's rules.

For the combined reference, see [Screenshot Sizes for App Store and Google Play](screenshot-sizes-app-store-google-play.md).

## How Screenshot Bro Helps

[Screenshot Bro](https://screenshotbro.app) keeps App Store and Google Play rows in one native Mac project. You can design phone, tablet, and Android rows together, localize text, batch export organized folders, and avoid rebuilding screenshot files by hand every release.
