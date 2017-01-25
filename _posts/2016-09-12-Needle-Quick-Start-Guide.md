---
layout: needle-guide
title: "Quick Start Guide"
description: 
categories:
  - needle
  - usage
tags:
---

## Startup

#### Standard usage

To launch Needle, just open a console and type:

```
$ python needle.py
      __  _ _______ _______ ______         ______
      | \ | |______ |______ | \     |      |______
      | \_| |______ |______ |_____/ |_____ |______
                  Needle v0.0.3 [mwr.to/needle] 
    [MWR InfoSecurity (@MWRLabs) - Marco Lancini (@LanciniMarco)]

[needle] > help
Commands (type [help|?] <topic>):
---------------------------------
back exit info kill pull reload search shell show use
exec_command help jobs load push resource set shell_local unset

[needle] > show options
Name          Current Value   		Required   Description
------------  -------------   		--------   -----------
APP                            		no        Bundle ID of the target application (e.g., com.example.app). Leave empty to launch wizard
DEBUG         False            		yes       Enable debugging output
IP            127.0.0.1        		yes       IP address of the testing device (set to localhost to use USB)
OUTPUT_FOLDER /root/.needle/output  yes       Full path of the output folder, where to store the output of the modules
PASSWORD      alpine           		yes       SSH Password of the testing device
PORT          2222             		yes       Port of the SSH agent on the testing device (needs to be != 22 to use USB)
PUB_KEY_AUTH   True                 yes       Use public key auth to authenticate to the device. Key must be present in the ssh-agent if a 
SETUP_DEVICE  True             		yes       Set to true to enable auto-configuration of the device (installation of all the tools needed)
USERNAME      root             		yes       SSH Username of the testing device
VERBOSE       True             		yes       Enable verbose output

[needle] >
```

You will be presented with Needle's command line interface.

The tool has some global options (listed with the "`show options`" command, and set with the "`set <option> <value>`" command):

* **USERNAME, PASSWORD**: SSH credentials of the testing device (set by default to "root" and "alpine", respectively)
* **PUB_KEY_AUTH**: Use public key authentication to authenticate to the device. Key must be present in the ssh-agent if a passphrase is used
* **IP, PORT**: the session manager embedded in the core of Needle is able to handle SSH connections over Wi-Fi or USB. If SSH-over-USB is the chosen method, the IP option must be set to localhost ("set IP 127.0.0.1"), and PORT set to anything different from 22 ("set PORT 2222")
* **VERBOSE, DEBUG**: if set to True, they will enable verbose and debug logging, respectively
* **SETUP_DEVICE**: if set to True, Needle checks if all the dependencies needed are already present on the device, otherwise it will install them
* **APP**: this is the bundle identifier of the app to analyze (e.g., "com.example.app"). If it is not known beforehand, this field can be left empty. In this case, Needle will launch a wizard which prompts the user to select an app among those already installed on the device
* **OUTPUT_FOLDER**: this is the full path of the output folder, where Needle will store the output of the modules





#### Automated, using a resource file

Configuration of the global options can also be automated, using a resource file. First, create a resource file with the commands you want to have automatically executed. For example:

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
