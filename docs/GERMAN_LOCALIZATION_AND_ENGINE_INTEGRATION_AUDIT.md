# German Localization and Engine Integration Audit

Branch audited: `Develop`

## Verified findings

### Engine submodule is not yet wired to the investigation fork

The Android repository's `.gitmodules` currently declares:

- Path: `melonDS-android-lib`
- URL: `https://github.com/SapphireRhodonite/melonDS-android-lib.git`
- Branch hint: `GBARumble_PR`

The Vulkan investigation work is being developed separately in `pascalstraube04-max/melonDS-android-lib`, branch `coverage-fix-engine-investigation`.

Changing the branch hint alone would not change the checked-out submodule revision. Before any build, the Android repository must be deliberately updated to the intended fork/revision and its gitlink must point at the reviewed engine commit. This has not been changed in this audit.

### German Android resources are absent

The base resource file `app/src/main/res/values/strings.xml` contains English UI strings. A fetch of `app/src/main/res/values-de/strings.xml` returned not found, so a German resource overlay is not currently present at that path. The base `strings.xml` is 89,228 characters and contains 929 `<string>` entries plus 35 `<string-array>` / `<plurals>` resources. These counts are a lower bound for the full translation workload because Compose/Kotlin/Java and native UI text still need a separate inventory.

Creating `values-de/strings.xml` is only one part of full localization. Compose screens and other Kotlin/Java UI code must also be audited for hard-coded user-facing English text, and any additional resource-backed labels, preference summaries, dialogs, notifications, and error messages must be covered.

## Required work before release

1. Inventory all user-visible strings across Android resources and Kotlin/Java/Compose sources.
2. Add a complete German resource overlay while preserving format placeholders, plurals, arrays, and resource names.
3. Replace user-facing hard-coded English text with resource-backed strings where practical.
4. Check native/core-facing UI labels and Vulkan diagnostic/settings names for language consistency.
5. Audit all screens and dialogs for untranslated text, truncation, and incorrect placeholder formatting.
6. Confirm the Android submodule gitlink resolves to the reviewed Vulkan engine fork commit.
7. Only then perform the final integration review and, after explicit user approval, build.

## Status

This is an audit note only. No submodule pointer, build workflow, or APK build was changed or triggered. Full German localization and engine integration are not yet complete.
