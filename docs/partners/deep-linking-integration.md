# QRS.SG QR Code Deep Linking Integration Guide

For Enterprise App Partners

---

## 📖 Overview

QRS.SG enables partners to create QR codes that, when scanned, take users directly into your mobile app instead of being interrupted by a browser prompt. This creates a seamless user experience for QR code interactions.

For this to work on iOS (Universal Links) and Android (App Links), both sides must be configured:

1. **QRS.SG will host the necessary verification files** (`apple-app-site-association` and `assetlinks.json`) under `https://qrs.sg/.well-known/`.

2. **Your app must declare qrs.sg as a recognized domain** in its configuration (iOS entitlements / Android manifest).

!!! warning "Important"
    Without changes on the app side, QR code scans will always open in the browser first.

---

## 🛠 iOS Setup (Universal Links)

### 1. Add Associated Domains Entitlement

In your iOS app project:

1. Open your `.entitlements` file.
2. Under Associated Domains, add:

```
applinks:qrs.sg
```

If you also serve from a subdomain, add:

```
applinks:www.qrs.sg
```

### 2. AASA File

QRS.SG will host the required `apple-app-site-association` file at:

```
https://qrs.sg/.well-known/apple-app-site-association
```

This file will contain your app's Team ID + Bundle ID (appID) and the paths you want enabled.

!!! info "Note"
    You do not need to host this file yourself; QRS.SG manages it.

### 3. Rebuild & Release

1. Recompile your app with the updated entitlements.
2. Submit to App Store / TestFlight.
3. **Test**: scanning a QR code with `https://qrs.sg/...` should now open directly in your app.

---

## 🛠 Android Setup (App Links)

### 1. Update AndroidManifest.xml

In your Android app, declare an intent filter for QRS.SG:

```xml
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />

    <data android:scheme="https"
          android:host="qrs.sg"
          android:pathPrefix="/" />
</intent-filter>
```

This tells Android that `https://qrs.sg/*` URLs should be opened directly by your app.

### 2. Provide SHA-256 Certificate Fingerprint

Share your App signing SHA-256 fingerprint with the QRS.SG team.

**Found in**: Google Play Console → App Integrity → App signing key certificate.

QRS.SG will add this fingerprint into the `assetlinks.json` file hosted at:

```
https://qrs.sg/.well-known/assetlinks.json
```

### 3. Rebuild & Release

1. Recompile your Android app with the updated manifest.
2. Submit to Google Play / distribute APK.
3. **Test with**:

```bash
adb shell pm get-app-links your.package.name
```

You should see `qrs.sg` listed as verified.

---

## 🔄 Maintenance Notes

### No Redirects
The first tap must land on `qrs.sg`. If the URL redirects to another host, the OS will treat it as a normal web link.

### Version Requirement
Only users who upgrade to your app build with the updated entitlements/manifest will get seamless deep linking. Older app versions will continue to see a browser interstitial.

### Rotating Keys
If your signing key changes, you must provide QRS.SG with the new SHA-256 fingerprint so we can update `assetlinks.json`.

---

## ✅ Checklist for Partners

- [ ] Add `applinks:qrs.sg` in iOS entitlements.
- [ ] Add `<intent-filter>` for `qrs.sg` in AndroidManifest.xml.
- [ ] Send QRS.SG your iOS Team ID + Bundle ID and Android signing SHA-256 fingerprint.
- [ ] Rebuild and release updated app builds.
- [ ] Test using Safari (iOS) or `adb pm get-app-links` (Android).

---

## 🎯 Result

Once complete, QRS.SG QR codes will take your users directly into your app, creating a seamless user experience for QR code interactions.

## 📞 Support

For technical support or questions about deep linking integration:

- **Email**: partners@qrs.sg
- **Documentation**: [QRS.SG Documentation](https://qrs-sg.github.io/documentation/)
- **GitHub Issues**: [Report Issues](https://github.com/QRS-SG/QRS-SG.github.io/issues)

## 🔗 Related Resources

- [iOS Universal Links Documentation](https://developer.apple.com/ios/universal-links/)
- [Android App Links Documentation](https://developer.android.com/training/app-links)
- [QRS.SG API Reference](../user-guide/api-reference.md)
- [Partner Onboarding Guide](partner-onboarding.md)
