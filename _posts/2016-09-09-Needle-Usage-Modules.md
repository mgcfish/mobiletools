---
layout: needle-guide
title: "Modular Approach"
description: 
categories:
  - needle
  - usage
tags:
---


## Modular Approach

Another goal that shaped Needle's design was for it to be easily extensible. That's the reason why every feature has been wrapped in its own module.

Since every module focuses on a particular task, with the core handling common problems (like communication with the device, actual execution of commands, etc.), creation of new modules is a matter of a few lines of python code.

The "`show modules`" command can be used to list all the modules currently available in the framework.

```
[needle][install] > show modules

Binary
------
 binary/class_dump
 binary/compilation_checks
 binary/install
 binary/metadata
 binary/pull_ipa
 binary/shared_libraries
 binary/strings

Comms
-----
 comms/certs/delete_ca
 comms/certs/export_ca
...
```

Otherwise, the "`search <query>`" command can be used to search available modules that match the query.

```
[needle] > search binary
[*] Searching for "binary"...

Binary
------
 binary/class_dump
 binary/compilation_checks
 binary/install
 binary/metadata
 binary/pull_ipa
 binary/shared_libraries
 binary/strings

Storage
-------
 storage/data/files_binarycookies 
```

Once selected, the "`info`" command can be used to show details of a particular module.

```
[needle] > use binary/strings
[needle][strings] > info

Name: Strings
Path: modules/binary/strings.py
Author: @LanciniMarco (@MWRLabs)

Description:
Find strings in the (decrypted) application binary, then try to extract URIs and ViewControllers

Options:
 Name     Current Value                    Required   Description
 -------  -------------                    --------   -----------
 ANALYZE  True                             no         Analyze recovered strings and try to recover URI
 FILTER                                    no         Filter the output (grep)
 LENGTH   10                               yes        Minimum length for a string to be considered
 OUTPUT   /root/.needle/tmp/strings.txt    no         Full path of the output file 
```

Or, to only get the available options:

```
[needle][strings] > show options
 Name     Current Value                    Required   Description
 -------  -------------                    --------   -----------
 ANALYZE  True                             no         Analyze recovered strings and try to recover URI
 FILTER                                    no         Filter the output (grep)
 LENGTH   10                               yes        Minimum length for a string to be considered
 OUTPUT   /root/.needle/tmp/strings.txt    no         Full path of the output file 
```

Like the global options, even module-specific ones can be edited with the "`set`" and "`unset`" commands.

```
[needle][strings] > set FILTER password
FILTER => password
[needle][strings] > show options
 Name     Current Value                    Required   Description
 -------  -------------                    --------   -----------
 ANALYZE  True                             no         Analyze recovered strings and try to recover URI
 FILTER   password                         no         Filter the output (grep)
 LENGTH   10                               yes        Minimum length for a string to be considered
 OUTPUT   /root/.needle/tmp/strings.txt    no         Full path of the output file 
```

When all the options are set as preferred, the "`run`" command can be used to start the module's execution. If a target app has not been selected yet (with the global option "`TARGET_APP`" still unset), Needle will first launch a wizard that will help the user in selecting a target.

```
[needle][strings] > run
[*] Checking connection with device...
[+] Already connected to: 127.0.0.1
[V] Creating temp folder: /var/root/needle/
[*] Target app not selected. Launching wizard...
[V] Refreshing list of installed apps...
[+] Apps found:
    0 - com.highaltitudehacks.dvia
    1 - uk.co.bbc.newsuk
Please select a number: 0
[+] Target app: com.highaltitudehacks.dvia
[*] Decrypting the binary...
[?] The app might be already decrypted. Trying to retrieve the IPA...
[V] Decrypted IPA stored at: /var/root/needle/decrypted.ipa
[*] Unpacking the decrypted IPA...
[V] Analyzing binary...
[+] The following strings has been found: 
     %@: Unable to get password of credential %@
     %s -- Cannot be used in OpenSSL mode. An IV or password is required
     Both password and the key (%d) or HMACKey (%d) are set.
     CFHTTPMessageAddAuthentication(httpMsg, _responseMsg, (__bridge CFStringRef)_credential.user, (__bridge CFStringRef)password, kCFHTTPAuthenticationSchemeBasic, _httpStatus == 407)
     Cannot sign up without a password.
     Congrats! You've found the right username and password!
     Huh, couldn't get password of %@; trying again
     Please enter a password
     T@"NSString",&,N,V_password
     T@"NSString",C,N,V_password
     T@"UITextField",&,N,V_passwordTextField
     ...
[*] Saving output to file: /root/.needle/tmp/strings.txt
```

Finally, the "`show source`" command can be used to inspect the actual source code of the selected module.

```
[needle][strings] > show source
 1|from core.framework.module import BaseModule
 2|
 3|
 4|class Module(BaseModule):
 5|    meta = {
 6|           'name': 'Strings',
 7|           'author': '@LanciniMarco (@MWRLabs)',
 8|           'description': 'Find strings in the (decrypted) application binary, then try to extract URIs and ViewControllers',
 9|           'options': (
10|                      ('length', 10, True, 'Minimum length for a string to be considered'),
11|                      ('filter', '', False, 'Filter the output (grep)'),
12|                      ('output', True, False, 'Full path of the output file'),
13|                      ('analyze', True, False, 'Analyze recovered strings and try to recover URI'),
14|          ),
15|    }
16|
17|    # ====================================================================
18|    # UTILS
19|    # ====================================================================
20|    def __init__(self, params):
21|        BaseModule.__init__(self, params)
...
```
