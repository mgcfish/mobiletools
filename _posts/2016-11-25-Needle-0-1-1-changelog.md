---
layout: needle-post
title: "Needle V0.1.1"
description: New Release
author: "Marco Lancini"
categories:
  - needle
  - blog
tags:
---

## Needle V0.1.1 Released

### Changelog [0.1.1] - 2016-11-25


### Added
- **[CORE]** Support for plist files into print_cmd_output
- **[CORE]** `move` function for Remote operations
- **[CORE]** Automatically install Theos
- **[CORE]** Automatically install SSL Kill Switch
- **[CORE]** Add `validate_editor` (`core/framework/module`)
- **[CORE]** Parametrize `module_run` (`core/framework/module`)
- **[CORE]** Centralized utility for user interaction
- **[MODULE]** Theos integration (`hooking/theos/theos_tweak`)
- **[MODULE]** List installed Tweaks (`hooking/theos/list_tweaks`)
- **[MODULE]** Frida Script: print view hierarchy (`hooking/frida/script_dump-ui`)
- **[MODULE]** Install Burp Proxy CA Certificate (`comms/certs/install_ca_burp`)
- **[MODULE]** Allow using nano to edit hosts file (`various/hosts`) _[from @tghosth]_
- **[MODULE]** Automatically print row counts for standard tables in Cache.db files (`storage/data/files_cachedb`) _[from @tghosth]_
- **[MODULE]** Automatically print row counts for standard tables in SQL files (`storage/data/files_sql`) _[from @tghosth]_
- **[MODULE]** View Server Certificate (`comms/certs/view_cert`) _[from @tghosth]_
- **[MODULE]** Pull IPA: pull the binary as well as the .ipa file (`binary/pull_ipa`) _[from @tghosth]_

### Fixed
- **[CORE]** Sanitization of parsed plist files
- **[CORE]** App metadata: show all URI handlers
- **[CORE]** Invalid characters when parsing plist files
- **[CORE]** Minor on Remote Operations' wrapper: `list_dir` and `cat_file`
- **[MODULE]** Dump entire keychain _[idea from @tghosth]_
- **[MODULE]** `storage/caching/screenshot`: OS X support for rendering preview images
- **[MODULE]** Error saving files in `storage/data/files_*` modules _[from @tghosth]_
- **[MODULE]** Run proxy regular even without selecting a target app
- **[MODULE]** File monitoring: automatically detect folder to monitor (regression)


To stay updated, remember to also follow [@mwrneedle](https://twitter.com/mwrneedle) on Twitter!
