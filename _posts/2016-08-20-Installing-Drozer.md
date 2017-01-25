---
layout: drozer-guide
title: "Installing Drozer"
description: abcde
categories:
  - drozer
  - usage
tags:
---

## Installing Drozer

### Arch Linux

`yaourt -S drozer`

### Debian Linux

`dpkg -i drozer-2.3.4.deb`

### Building from Source

```
git clone https://github.com/mwrlabs/drozer/
cd drozer
python setup.py build
python setup.py install
```

### Installing the Agent

Drozer can be installed using Android Debug Bridge (adb).

`$ adb install drozer.apk`
