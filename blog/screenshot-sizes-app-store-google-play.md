# Screenshot Sizes for App Store and Google Play (2026)

Apple App Store Connect and Google Play Console both reject uploads that do not match their accepted dimensions, aspect ratios, and file formats. The two stores use completely different rules -- Apple pins screenshots to specific pixel sizes per device class, while Google accepts a range of sizes constrained by aspect ratio. This is the full reference for both, current as of April 2026.

## Apple App Store

Apple groups screenshots by **device family** (iPhone, iPad, Mac, Apple Watch, Apple TV, Apple Vision Pro). Each family requires screenshots when your app supports that platform. For iPhone and iPad, App Store Connect can scale higher-resolution screenshots down for older displays when you do not provide custom assets for every size.

Screenshots must be JPEG, JPG, or PNG. You can upload **1 to 10** screenshots per device family per localization.

### iPhone

| Display | Pixels (Portrait) | Devices |
|---|---|---|
| 6.9" *(required)* | 1260 x 2736, 1290 x 2796, or 1320 x 2868 | iPhone Air, 17 Pro Max, 16 Pro Max, 16 Plus, 15 Pro Max, 15 Plus, 14 Pro Max |
| 6.5" | 1284 x 2778 or 1242 x 2688 | iPhone 14 Plus, 13 Pro Max, 12 Pro Max, 11 Pro Max, 11, XS Max, XR |
| 6.3" | 1206 x 2622 or 1179 x 2556 | iPhone 17 Pro, 17, 16 Pro, 16, 15 Pro, 15, 14 Pro |
| 6.1" | 1170 x 2532, 1125 x 2436, or 1080 x 2340 | iPhone 17e, 16e, 14, 13 Pro, 13, 13 mini, 12 Pro, 12, 12 mini, 11 Pro, XS, X |
| 5.5" (legacy) | 1242 x 2208 | iPhone 8 Plus, 7 Plus, 6s Plus, 6 Plus |

For current iPhone apps, the 6.9" set is the primary required set. The 6.5" set is required only if your app runs on iPhone and you do not provide 6.9" screenshots. Landscape variants use the same dimensions rotated 90 degrees.

### iPad

| Display | Pixels (Portrait) | Devices |
|---|---|---|
| 13" *(required)* | 2064 x 2752 or 2048 x 2732 | iPad Pro 13" (M5/M4), iPad Pro 12.9" (3rd-6th gen), iPad Air 13" (M4/M3/M2) |
| 11" | 1488 x 2266, 1668 x 2420, 1668 x 2388, or 1640 x 2360 | iPad Pro 11" (M5/M4 and 1st-4th gen), iPad Air 11" (M4/M3/M2), iPad Air (4th-5th gen), iPad (A16/10th gen), iPad mini (A17 Pro/6th gen) |

### Mac

The Mac App Store accepts four resolutions, each at a 16:10 aspect ratio. Pick one and stay consistent across the whole set.

- **1280 x 800** (minimum)
- **1440 x 900**
- **2560 x 1600**
- **2880 x 1800** (largest accepted size)

### Apple Watch

| Case | Pixels | Devices |
|---|---|---|
| 49mm Ultra 3 | 422 x 514 | Apple Watch Ultra 3 |
| 49mm Ultra | 410 x 502 | Apple Watch Ultra 2, Ultra |
| 46mm | 416 x 496 | Series 11, Series 10 |
| 45mm / 41mm | 396 x 484 | Series 9, 8, 7 |
| 44mm / 40mm | 368 x 448 | Series 6, 5, 4, SE 3, SE |
| 42mm / 38mm | 312 x 390 | Series 3 |

### Apple TV and Vision Pro

- **Apple TV:** 3840 x 2160 (4K) or 1920 x 1080 (HD).
- **Apple Vision Pro:** 3840 x 2160.

### App Previews (video)

Optional, up to 3 per device family, 15-30 seconds, M4V / MP4 / MOV. App previews have their own accepted video resolutions, so do not assume the screenshot pixel dimensions are valid for preview videos.

## Google Play

Google Play takes a different approach: instead of rigid pixel dimensions, it accepts any size that satisfies its **aspect ratio and side-length rules**. Files must be JPEG or 24-bit PNG with no alpha channel.

### Phone screenshots

- Google requires at least **2 screenshots overall** to publish a store listing, and you can add up to **8 screenshots per supported device type**.
- Each side must be between **320 px and 3840 px**.
- The longest side cannot be more than **twice the length** of the shortest side (so the aspect ratio sits between 1:2 and 2:1).
- Recommended: **1080 x 1920** (portrait) or **1920 x 1080** (landscape).

### Tablet screenshots

Google has a separate large-screen screenshot section for tablets and Chromebooks. Add at least 4 screenshots for these surfaces, use sides between **1080 px and 7680 px**, and keep them at 16:9 landscape or 9:16 portrait.

- **7-inch tablet:** recommended **1200 x 1920**.
- **10-inch tablet:** recommended **1600 x 2560**.

### Wear OS, Android TV, Chromebook, Auto, XR

- **Wear OS:** 384 x 384 px (square, 1:1).
- **Android TV:** 1920 x 1080 px (16:9 landscape only).
- **Chromebook:** aspect ratio of 16:9 or 9:16, sides between 1080 px and 7680 px.
- **Android Automotive OS:** 800 x 1280 portrait or 1024 x 768 landscape; provide at least 2 of each if you provide Automotive screenshots.
- **Android XR:** 4 to 8 screenshots, 8:5 aspect ratio, recommended 3840 x 2400 and minimum 1920 x 1200.

### Other Google Play assets

- **App icon:** 512 x 512 px, 32-bit PNG with alpha.
- **Feature graphic:** 1024 x 500 px, PNG or JPEG, no alpha. Required.
- **Promo video:** public YouTube URL (no upload).

## Side-by-side cheat sheet

| Surface | App Store | Google Play |
|---|---|---|
| Phone | 1320 x 2868 (6.9") | 1080 x 1920 recommended |
| Tablet (small) | 1668 x 2388 (11" iPad) | 1200 x 1920 (7-inch) |
| Tablet (large) | 2064 x 2752 (13" iPad) | 1600 x 2560 (10-inch) |
| Desktop | 2880 x 1800 (Mac) | 1920 x 1080 or 1080 x 1920 minimum (Chromebook) |
| Wearable | 422 x 514 (Watch Ultra 3) | 384 x 384 (Wear OS) |
| TV | 3840 x 2160 | 1920 x 1080 |
| Min / max count | 1-10 per family | 2 total minimum; up to 8 per device type |
| Format | JPEG, JPG, or PNG | JPEG or 24-bit PNG, no alpha |

## Working tips

- **Design at the largest accepted size.** 1320 x 2868 for iPhone, 2064 x 2752 for iPad, 2880 x 1800 for Mac. Downscaling preserves quality better than upscaling.
- **Pick one Mac resolution and lock it.** Mixing 2880 x 1800 and 1440 x 900 in the same set looks visibly inconsistent in App Store Connect previews.
- **Watch the safe area.** Store surfaces crop and arrange screenshots differently, so keep critical text away from the edges.
- **Strip alpha for Google Play.** Google rejects PNGs with an alpha channel. Export flat 24-bit PNGs.
- **Reuse layouts across stores.** A 9:19.5 iPhone screenshot is close enough to a 9:16 Android phone screenshot that the same composition usually works -- just re-export at the target dimensions.

## Where Screenshot Bro fits

Screenshot Bro keeps every device size in one project. Pick the rows you need (iPhone 6.9", iPad 13", MacBook, Android phone, tablet), design once, localize per locale, and batch export. The export folder is grouped by locale and row, so the App Store Connect upload and the Google Play Console upload pull from the same source of truth.
