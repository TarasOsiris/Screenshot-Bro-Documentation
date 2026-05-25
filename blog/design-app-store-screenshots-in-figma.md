# How to Design App Store Screenshots in Figma

Figma is a solid way to design App Store screenshots if you want total layout control. You can build your own device frames, typography, gradients, components, export presets, and localized variants. The tradeoff is that Figma does not know much about App Store Connect or Google Play. You have to create the production system yourself.

This tutorial walks through how to design app store screenshots in Figma without letting the file turn into a pile of duplicated frames. It works for iOS, iPadOS, macOS, and Android screenshots, and it is especially useful if you are still deciding whether Figma is enough or whether you need a dedicated [app store screenshot tool](best-app-store-screenshot-tools.md).

## 1. Start With the Store Sizes

Do not start with a random presentation frame. Start with the pixel sizes the stores actually accept. Apple and Google can reject screenshots that do not match their dimension, format, or aspect ratio rules.

For current iPhone screenshots, a safe primary App Store size is **1320 x 2868** for 6.9" devices. For iPad, common sizes include **2064 x 2752** for 13" iPad and **1668 x 2388** for 11" iPad. Google Play is more flexible, but screenshots still need to fit Google's side length and aspect ratio rules.

Useful references:

- [Screenshot sizes for App Store and Google Play](screenshot-sizes-app-store-google-play.md)
- [Apple screenshot specifications](https://developer.apple.com/help/app-store-connect/reference/screenshot-specifications)
- [Google Play preview asset requirements](https://support.google.com/googleplay/android-developer/answer/9866151)

## 2. Create One Figma Page per Store Surface

Keep the file boring and predictable. A clean structure is more valuable than a clever one:

- **01 - iPhone 6.9** for primary App Store phone screenshots.
- **02 - iPad 13** for iPad screenshots.
- **03 - Google Play Phone** for Android phone screenshots.
- **04 - Components** for shared styles, device frames, badges, and copy blocks.
- **05 - Archive** for old screenshots you may need later.

Inside each page, create one frame for each screenshot slot: `01 - Main Benefit`, `02 - Feature Detail`, `03 - Social Proof`, and so on. Keep the number prefix in the frame name because Figma exports will be easier to sort.

## 3. Build a Reusable Screenshot Component

The biggest mistake is designing every screenshot as a unique artboard. That works for one release, then breaks as soon as you update the app, add a language, or change the background style.

Instead, build a reusable screenshot structure:

- A background layer with your color, gradient, or image treatment.
- A headline text block with shared typography styles.
- An optional subheadline block.
- A device frame component.
- A masked screenshot layer inside the device frame.
- Optional badges, arrows, callouts, or feature labels.

Use Figma components for repeated parts such as device frames, badges, and callout labels. Use Auto Layout where the content should reflow, especially for text groups that may change length during localization.

## 4. Design the First Screenshot Like a Store Visitor

Your first screenshot should answer one question: why should a person care about this app? Avoid using the first slot for a settings screen, onboarding screen, or vague feature list.

A practical formula:

- **Headline:** 4 to 8 words that state the outcome.
- **Device screenshot:** show the app doing the thing, not an empty state.
- **Support visual:** one badge, chart, or callout if it clarifies the value.
- **Background:** branded, simple, and consistent across the set.

Keep text large enough to read in App Store search results and product-page previews. If the text only works when the Figma canvas is zoomed in, it is probably too small.

## 5. Turn One Layout Into a Set

Once the first screenshot works, duplicate the frame for each store slot and change one message at a time. A common seven-screenshot sequence:

1. Main outcome.
2. Core workflow.
3. Feature that differentiates the app.
4. Personalization or settings.
5. Trust, privacy, sync, or integrations.
6. Secondary use case.
7. Final reason to download.

Keep device position, headline position, and background treatment consistent unless you have a deliberate reason to change them. The set should feel like one product story, not seven separate ads.

## 6. Prepare for Localization Before You Translate

Localization is where many Figma screenshot files become painful. If you duplicate every artboard for every language, a small design change turns into dozens of manual edits.

To make localization less fragile:

- Keep text in predictable layers: `Headline`, `Subheadline`, `Badge`.
- Use Auto Layout for text containers that need to grow.
- Avoid hard line breaks unless the phrase is final.
- Leave extra width for German, French, Spanish, and other longer translations.
- Create a separate page or section per locale only after the base English layout is stable.

If you support many languages, this is the point where Figma often becomes a production bottleneck. You can still do it, but you need discipline: naming, components, export presets, and a checklist for every release.

## 7. Set Up Export Presets

Figma lets you export selected layers and frames in common image formats. For store screenshots, export the final top-level frames, not nested groups.

Recommended export setup:

- Set each screenshot frame to the exact store pixel dimensions.
- Add a PNG export setting for each final frame.
- Name frames with a zero-padded prefix: `01`, `02`, `03`.
- Export one device family at a time so files do not get mixed.
- Review the exported PNGs in Finder before uploading.

## 8. Upload Carefully

Before uploading, check four things:

- The file dimensions match the target store slot.
- The files are ordered correctly.
- The screenshots are in the correct locale.
- No transparent backgrounds or accidental crop issues slipped in.

For App Store Connect specifically, every locale has its own screenshot set. If you export English, German, Spanish, and French from Figma, keep those folders separate and upload them one locale at a time. For a deeper walkthrough, read [How to Upload Screenshots to App Store Connect](upload-screenshots-to-app-store-connect.md).

## Where Figma Works Well

Figma is a good choice when you need custom layouts, a designer is already involved, and your screenshot set is not changing every week. It gives you full control over composition, typography, brand systems, and visual polish.

## Where Figma Gets Painful

Figma becomes harder when screenshots are part of your regular release process. Common pain points:

- Duplicated frames for every device size.
- Duplicated frames for every locale.
- Manual replacement of simulator screenshots.
- Manual export and folder organization.
- Easy-to-break layouts when translated text gets longer.
- No native App Store Connect upload workflow.

None of these make Figma a bad tool. They just mean Figma is a general design tool, not a purpose-built App Store screenshot workflow.

## A Faster Alternative for Indie Developers

If you only make screenshots once or twice a year, Figma may be enough. If you ship often, support multiple languages, or maintain App Store and Google Play screenshots together, try [Screenshot Bro](https://screenshotbro.app).

Screenshot Bro is a native macOS app for designing, localizing, exporting, and uploading store screenshots. It gives you device rows, reusable templates, built-in localization, batch export, and [App Store Connect upload](upload-screenshots-to-app-store-connect.md) without rebuilding a production system in Figma.
