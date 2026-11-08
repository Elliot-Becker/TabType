# Maintained TabType fork

This is Elliot-Becker's maintained fork of [nilava/TabType](https://github.com/nilava/TabType), based on upstream commit `b75ea0e22bc565e57b133b2625a49d7e8a24c789`. Upstream authorship and the MIT license are retained. This fork is not an official upstream release.

## Current scope

- Confine model download, loading, partial resume and deletion to validated regular files in the model directory; bound received bytes and validate resumed response ranges.
- Authenticate executable updates with the installed certificate and code identifier, including every architecture in a universal binary.
- Gate keyboard, accessibility, clipboard, screen context and deferred insertion on the current application/field policy; invalidate stale work on focus, pause or settings changes.
- Capture only an unambiguous authorized window, including appearance probes; never substitute an unrelated window.
- Canonicalize website rules consistently, reject stale inline actions and selected-text replacements, and revoke model context/alternatives with the originating field.
- Make learned-writing erasure durable across sync adoption, clear derived indexes, and report diagnostic deletion failures.
- Sign in clean temporary staging, retain a transport archive, and verify signed output before reporting build success.
- Avoid integer conversion traps in large finite numeric macros.
- Provide grouped native Settings navigation, keyword search, accessible slider labels, saved overrides for closed apps and a stable window layout. Restore the previous macOS prediction value only while TabType's own off-value remains unchanged.

Focused upstream contributions: [#10 model/download safety](https://github.com/nilava/TabType/pull/10), [#11 update identity](https://github.com/nilava/TabType/pull/11), [#12 context privacy and erasure](https://github.com/nilava/TabType/pull/12), [#13 signed packaging](https://github.com/nilava/TabType/pull/13), [#14 numeric macros](https://github.com/nilava/TabType/pull/14), and [#15 native Settings](https://github.com/nilava/TabType/pull/15). Acceptance is the maintainer's decision and is not required to use this fork.

## Verification and limits

Run `DEVELOPER_DIR=/Applications/Xcode.app/Contents/Developer swift test`. Place a catalog-checksummed model in the gitignored `models/` folder to include native inference tests. Run `bash Scripts/test-update-signatures.sh` for real certificate, tampering and universal-binary fixtures in a disposable keychain. It never executes the fixture binaries and does not change certificate trust settings.

Build with `CONFIG=Release bash Scripts/build.sh app`. A local build without an installed signing identity is ad-hoc signed. It deliberately cannot install in-app updates: the updater requires a certificate-signed installed application. Rebuild and install verified source changes manually instead. No notarization or automatic transfer of macOS permission grants is implied.

On 2026-11-08 the combined source suite passed 208 tests with 8 skips and no failures on Apple Silicon macOS 27.2 / Xcode 27.2. The Settings-only contribution independently passed its four focused tests. Its native layout regression reproduced the old clipping and passed at 780×540 and 860×620 in Aqua and Dark Aqua. Live UI checks covered search, keyboard navigation, close/reopen, scrolling and a saved override after the target app quit. The signed Release archive was unpacked and verified, and the installed copy's signature/hash matched the candidate.

Real Safari/helper privacy checks passed against synthetic fixtures for authorized OCR/appearance, policy denials, secure fields, origin/selection/navigation races, second-window exclusion and model cache revocation. These helper tests do not certify every browser or the global keyboard route. Final physical autocomplete acceptance of the latest rebuilt app remains pending its refreshed Accessibility grant; suggestions are left off until that check. There is no Developer ID notarization or long-running compatibility claim.

Learned-writing storage now uses an encrypted envelope with an erase watermark and per-document epoch. This version reads the old array format, but older versions cannot read the new envelope. Update all syncing Macs before relying on synced erasure; old/offline clients and backups need separate attention. Personalization currently reuses shared writing history and custom instructions, without separate memory per contact. No controlled quality comparison establishes parity with Cotypist.

Passing these checks is not a security certification. Accessibility metadata differs across apps, focus and input delivery are controlled by macOS, and cross-app/long-running behavior requires live checks. The pinned native framework is verified by SwiftPM's checksum; that does not constitute a complete audit of its implementation. Use trusted, checksum-verified models.

Writing history, clipboard context, screen recording, sync and verbose diagnostics should remain off until deliberately enabled and tested. Optional iCloud sync transmits encrypted writing/settings; inference itself stays local. Do not describe optional sync as data never leaving the Mac.

## Maintaining the fork

Keep `upstream` pointed at the original repository and `origin` at this fork. Import upstream changes into a review branch, inspect the diff, rerun security regressions and the full suite, then verify a new signed build. Submit narrow upstream changes independently of fork-specific documentation or preferences. Do not replace the installed app or publish a binary before its relevant checks pass.
