# macos-iso-builder: 2026 audit (2026-10-03)

Audit only. No code changed. Web-sourced claims are from search snippets, not primary Apple/GitHub docs; verify before relying on them.

## Blocker for the Unraid goal
- macOS 27 "Golden Gate" shipped 2026-09-14 and is **Apple silicon only** (M1+). Tahoe 26 is the last Intel release. (Sources: MacRumors release-date article, Smartprix/Gigazine WWDC26 coverage.)
- Unraid runs x86 QEMU/KVM. A macOS 27 guest is not expected to boot there (no Apple silicon, and the x86 OpenCore path ends at Tahoe). **Practical newest target for Unraid: Tahoe 26.x.** This repo already supports Tahoe.

## What is stale or needs checking in the repo
| Item | Location | Status |
|---|---|---|
| No macOS 27 entry | `mkmaciso` lines ~11, 187, 210, 263, 306, 1433-1434, 1564-1576; workflow dropdowns | Missing. Adding it is mechanical (version/codename maps, menu, help text) but the "Install macOS Golden Gate" app name and catalog availability are unverified. |
| Full installer runner `macos-15-intel` | `build-macos-image.yml:148` | Intel label is long-term deprecated; Apple silicon required after fall 2027. Works today, will break later. macOS 27 installers likely need an arm64 runner (`macos-26`). |
| Recovery runner `macos-latest` | `build-recovery.yml:140` | Now resolves to macos-26 (arm64). Fine, but unpinned. |
| Recovery board IDs | `build-recovery.yml:176-189` | Tahoe uses `Mac-CFF7D910A743CAAF ... -os latest`. No macOS 27 entry; board ID/`-os` flag for 27 unknown. macrecovery.py is pulled from OpenCorePkg `master` (unpinned). |
| Catalina/Mojave hardcoded Apple CDN URLs | `mkmaciso:44-46` | Could rot. Not tested. |
| README says "through macOS Tahoe (26, 2025)" | `README.md` | Needs update if 27 is added. Upstream links point to LongQT-sea, not this fork. |

## Not verified (no macOS runner here)
- Whether `mkmaciso` still succeeds against Apple's current catalog for Tahoe.
- Whether Apple's recovery servers serve a macOS 27 image to macrecovery.

## Suggested next steps
1. Skip macOS 27 for Unraid. Use the Tahoe (or Sequoia, for better OSX-KVM compatibility) Recovery ISO.
2. Trigger the Recovery workflow in a fork to confirm Tahoe still builds today.
3. Optional: add macOS 27 for completeness (arm64 runner, new board ID), lower priority.
4. Replace `macos-15-intel` with an arm64 runner or document the sunset.
