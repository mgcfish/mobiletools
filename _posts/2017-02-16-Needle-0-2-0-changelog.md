---
layout: needle-post
title: "Needle V0.2.0"
description: New Release
categories:
  - needle
  - blog
tags:
---

## Needle V0.2.0 Released

### [0.2.0] - 2017-02-16

### Added
- **[CORE]** Preliminary support for iOS10
- **[CORE]** Support for persisting command history across sessions
- **[CORE]** Improved metadata parsing for extensions
- **[CORE]** Improved issues recognition from metadata
- **[CORE]** Improved plist parsing
- **[CORE]** Star out password _[from @tghosth]_
- **[MODULE]** Frida Script: TLS Pinning Bypass (`hooking/frida/script_pinning_bypass`)
- **[MODULE]** Frida Script: Keychain Dumper (`hooking/frida/script_dump-keychain`) _[from @bernard-wagner]_
- **[MODULE]** Frida Script: iCloud Backups (`hooking/frida/script_documents-backup-attr`) _[from @bernard-wagner]_
- **[MODULE]** Frida Script: Anti Hooking Checks (`hooking/frida/script_anti-hooking-check`) _[from @HenryHoggard]_
- **[MODULE]** Calculate binary checksums (`binary/checksums`) _[from @HenryHoggard]_
- **[MODULE]** Retrieve application container (`storage/data/container`)
- **[MODULE]** Strings: now look also in the application resources (`binary/strings`)
- **[MODULE]** Provisioning profile: Inspect the provisioning profile of the application (`binary/provisioning_profile`)

### Fixed
- **[CORE]** Modified the organization of modules into packages
- **[CORE]** App metadata: creation of binary path from MobileInstallation.plist
- **[CORE]** Plist wrapper using biplist
- **[CORE]** Multiple plist parsing issues _[from @tghosth]_
- **[CORE]** Paramiko hanging waiting for an EOF _[from @TheBananaStand]_
- **[MODULE]** Frida Script: print view hierarchy (`hooking/frida/script_dump-ui`) _[from @HenryHoggard]_
- **[MODULE]** Improved SQLite DB identification by reducing false positives and false negatives _[from @HenryHoggard]_
- **[MODULE]** Editing with different editors _[from @tghosth]_
- **[MODULE]** Clean storage does not need to require a target

### Removed
- **[CORE]** Unused dependencies


To stay updated, remember to also follow [@mwrneedle](https://twitter.com/mwrneedle) on Twitter!
