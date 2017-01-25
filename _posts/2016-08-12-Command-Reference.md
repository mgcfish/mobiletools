---
layout: drozer-guide
title: "Command Reference"
description: 
categories:
  - drozer
  - usage
tags:
---

## Command Reference

| Command        | Description           |
| ------------- |:-------------|
| run  | Executes a drozer module
| list | Show a list of all drozer modules that can be executed in the current session. This hides modules that you do not have suitable permissions to run. | 
| shell | Start an interactive Linux shell on the device, in the context of the Agent process. | 
| cd | Mounts a particular namespace as the root of session, to avoid having to repeatedly type the full name of a module. | 
| clean | Remove temporary files stored by drozer on the Android device. | 
| contributors | Displays a list of people who have contributed to the drozer framework and modules in use on your system. | 
| echo | Print text to the console. | 
| exit | Terminate the drozer session. | 
| help | Display help about a particular command or module. | 
| load | Load a file containing drozer commands, and execute them in sequence. | 
| module | Find and install additional drozer modules from the Internet. | 
| permissions | Display a list of the permissions granted to the drozer Agent. | 
| set | Store a value in a variable that will be passed as an environment variable to any Linux shells spawned by drozer. | 
| unset | Remove a named variable that drozer passes to any Linux shells that it spawns. | 