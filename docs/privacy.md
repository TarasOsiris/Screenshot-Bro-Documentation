# Privacy Policy

*Effective date: March 25, 2026*

This Privacy Policy describes how Nineva Studios ("we", "us", or "our") handles information in connection with the **Screenshot Bro** application for macOS (the "App"). We are committed to protecting your privacy and being transparent about our practices.

## 1. Information We Do Not Collect

Screenshot Bro is designed to work entirely on your device. We do **not** collect, transmit, or store:

- Personal information (name, email address, phone number)
- Usage analytics or behavioral data
- Device identifiers for tracking purposes
- Location data
- Crash reports or diagnostics sent to our servers
- Advertising identifiers

The App contains **no analytics SDKs**, no advertising frameworks, and no telemetry or crash-reporting services.

## 2. Data Stored on Your Device

All projects, images, custom fonts, and settings you create in Screenshot Bro are stored locally on your Mac in the application's sandboxed container:

- **Project data** — screenshot layouts, shapes, text, backgrounds, and locale configurations (stored as JSON files in `~/Library/Application Support/screenshot/`)
- **Imported images and fonts** — copies of files you import into your projects
- **Preferences** — appearance mode, default export format, zoom level, and similar settings (stored in UserDefaults)

This data never leaves your device unless you explicitly enable iCloud sync (see Section 3) or export files to a location of your choice.

## 3. iCloud Sync (Optional)

Screenshot Bro offers an optional iCloud Drive sync feature that you can enable in the App's settings. When enabled:

- Your project files and imported images are synchronized to your personal iCloud Drive account so they are available across your Macs.
- Data is transmitted and stored using Apple's iCloud infrastructure. We do not operate any intermediate servers and have no access to your iCloud data.
- iCloud sync is governed by [Apple's Privacy Policy](https://www.apple.com/legal/privacy/).
- You can disable iCloud sync at any time in the App's settings. Disabling sync does not delete data already stored in iCloud; you can remove it via macOS System Settings > Apple Account > iCloud > Manage Storage.

## 4. In-App Purchases & Subscriptions (RevenueCat)

Screenshot Bro uses [RevenueCat](https://www.revenuecat.com/) to manage in-app purchase validation for the Pro entitlement, which can be unlocked via a one-time lifetime purchase or an auto-renewing subscription. When you start, renew, or restore a purchase:

- RevenueCat receives the transaction receipt from Apple's App Store to verify your entitlement. This is standard for all App Store purchases, including auto-renewing subscriptions.
- RevenueCat may process an anonymous app-specific identifier and purchase details (product ID, transaction and renewal dates, entitlement status, and subscription state such as expiration and renewal). No personal information such as your name or email is shared.
- For subscriptions, RevenueCat is also notified by Apple when a renewal succeeds, fails, is paused, or is cancelled, so the App can keep your entitlement state accurate. We do not see your billing details — Apple handles all payment processing.
- RevenueCat's handling of data is governed by their [Privacy Policy](https://www.revenuecat.com/privacy/).

If you do not make a purchase, no data is sent to RevenueCat beyond an initial anonymous entitlement check. Subscription terms, including auto-renewal and cancellation, are described in our [Terms of Use](terms.md).

## 5. Third-Party Services Summary

| Service | Purpose | Data shared |
|---------|---------|-------------|
| Apple iCloud Drive | Optional project sync | Project files (only if user enables sync) |
| RevenueCat | Purchase validation | Anonymous ID, transaction receipt |
| Apple App Store | In-app purchases | Standard App Store transaction data |

No other third-party services, SDKs, or frameworks receive data from the App.

## 6. Data Retention and Deletion

- **Local data** — all project data and preferences are removed when you delete the App, or you can manually delete them from `~/Library/Application Support/screenshot/`.
- **iCloud data** — disable sync in the App's settings, then remove files via macOS System Settings or iCloud Drive.
- **Purchase records** — managed by Apple and RevenueCat. You can contact RevenueCat to request deletion of any anonymous records associated with your transactions.

## 7. Children's Privacy

Screenshot Bro is not directed at children under the age of 13 and does not knowingly collect personal information from children. Since we do not collect personal information from any user, no special provisions are necessary.

## 8. Security

The App runs inside macOS App Sandbox, which restricts file system access and network capabilities. All data at rest is protected by macOS file-level encryption (FileVault) and iCloud encryption when applicable.

## 9. Changes to This Policy

We may update this Privacy Policy from time to time. The updated version will be posted at [https://screenshotbro.app/privacy](https://screenshotbro.app/privacy) with a revised effective date. We encourage you to review this page periodically.

## 10. Contact Us

If you have questions or concerns about this Privacy Policy or the App's data practices, please contact us:

**Nineva Studios**
[tleskiv@ninevastudios.com](mailto:tleskiv@ninevastudios.com)
