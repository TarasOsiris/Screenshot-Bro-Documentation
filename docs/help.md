# Screenshot Bro Help

Complete guide to designing App Store and Google Play screenshots on macOS.

## Welcome to Screenshot Bro

*Beautiful App Store and Google Play screenshots, made on your Mac.*

Screenshot Bro turns raw device screenshots into polished, store-ready marketing images. Drop in a screenshot, pick a device frame, add a headline, and export at exactly the resolution the App Store and Google Play expect.

### Three things to know first

- **Projects** hold one screenshot set per app — usually one project per app, or one per major release.
- **Rows** inside a project group screenshots by device type (iPhone, iPad, Android phone, etc.). Each device size gets its own row because the App Store requires different resolutions.
- **Templates** are the columns inside a row — the individual screenshots you'll submit. Most apps need 3–10 templates per row.

### A typical workflow

1. Create a new project from a template, or start blank.
2. Drop your raw device screenshots onto the templates — Screenshot Bro detects iPhone vs iPad vs Android from the image dimensions and routes them to the right row.
3. Pick a device frame, add a headline, choose a background, and arrange shapes.
4. Add locales for languages you support — translate text once and let the layout follow.
5. Export. You'll get a folder organized by locale and device, ready to upload.

> **Tip:** If this is your first time, the **Onboarding** sheet will walk you through picking a default screenshot size and device. Pick a new project from a template any time via **File > New Project**.

---

## Projects

*One project per app — or per major release.*

A project is a self-contained collection of rows, templates, shapes, locales, and image resources. Projects are stored on disk under your user Application Support folder and can be optionally synced via iCloud Drive.

### Creating a project

- **File > New Project…** (⌘N) opens the New Project window.
- Choose **Blank** to set up rows and screenshot sizes manually, or **From Template** to start with a pre-designed layout.
- In Blank mode, pick the device categories you want — each one becomes a row with the right default screenshot size for the App Store / Play Store.

### Switching between projects

- Use the project picker in the toolbar to jump between projects.
- Pinned and recent projects appear at the top.
- Project order can be set to **Creation date** or **Manual** in Settings > General.

### Renaming, duplicating, deleting

- Right-click a project in the picker for rename, duplicate, and delete actions.
- Deleted projects are kept as **tombstones** for 30 days so iCloud sync can resolve conflicts cleanly. After 30 days the tombstone (and all images) are purged.

### Where projects live on disk

- `~/Library/Application Support/screenshot/projects.json` — index of all projects.
- `~/Library/Application Support/screenshot/projects/<uuid>/project.json` — project data.
- `~/Library/Application Support/screenshot/projects/<uuid>/resources/` — imported images, screenshots, and SVGs.
- The `project.json` format is fully documented as a JSON Schema — see the [Project File Schema](project-schema.md) reference for generating, validating, or transforming projects with scripts and AI.

> **Tip:** Projects autosave 0.3 seconds after the last change. You don't need to manually save. To make a one-off backup, use **Settings > Export > Back Up Projects**.

---

## Rows

*One row per device type.*

Rows are horizontal groups of screenshots inside a project. Each row has its own screenshot size (in pixels), device category, and a row-level background. The App Store requires separate uploads per device size — that's why rows exist.

### Adding rows

- Click **Add Row** at the bottom of the canvas, or use the inspector when no row is selected.
- Choose a device category: **iPhone**, **iPad Pro 11"**, **iPad Pro 13"**, **MacBook**, **Android Phone**, **Android Tablet**, or **Invisible** (an abstract layout with no visible frame).
- Each category sets the row's default screenshot pixel size to a value the relevant store accepts.

### Row inspector

- Select a row (click empty canvas space inside it) to reveal row-level controls in the inspector.
- **Row label** — names the folder this row exports into.
- **Screenshot size presets** — quickly switch between supported store resolutions.
- **Background editor** — color, gradient, or image. See the **Backgrounds** section.
- **Spanning background** — when on, the background spans the entire row width across all templates. When off, every template paints the same background independently.

### Reordering and deleting

- Drag a row's header to reorder. Use **⌘D** to duplicate a selected row.
- **Delete** removes the row. Settings > General has a confirmation toggle.

> **Tip:** If you only see one row, you may be on the **Free** tier (limit: 3 rows per project). Upgrading to Pro removes this limit. See **Free vs Pro**.

---

## Templates

*The individual screenshots inside a row.*

Each column inside a row is a template. A template is one final exported image — its dimensions match the row's screenshot size. The App Store accepts up to 10 templates per row; Google Play up to 8.

### Adding templates

- Click **Add Template** (the **+** button at the right end of the row).
- New templates inherit the row's background and dimensions.
- Drag templates left/right to reorder. Reordering also reorders the exported file numbering.

### Per-template controls

- The **Template Control Bar** below each template lets you override the row background just for that template.
- Drop a screenshot directly onto a template to attach it as the device screenshot.
- The **⋯ menu** offers per-template actions like duplicate, delete, and export preview.

### How shapes relate to templates

- Shapes (text, images, devices, etc.) live on the **row canvas** — the unified area behind all templates in a row. A shape can be positioned to land entirely inside one template, or to span across templates.
- On export, each template is clipped to its own bounds, so a shape that spans templates will appear on each of them at the right horizontal offset.
- This is what makes layouts like a single headline that flows across two screenshots possible.

> **Tip:** Free tier limit: 5 templates per row. Pro removes this limit. See **Free vs Pro**.

---

## Shapes & Text

*Build the layout with rectangles, circles, stars, text, images, devices, and SVGs.*

### Adding shapes

- Use the **Shapes** dropdown in the inspector to add a Rectangle, Circle, or Star.
- Buttons next to it add Text, Image, Device, or SVG elements.
- New shapes are placed at the center of the active template and immediately selected.

### Text

- Double-click a text shape to edit inline. Press **Esc** or click outside to commit.
- The properties bar shows font, weight, size, color, alignment, line height, and letter spacing.
- Text auto-grows vertically by default. Drag a side handle to fix the width and let it wrap.
- Custom fonts: import via **Settings > General > Custom Fonts**.

### Image

- Click the image well in the properties bar to pick a file, or drag and drop directly onto the shape.
- Fill modes: **Fill** (crop to fit), **Fit** (letterbox), **Stretch** (distort), **Tile** (repeat). Tile mode unlocks spacing, offset, and scale controls.
- Add an outline, corner radius, or rotation from the properties bar.

### Device

- Device shapes render the screenshot inside a real device frame. Pick a category and model in the properties bar.
- **Drop a screenshot onto the device** to attach it. The image is automatically clipped to the screen area.
- Each model has color variants and (where applicable) a landscape variant.
- **Invisible** category shows the screenshot with no bezel — useful for clipped or abstract designs.

### SVG

- Click **SVG** to import a vector file. Or paste raw SVG via the SVG paste dialog.
- SVGs render with a configurable color override and scale crisply at any export resolution.
- During resize, rendering is debounced for performance — release the mouse to see the final crisp output.

### Common properties

- Color, opacity, rotation (in degrees, editable as text), border radius, outline (color + width), and a clip toggle (clips overflow to the shape).
- Z-order: **⌘⇧]** brings forward, **⌘⇧[** sends back.

---

## Devices & Frames

*Real device frames with accurate screen insets.*

Device frames wrap your screenshot in an authentic phone or tablet bezel. Screenshot Bro ships pixel-accurate frames for the latest iPhones, iPads, MacBooks, and a generic Android catalog.

### Categories

- **iPhone** — iPhone 17, Air, Pro, Pro Max with the latest color variants.
- **iPad Pro 11"** and **iPad Pro 13"** — current generation with portrait and landscape.
- **MacBook** — MacBook Air 13", MacBook Pro 14", MacBook Pro 16", iMac 24".
- **Android Phone** and **Android Tablet** — generic frames that flex to match the aspect ratio of any dropped screenshot.
- **Invisible** — no visible bezel, just the screenshot. Useful for clipped layouts or abstract designs.

### Picking a model and color

- With a device shape selected, click the device thumbnail in the properties bar to open the picker.
- Models are grouped by category. Each shows available colors as small swatches.
- Switching color preserves the screenshot and any rotation.

### Landscape mode

- Devices that support landscape (iPad, MacBook) auto-rotate the frame to match the dropped screenshot's aspect ratio.
- Manual rotation via the rotation control on the properties bar rotates the entire shape including frame and screen content.

### Image-based vs programmatic frames

- Most modern devices use **image-based frames** — high-res PNG bezels with precise screen insets defined per model.
- Some abstract categories use **programmatic frames** rendered as SwiftUI shapes. They scale flawlessly to any resolution.
- Both render identically in the editor preview and in the exported PNG.

> **Tip:** If you drop a screenshot onto an empty template (not a device shape), Screenshot Bro creates a device shape automatically using the row's category and the right model based on the screenshot's pixel size.

---

## Backgrounds

*Color, gradient, or image — at row or template level.*

### Three styles

- **Color** — a solid fill picked from the inline color picker.
- **Gradient** — Linear, Radial, or Angular. Edit color stops, angle, and (for Radial / Angular) the center point.
- **Image** — bring in any PNG / JPEG / SVG. Pick a fill mode and tweak opacity.

### Gradients

- **Linear**: choose start/end via the angle wheel. Add as many stops as you want.
- **Radial**: a circular gradient with an editable center point and end radius derived from the canvas size.
- **Angular**: a sweep gradient rotating around the center.
- **Gradient presets**: pick from the preset gallery to apply tested stop combinations.

### Image fill modes

- **Fill** — scales to cover; crops anything that doesn't fit.
- **Fit** — scales so the whole image is visible; leaves transparent letterbox bars.
- **Stretch** — fills exactly, distorting aspect if needed.
- **Tile** — repeats the image with adjustable spacing, offset, and scale per axis.

### Row vs template backgrounds

- By default a row's background applies to every template in the row.
- **Spanning background** (row toggle): when on, gradients and images render once across the entire row, so a single horizon or gradient flows across all templates.
- **Override per template**: from the template control bar, set a unique background that replaces the row's default just for that template.

> **Tip:** Spanning is great for storytelling: a sunset gradient or a single panoramic image can stretch across three templates and tell a continuous visual story in the App Store carousel.

---

## Editing on the Canvas

*Drag, resize, rotate, snap.*

### Selection

- Click a shape to select it. **Shift-click** to add to or remove from the selection.
- **⌘A** selects every shape in the active row.
- **Esc** deselects shapes; press again to deselect the row.
- Click empty canvas inside a row to select the row itself and reveal row-level inspector controls.

### Move, resize, rotate

- Drag the shape body to move. Drag a corner or edge handle to resize.
- Drag the rotation handle (above the shape) to rotate freely. Type a degree value into the rotation field for exact control.
- Hold **⇧** while resizing to lock aspect ratio.
- Hold **⌥** while dragging to duplicate the shape as you move.

### Snapping & alignment guides

- Shapes snap to other shapes' edges and centers, and to template boundaries, within a 4px threshold.
- Blue **alignment guides** appear while dragging to show which edges are aligned.

### Nudge

- Arrow keys nudge the selection by 1px.
- **⇧ + Arrow** nudges by 10px.

### Pan & zoom

- Scroll vertically to navigate rows.
- Hold the **middle mouse button** and drag to pan.
- **⌘+** / **⌘−** zoom in/out, **⌘0** resets to 100%, **F** focuses on the current selection.
- The zoom slider in the toolbar ranges from 50% to 200% in 25% steps.

> **Tip:** If a shape spans across templates and you only see part of it, that's expected — each template clips shapes to its own bounds. Switch to a different template view or use **F** to focus on the whole shape.

---

## Locales & Translations

*Translate text once, lay it out once, ship every language.*

Locales let you generate localized screenshot sets without duplicating your project. Each locale shares the same layout and shapes; only text properties (content, font, size, alignment) are overridden per locale.

### Adding locales

- Open the **Locale** menu in the toolbar, or use **Locale > Manage Locales…** in the menu bar.
- Pick from 30 built-in language presets, or define a custom code.
- The first locale you add is the **base locale** — the one whose text is the source of truth.

### Switching the active locale

- **⌘]** / **⌘[** cycle forward / backward through locales.
- **⌘⌥0** jumps back to the base locale.
- When editing a non-base locale, a banner appears at the top of the canvas reminding you which locale you're in.

### How translations work

- In a non-base locale, edits to text shapes are saved as **per-locale overrides** — they don't change the base.
- Other shape properties (position, size, color, image) are shared across all locales. Edit them once and every locale picks up the change.
- If a locale has no override for a text shape, it falls back to the base locale's text.

### Translation helpers

- **Auto-Translate Missing Text** — fills in text shapes that don't yet have an override for the current locale.
- **Re-Translate All Text…** — replaces every existing override with a fresh translation. Use after editing the base locale's text.
- **Revert to Base Language…** — drops all overrides for the current locale, falling back to base text everywhere.
- **Edit Translations…** — open a side-by-side editor showing every text shape with its base content and locale overrides.

### Exporting with locales

- On export, Screenshot Bro creates one folder per locale, then sub-folders per row. The structure matches what App Store Connect's localized screenshot uploads expect.

---

## Importing

*Drop screenshots, images, fonts, and SVGs.*

### Screenshots

- Drag and drop a PNG / JPEG onto a template to attach it as a device screenshot. A device shape is auto-created if needed.
- **Batch import**: drop multiple screenshots at once. Screenshot Bro inspects each image's pixel dimensions and routes it to the matching device row (iPhone vs iPad vs Android).
- If a screenshot doesn't match any existing row, a new row is offered.

### Background images

- Drop directly into the background image well in the inspector, or pick via the file dialog.
- Both raster and SVG images are supported as backgrounds.

### SVG paste

- Use the **SVG** button in the shape toolbar to open the paste dialog.
- Paste SVG markup directly. Width and height are auto-detected; you can override them.
- SVGs are sanitized — script and event handlers are stripped before rendering.

### Custom fonts

- **Settings > General > Custom Fonts** — pick `.otf` / `.ttf` files to register them with the app.
- Imported fonts are bundled with the project so they survive iCloud sync and project transfer.
- Fonts appear in the text shape font picker once registered.

> **Tip:** To capture screenshots from a connected simulator quickly, use the screenshot capture button in the template control bar — it pulls the most recent simulator screenshot directly into the template.

---

## Exporting

*Produce store-ready PNGs and JPEGs.*

### Quick export

- Click **Export** in the toolbar to render the current project to PNG.
- By default, Screenshot Bro exports every locale, every row, and every template at 1× scale.
- File names are zero-padded (`01_…`, `02_…`) so they sort correctly when uploaded.

### Format and scale

- **Settings > Export > Format**: PNG or JPEG. PNG is recommended for marketing screenshots.
- **Scale**: 1×, 2×, or 3×. The App Store and Google Play require exact pixel dimensions, so keep this at 1× unless you specifically need oversized assets.

### Folder structure

- With one locale and one row: a flat folder of templates.
- With multiple locales: a top-level folder per locale.
- With multiple rows: a sub-folder per row label (e.g. `iPhone 6.9"`, `iPad 13"`).
- This mirrors the upload flow expected by App Store Connect's localized screenshot uploader.

### Export folder memory

- Screenshot Bro remembers the last folder you exported to (security-scoped bookmark).
- Toggle **Open export folder on success** in Settings to auto-reveal the result in Finder.

### Preview vs export

- Use the **Export Preview** button in the template control bar to render a single template to a preview window — handy for spot-checking without going through the full export flow.
- Editor and export must always match exactly. If they don't, please report it as a bug.

---

## App Store Connect

*Upload screenshots straight from Screenshot Bro.*

Connect your App Store Connect API key once and Screenshot Bro can upload exported screenshots to a specific app version without leaving the app.

### Set up an API key

- Go to **App Store Connect > Users and Access > Integrations > App Store Connect API**.
- Create a key with **App Manager** access. Download the `.p8` private key file (you can only download it once).
- Note the **Issuer ID** and **Key ID**.
- In Screenshot Bro: **Settings > App Store Connect**, paste the Issuer ID and Key ID, and import the `.p8` file.

### Uploading

- Run an export first, then click **Upload to App Store Connect** in the toolbar.
- Pick the app and version. Screenshot Bro maps each row to the right device family automatically.
- You can preview which screenshots will be uploaded for which locale before confirming.

> **Tip:** App Store Connect allows up to 10 screenshots per device family per locale. Screenshot Bro respects template ordering so the first 10 templates in each row will be uploaded in order.

---

## iCloud Sync

*Edit on one Mac, continue on another.*

iCloud sync keeps your project library in iCloud Drive (`iCloud.xyz.tleskiv.screenshot`). Changes made on one Mac propagate to others signed into the same iCloud account.

### Enabling

- **Settings > General > iCloud Sync** — toggle on.
- First-time enable migrates your local project library into iCloud. A progress indicator shows the migration.
- Disabling does **not** delete your iCloud data — your projects remain in the iCloud container until you delete them manually.

### How conflicts are resolved

- Each project is merged using a **last-writer-wins** strategy at the field level. The most recently edited shape, row, or background wins.
- Deletions are tracked as **tombstones** for 30 days, so a delete on Mac A correctly propagates to Mac B even if the device is offline at the moment of deletion.
- File coordination (`NSFileCoordinator`) prevents corruption from concurrent reads/writes.

### Knowing what's syncing

- The toolbar shows an iCloud status icon when an upload or download is in progress.
- Behind the scenes, an `NSMetadataQuery` watches each project for upload/download progress.

> **Tip:** If sync seems stuck, open Finder > iCloud Drive > Screenshot Bro and check whether files are still uploading. Toggling iCloud off and on again forces a re-scan.

---

## Settings & Defaults

*Tune the app to match your workflow.*

### General

- **Appearance** — Auto / Light / Dark.
- **Language** — override the app interface language. Requires a relaunch.
- **Default screenshot size** — used when creating new rows.
- **Default device** — pre-selects a device category and model for new rows.
- **Default templates per row** — number of empty templates a new row starts with.
- **Default zoom** — initial zoom level when opening the app.
- **Confirm before deleting** — show a confirmation prompt for destructive actions on rows and screenshots.
- **Project order** — Creation date or Manual.
- **Custom fonts** — manage imported `.otf`/`.ttf` files.
- **iCloud sync** — toggle and check status.
- **Back up projects** — write a one-off zip of your project library to a folder you choose.

### Export

- **Format** — PNG or JPEG.
- **Scale** — 1×, 2×, 3×.
- **Open export folder on success** — auto-reveal results in Finder.
- **Last export folder** — Screenshot Bro remembers and reuses your folder choice.

### App Store Connect

- API key, Issuer ID, Key ID. See the App Store Connect section.

### Purchase

- Current plan, restore purchases, manage subscription.

### Attributions

- Credits and licenses for fonts, icons, and bundled assets.

---

## Free vs Pro

*What's included and where Pro unlocks more.*

### Free tier

- **1 project** — you can keep editing it forever.
- **3 rows** per project.
- **5 templates** per row.
- Full access to all device frames, shapes, locales, and export resolutions.
- Watermark-free exports.

### Pro

- Unlimited projects, rows, and templates.
- App Store Connect upload.
- iCloud sync.
- Future Pro-only features as they ship.

### Buying or restoring

- **Settings > Purchase** lists the available plans. RevenueCat handles the transaction.
- **Restore Purchases** brings back an existing subscription on a new Mac.
- Subscriptions are managed through your Apple ID; cancellations happen via System Settings > Apple ID > Subscriptions.

> **Tip:** Pro paywall messages adapt to context — the prompt you see when adding a 4th row is different from the one you see when adding a 6th template, so you always know exactly which limit you're hitting.

---

## Tips & Tricks

*Small things that save time.*

- **Drop folders, not files.** Drag a folder of screenshots onto the canvas — Screenshot Bro will batch-import and route by device size.
- **Span backgrounds for storytelling.** Turn on row spanning and use a wide gradient or panoramic image to make a 3-template carousel feel like one continuous scene.
- **Lock aspect when resizing icons** by holding **⇧** while dragging a corner handle.
- **Duplicate while dragging** with **⌥**. Combined with snap, this is the fastest way to lay out a row of equal-sized cards.
- **Type rotation degrees directly.** The rotation field accepts text input — type `45` for an exact 45° rotation instead of dragging.
- **Use the SVG button for icons.** SVG scales infinitely, so your hero icon stays crisp at 1×, 2×, or 3× export.
- **Re-translate after editing base text.** If you change the base headline, run **Locale > Re-Translate All Text…** so every locale picks up the new wording.
- **Use Invisible category for clipped designs.** When you want the screenshot to bleed off the canvas with no bezel, pick the Invisible device category.
- **Pin frequently used projects.** Right-click in the project picker to pin and keep them at the top.
- **Preview before exporting.** The export preview button on each template renders just that one template — handy for spot-checks.
- **Custom fonts persist.** Imported fonts are bundled per project, so a project shared via iCloud or zip backup keeps its typography.

---

## Keyboard Shortcuts

*Everything you can do without the mouse.*

### File

| Keys | Action |
|------|--------|
| `⌘N` | New project |

### Edit

| Keys | Action |
|------|--------|
| `⌘C` | Copy selected shapes (or text in fields) |
| `⌘X` | Cut selected shapes |
| `⌘V` | Paste shapes |
| `⌘A` | Select all shapes in the active row |
| `⌘D` | Duplicate selected shapes / row |
| `Delete` | Delete selected shapes |
| `Esc` | Deselect |
| `⌘⇧]` | Bring shape to front |
| `⌘⇧[` | Send shape to back |
| `← → ↑ ↓` | Nudge selection by 1px |
| `⇧ + Arrow` | Nudge selection by 10px |
| `⌥ + Drag` | Duplicate while dragging |

### View

| Keys | Action |
|------|--------|
| `⌘+` | Zoom in |
| `⌘−` | Zoom out |
| `⌘0` | Actual size (100%) |
| `F` | Focus on selection |
| `Middle-click + drag` | Pan canvas |

### Locale

| Keys | Action |
|------|--------|
| `⌘]` | Next locale |
| `⌘[` | Previous locale |
| `⌘⌥0` | Switch to base locale |

### Text editing

| Keys | Action |
|------|--------|
| `Double-click text` | Enter inline edit mode |
| `Esc / click outside` | Commit text edit |

---

## Support & Feedback

*We read every message.*

### Get in touch

- Email: [leskiv.taras@gmail.com](mailto:leskiv.taras@gmail.com)

### When reporting a bug

To help us reproduce, please include:

- macOS version (Apple menu > About This Mac).
- Screenshot Bro version (Apple menu > About Screenshot Bro).
- Steps to reproduce, ideally with a screen recording.
- If the issue affects a project, **Settings > Export > Back Up Projects** and attach the resulting backup so we can reproduce on the exact data.

### Legal

- [Privacy Policy](privacy.md)

> **Tip:** Loved the app? An App Store review helps tremendously and keeps Screenshot Bro independent.
