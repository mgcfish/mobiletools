---
layout: needle-usage
title: User Guide
description: "Needle Usage"
permalink: /needle/usage.html
---

## Startup

#### Standard usage

To launch Needle, just open a console and type:

```
python needle.py
```

You will be presented with Needle's command line interface.


#### Automated, using a resource file

First, create a resource file with the commands you want to have automatically executed. For example:

```
$ cat config.txt
# This is a comment, it won't be executed
set DEBUG False
set VERBOSE False

# If SETUP_DEVICE is set to True, 
# Needle will automatically install all the required tools on the device
set SETUP_DEVICE False

set IP 192.168.0.10
set PORT 5555
set APP com.example.app
use binary/metadata
```

Then, launch Needle and instruct it to load the resource file:

```
python needle.py -r config.txt
```

## Usage Walkthrough 

A complete walkthrough on how to quickly get up to speed with Needle can be found on the MWR Labs website: [https://labs.mwrinfosecurity.com/blog/needle-how-to/](https://labs.mwrinfosecurity.com/blog/needle-how-to/)