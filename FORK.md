# Maintained TabType fork

This is Elliot-Becker's maintained fork of [nilava/TabType](https://github.com/nilava/TabType), based on upstream commit `b75ea0e22bc565e57b133b2625a49d7e8a24c789`. Upstream authorship and the MIT license are retained. This fork is not an official upstream release.

## Current scope

- Confine model download, loading, partial resume and deletion to validated regular files in the model directory.
- Authenticate executable updates with the installed certificate and code identifier, including every architecture in a universal binary.
- Gate keyboard, accessibility, clipboard, screen context and deferred insertion on the current application/field policy; invalidate stale work on focus, pause or settings changes.
- Capture only an unambiguous authorized window, including appearance probes; never substitute an unrelated window.
- Package locally built apps without forbidden Finder metadata and verify their signatures before reporting build success.

The first two changes are offered upstream as [PR #10](https://github.com/nilava/TabType/pull/10) and [PR #11](https://github.com/nilava/TabType/pull/11). Acceptance is the maintainer's decision and is not required to use this fork.

## Verification and limits

Run `DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer swift test`. Place a catalog-checksummed model in the gitignored `models/` folder to include native inference tests. Run `bash Scripts/test-update-signatures.sh` for real certificate, tampering and universal-binary fixtures in a disposable keychain. It never executes the fixture binaries and does not change certificate trust settings.

Build with `CONFIG=Release bash Scripts/build.sh app`. A local build without an installed signing identity is ad-hoc signed. It deliberately cannot install in-app updates: the updater requires a certificate-signed installed application. Rebuild and install verified source changes manually instead. No notarization or automatic transfer of macOS permission grants is implied.

Passing these checks is not a security certification. Accessibility metadata differs across apps, focus and input delivery are controlled by macOS, and cross-app/long-running behavior requires live checks. The pinned native framework is verified by SwiftPM's checksum; that does not constitute a complete audit of its implementation. Use trusted, checksum-verified models.

Writing history, clipboard context, screen recording, sync and verbose diagnostics should remain off until deliberately enabled and tested. Optional iCloud sync transmits encrypted writing/settings; inference itself stays local. Do not describe optional sync as data never leaving the Mac.

## Maintaining the fork

Keep `upstream` pointed at the original repository and `origin` at this fork. Import upstream changes into a review branch, inspect the diff, rerun security regressions and the full suite, then verify a new signed build. Submit narrow upstream changes independently of fork-specific documentation or preferences. Do not replace the installed app or publish a binary before its relevant checks pass.
