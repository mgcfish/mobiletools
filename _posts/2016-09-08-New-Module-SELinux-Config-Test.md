---
layout: drozer-post
title: "New Module: SELinux Permissions Check"
description: New module for assessing SELinux Permissions.
categories:
  - drozer
  - blog
tags:
  - module
  - device review
---

## SELinux Config Test

Drozer traditionally has been focused on assessing the security of applications, we are extending Drozer's functionality with a set of modules to help perform device reviews.  These modules look for security weaknesses such as permission issues, mis-configurations and unpatched vulnerabilities.

The module `device.selinux.permissions` scans for basic permission misconfigurations in the SELinux installation.

#### Usage

`dz> run device.selinux.permissions`

#### Demo

<script type="text/javascript" src="https://asciinema.org/a/7saov7hfl3acvoxohy1oz10g4.js" id="asciicast-7saov7hfl3acvoxohy1oz10g4" async></script>


#### Future Work

* Analyse individual SELinux policies.