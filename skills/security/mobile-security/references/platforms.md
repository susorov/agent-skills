# Mobile platform checks

Read only applicable sections. Verify APIs, OS targets, plugin versions, and
release behavior; a wrapper name or debug configuration is not security evidence.

## Android

- Use Keystore-backed encryption for credentials in app-private storage. Choose
  key authentication requirements for the actual foreground/background use case;
  handle key invalidation and exclude sensitive data from backup/device transfer.
- Inspect the merged release manifest: export only required components, constrain
  access with appropriate permissions, and validate IPC callers and payloads.
  Prefer verified App Links (`autoVerify` plus domain association), but validate
  scheme/host/path and authorization for every sensitive link action.
- Inspect effective Network Security Config and the actual HTTP client: reject
  cleartext and invalid certificates/hostnames; keep debug trust overrides out of
  release.
- Restrict WebView navigation and file/content access; disable unused JavaScript.
  `addJavascriptInterface` can expose objects to all frames: do not combine it
  with untrusted content. Validate message origins and minimize bridge privileges.
- Check release debuggability, signing configuration, embedded secrets, permissions,
  and logging. Use `FLAG_SECURE` for sensitive screens when warranted; document limits.

## iOS

- Store credentials in Keychain with the least accessible class compatible with
  foreground/background use. Choose access groups, synchronization, and device-only
  migration policy explicitly; remove items and cached state on account changes.
- Verify Universal Links association/entitlements and validate destination/input
  plus authorization; custom URL schemes are not proof of caller identity.
- Inspect ATS exceptions and actual TLS handling. ATS governs URL Loading System
  traffic; lower-level/custom networking still needs explicit TLS verification.
- Limit WKWebView navigation and script message handlers to trusted origins/frames;
  validate each message and its authorization before invoking native operations.
- Check release entitlements, signing, debug features, privacy/permission usage
  declarations, logs, and SDK data collection. Obscure sensitive app-switcher views;
  screenshot notifications arrive after capture and cannot prevent screenshots.

## Flutter and React Native

- Inspect plugins and both native implementations. Prefer maintained secure
  storage backed by Keychain/Keystore; verify backup, accessibility, and deletion
  settings. React Native AsyncStorage, persisted JS state, and ordinary Flutter
  preferences/files are not secure token stores. Check analytics/crash payloads too.
- Verify each network path. Reject permissive Dart `badCertificateCallback`,
  custom trust managers, and hostname bypasses. Native ATS/Android settings alone
  do not prove that Dart sockets or custom/native clients enforce the same policy.
- Review WebView plugins and exposed native modules for trusted callers, input
  validation, and minimal capabilities. Exercise controls in release builds on
  each targeted OS; emulator or source-only checks leave device behavior unverified.

## Authentication and optional hardening

- Use system browser/authentication sessions for native OAuth, not embedded
  WebViews. Test callback binding, redirect matching, cancellation, and token cleanup.
- Biometrics may gate local key use; they do not establish backend authorization.
  If pinning is selected, plan key rotation, backup pins, and recovery; do not add
  trust-all fallbacks. Treat attestation/tamper checks as risk signals, not sole access control.

Sources: [Android Keystore](https://developer.android.com/privacy-and-security/keystore),
[App Links](https://developer.android.com/privacy-and-security/risks/unsafe-use-of-deeplinks),
[Android networking](https://developer.android.com/privacy-and-security/security-config),
[Android bridges](https://developer.android.com/privacy-and-security/risks/insecure-webview-native-bridges),
[Keychain](https://support.apple.com/guide/security/keychain-data-protection-secb0694df1a/web),
[ATS](https://developer.apple.com/documentation/security/preventing-insecure-network-connections),
[iOS screenshots](https://developer.apple.com/documentation/uikit/uiapplication/userdidtakescreenshotnotification),
[React Native security](https://reactnative.dev/docs/security),
[Dart TLS callbacks](https://api.dart.dev/dart-io/HttpClient/badCertificateCallback.html),
[Native OAuth](https://www.rfc-editor.org/rfc/rfc8252),
[MASVS](https://mas.owasp.org/MASVS/).
