---
layout: drozer-guide
title: "Installing Modules"
description: abcde
categories:
  - drozer
  - usage
tags:
---

## Installing Modules

Out of the box, drozer provides modules to investigate various aspects of the Android platform, and a few
remote exploits.
You can extend drozer’s functionality by downloading and installing additional modules

### Finding Modules

The official drozer module repository is hosted alongside the main project on Github. This is automatically set
up in your copy of drozer. You can search for modules using the `module` command:

```bash
dz> module search root
metall0id.root.cmdclient
metall0id.root.exynosmem.exynosmem
metall0id.root.scanner_check
metall0id.root.ztesyncagent
```
For more information about a module, pass the `–d` option:

```
dz> module search cmdclient -d
metall0id.root.cmdclient
 Exploit the setuid-root binary at /system/bin/cmdclient on certain devices to gain a
 root shell. Command injection vulnerabilities exist in the parsing mechanisms of the
 various input arguments.
 This exploit has been reported to work on the Acer Iconia, Motorola XYBoard and
 Motorola Xoom FE.
 ```

### Installing Modules

 You install modules using the `module` command:

 ```
dz> module install cmdclient
Processing metall0id.root.cmdclient... Done.
Successfully installed 1 modules, 0 already installed
```

This will install any module that matches your query. Newly installed modules are dynamically loaded into your
console and are available for immediate use