---
layout: needle-guide
title: "Installing Needle"
description: 
categories:
  - needle
  - usage
tags:
---

## Installing Needle

## Manual

### **1. Device Setup**

The only prerequisite is a Jailbroken device, with the following packages installed:

* `Cydia`
* `OpenSSH`
* `Apt 0.7 Strict`



### **2. Workstation Setup**

#### Get a copy of Needle

```
git clone https://github.com/mwrlabs/needle.git
```


#### Install Dependencies

###### Kali

```
# Unix packages
sudo apt-get install python2.7 python2.7-dev sshpass sqlite3 lib32ncurses5-dev

# Python packages
sudo pip install readline
sudo pip install paramiko
sudo pip install sshtunnel
sudo pip install frida
sudo pip install mitmproxy
```


###### OS X

```
# Core dependencies
brew install python
brew install libxml2
xcode-select --install

# Python packages
sudo -H pip install --upgrade --user readline
sudo -H pip install --upgrade --user paramiko
sudo -H pip install --upgrade --user sshtunnel
sudo -H pip install --upgrade --user frida

# sshpass
brew install https://raw.githubusercontent.com/kadwanev/bigboybrew/master/Library/Formula/sshpass.rb

# mitmproxy
wget https://github.com/mitmproxy/mitmproxy/releases/download/v0.17.1/mitmproxy-0.17.1-osx.tar.gz
tar -xvzf mitmproxy-0.17.1-osx.tar.gz
sudo cp mitmproxy-0.17.1-osx/mitm* /usr/local/bin/

# libimobiledevice4
brew install -v --fresh automake autoconf libtool wget libimobiledevice
brew install -v --HEAD --fresh --build-from-source ideviceinstaller
```


## Using Docker

Coming soon...