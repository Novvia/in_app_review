# in_app_review

[![pub package](https://img.shields.io/pub/v/in_app_review.svg)](https://pub.dartlang.org/packages/in_app_review) [![pub points](https://badges.bar/in_app_review/pub%20points)](https://pub.dev/packages/in_app_review/score) [![likes](https://badges.bar/in_app_review/likes)](https://pub.dev/packages/in_app_review/score) [![popularity](https://badges.bar/in_app_review/popularity)](https://pub.dev/packages/in_app_review/score)

![In-App Review Android Demo](https://github.com/britannio/in_app_review/blob/master/in_app_review/screenshots/android.jpg)
![In-App Review IOS Demo](https://github.com/britannio/in_app_review/blob/master/in_app_review/screenshots/ios.png)

# Description
Flutter plugin that lets you show a system review pop up where users can leave a review for your app without needing to leave it. Alternatively you can open your store listing via a deep link.

Uses the [In-App Review](https://developer.android.com/guide/playcore/in-app-review) API on Android and the [SKStoreReviewController](https://developer.apple.com/documentation/storekit/skstorereviewcontroller) on iOS/MacOS


# Usage
Use the following code to attempt to display the system review pop up. Beware that this does **not** guarantee that the pop up will be displayed as the quota may have been exceeded. On IOS & MacOS, this quota is three times in a 365 day period. On Android it is unclear: https://developer.android.com/guide/playcore/in-app-review#quotas
```dart
import 'package:in_app_review/in_app_review.dart';

final InAppReview inAppReview = InAppReview.instance;

if (await inAppReview.isAvailable()) {
    inAppReview.requestReview();
}
```

Use the following code to open the Google Play Store on Android, the App Store on IOS & MacOS or the Microsoft Store on Windows. This is the recommended option if you want to permanently provide a button or other call-to-action to let users leave a review.
```dart
import 'package:in_app_review/in_app_review.dart';

final InAppReview inAppReview = InAppReview.instance;

inAppReview.openStoreListing(appStoreId: '...', microsoftStoreId: '...');
```

`appStoreId` is only required on IOS and MacOS and can be found in App Store Connect under General > App Information > Apple ID.

`microsoftStoreId` is only required on Windows.

# Guidelines
https://developer.apple.com/design/human-interface-guidelines/ios/system-capabilities/ratings-and-reviews/

https://developer.android.com/guide/playcore/in-app-review#when-to-request
https://developer.android.com/guide/playcore/in-app-review#design-guidelines

Since there is a quota on how many times the pop up can be shown, you should **not** trigger `requestReview()` via a button or other *call-to-action* option. Instead, you can reliably redirect users to your store listing via `openStoreListing()`.

# Testing
## Android
You must upload your app to the Play Store to test `requestReview()`. An easy way to do this is to build an apk/app bundle and upload it via [internal app sharing](https://play.google.com/apps/publish/internalappsharing/).

More details at https://developer.android.com/guide/playcore/in-app-review/test

## IOS
`requestReview()` can be tested via the IOS simulator or on a physical device. 
Note that `requestReview()` has no effect when testing via TestFlight [as documented](https://developer.apple.com/documentation/storekit/skstorereviewcontroller/2851536-requestreview#discussion).

`openStoreListing()` can only be tested with a physical device as the IOS simulator does not have the App Store installed.

## MacOS
This plugin can be tested by running your MacOS application locally.

# Cross Platform Compatibility
| Function             | Android | IOS | MacOS | Windows(UWP) |
|----------------------|---------|-----|-------|--------------|
| `isAvailable()`      | ✅       | ✅   | ✅     | ❌            |
| `requestReview()`    | ✅       | ✅   | ✅     | ❌            |
| `openStoreListing()` | ✅       | ✅   | ✅     | ✅            |
Upvote https://github.com/flutter/flutter/issues/64958 if you're interested in Windows support!

# Requirements
## Android
Requires Android 5 Lollipop(API 21) or higher and the Google Play Store must be installed.
## IOS
Requires IOS version 10.3
## MacOS
Requires MacOS version 10.14

Issues & pull requests are more than welcome!