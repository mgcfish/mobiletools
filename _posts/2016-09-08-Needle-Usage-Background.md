---
layout: needle-guide
title: "Background Jobs"
description: 
categories:
  - needle
  - usage
tags:
---


## Background Jobs

Needle also has support for background jobs that can be left running during the execution of other modules. Some modules, especially those containing the "monitor" keyword in their name, rely on such background jobs.

```
[needle] > use dynamic/monitor/files
[needle][files] > info

Name: Monitor File changes
Path: modules/dynamic/monitor/files.py
Author: @LanciniMarco (@MWRLabs)

Description:
    Monitor the app data folder and keep track of modified files

Options:
 Name    Current Value                       Required   Description
 ------  -------------                       --------   -----------
 FOLDER                                      no         The folder to monitor (leave empty to use the app Data directory
 OUTPUT  /root/.needle/tmp/modifiedfiles.txt no         Full path of the output file


[needle][files] > run
[*] Checking connection with device...
[+] Already connected to: 127.0.0.1
[+] Target app: com.highaltitudehacks.dvia
[+] Monitoring: /private/var/mobile/Containers/Data/Application/031CAB32-6115-4613-B56F-CFF61BCED692
[*] Monitoring in background...Kill this process when you want to see the dumped content

[needle] > 
```

The "`jobs`" command can be used to list all the currently running background processes.

```
[needle] > jobs
[+] Running jobs:
 0 - dynamic_monitor_files
[needle] > 
```

The "`kill`" command can then be used to stop a background job, and therefore retrieve its output.

```
[needle][files] > kill 0
[D] [REMOTE CMD] Stopping Remote Background Command [pid: 510]
[D] [REMOTE CMD] Remote Command: kill 510
[*] Retrieving output file...
[*] Pulling: /var/root/needle/fsmon -> /root/.needle/tmp/modifiedfiles.txt
[+] Content of file '/root/.needle/tmp/modifiedfiles.txt': 
FSE_CREATE_FILE 512 "DamnVulnerableIO" /private/var/mobile/Containers/Data/Application/05F34A75-55C6-41E4-BB51-0F3777DF6D97/tmp/cy-TS2mr3.dylib
FSE_DELETE 512 "DamnVulnerableIO" /private/var/mobile/Containers/Data/Application/05F34A75-55C6-41E4-BB51-0F3777DF6D97/tmp/cy-TS2mr3.dylib
FSE_CONTENT_MODIFIED 512 "DamnVulnerableIO" /private/var/mobile/Containers/Data/Application/05F34A75-55C6-41E4-BB51-0F3777DF6D97/tmp/cy-TS2mr3.dylib
FSE_XATTR_MODIFIED 512 "DamnVulnerableIO" /private/var/mobile/Containers/Data/Application/05F34A75-55C6-41E4-BB51-0F3777DF6D97/Library/Private Documents/Parse
FSE_XATTR_MODIFIED 512 "DamnVulnerableIO" /private/var/mobile/Containers/Data/Application/05F34A75-55C6-41E4-BB51-0F3777DF6D97/Library/Application Support/FlurryFiles
...
[*] A copy of the output has been saved at the following location: /root/.needle/tmp/modifiedfiles.txt
[needle] > 
```
