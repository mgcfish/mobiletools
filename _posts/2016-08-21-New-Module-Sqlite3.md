---
layout: drozer-post
title: "New Module: Sqlite3"
description: A sqlite3 setup module has been added to drozer
author: "Henry Hoggard"
categories:
  - drozer
  - blog
tags:
  - module
---

## New Module: Sqlite3

We have added an installer module for Sqlite3 to the [drozer-modules](https://github.com/mwrlabs/drozer-modules) repository. 

* [Source Code](https://github.com/mwrlabs/drozer-modules/blob/master/mwrlabs/tools/setup/sqlite3.py)

### Install module

```
$ drozer module install mwrlabs.tools.setup.sqlite3
```

### Usage

```
dz> run tools.setup.sqlite3
```