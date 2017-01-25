---
layout: needle-guide
title: "Basic Features"
description: 
categories:
  - needle
  - usage
tags:
---


## Basic Features


Although the power of Needle is comprised in its modules, the core itself offers some basic features that allow it to interact with the device.

 
#### Execute local command (`<cmd>`)
 
Needle's CLI is basically a shell itself, so it is possible to run commands on the local workstation just by typing them.

```
[needle] > cat /etc/hostname
[*] Executing Local Command: cat /etc/hostname
launchpad

[needle] > ifconfig lo
[*] Executing Local Command: ifconfig lo
lo Link encap:Local Loopback 
 inet addr:127.0.0.1 Mask:255.0.0.0
 inet6 addr: ::1/128 Scope:Host
 UP LOOPBACK RUNNING MTU:65536 Metric:1
 RX packets:48454 errors:0 dropped:0 overruns:0 frame:0
 TX packets:48454 errors:0 dropped:0 overruns:0 carrier:0
 collisions:0 txqueuelen:0 
 RX bytes:33149117 (31.6 MiB) TX bytes:33149117 (31.6 MiB)
 ```


#### Shell (`shell`)

Type "shell" to drop a shell on the remote device.

```
[needle] > shell
[*] Spawning a shell...
[*] Checking connection with device...
[V] Connection not present, creating a new instance
[V] Setting up USB port forwarding on port 2222
[V] Setting up SSH connection...
[+] Connected to: 127.0.0.1
Warning: Permanently added '[127.0.0.1]:2222' (RSA) to the list of known hosts.
MWR-iPhone:~ root# id
uid=0(root) gid=0(wheel) groups=0(wheel),...
```


#### Execute command on device (`exec_command <cmd>`)

To execute a single command on the remote device.

```
[needle] > exec_command id
[*] Checking connection with device...
[+] Already connected to: 127.0.0.1
[*] Executing: id
 uid=0(root) gid=0(wheel) groups=0(wheel)...
```



#### Push/pull files (```<push/pull> "<src>" "<dst>"```)

It is also possible to retrieve or upload files from/on the device. Note that both the source and destination paths must be enclosed in quotes.

```
[needle] > pull "/etc/hosts" "/tmp/iphone_hosts"
[*] Checking connection with device...
[+] Already connected to: 127.0.0.1
[*] Pulling: /etc/hosts -> /tmp/iphone_hosts
[needle] > cat /tmp/iphone_hosts
[*] Executing Local Command: cat /tmp/iphone_hosts
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting. Do not change this entry.
##
127.0.0.1 localhost
255.255.255.255 broadcasthost
::1 localhost
```
