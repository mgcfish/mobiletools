---
layout: needle-guide
title: "Command Reference"
description: 
categories:
  - needle
  - usage
tags:
---

## Command Reference

| Command                               | Description           |
| -------------                         |:-------------|
| `<cmd>`                               | Execute a command on the local workstation
| `<push\pull> <src> <dst>`             | Push/pull files on the device
| `exec_command <cmd>`                  | Execute a single command on the remote device
| `exit`                                | Terminate the needle session
| `help`                                | Display help about a particular command or module
| `jobs`                                | List running background jobs
| `kill <num>`                          | Kill the specified background job
| `run`                                 | Executes a module
| `search <query>`                      | Search available modules that matches the query
| `set`                                 | Store a value in a variable
| `shell`                               | Start an interactive shell on the device
| `show [options\source\info\globals]`  | Show details of a particular module, once selected 
| `show modules`                        | Show a list of all needle modules that can be executed
| `unset`                               | Remove a named variable
| `use <module_name>`                   | Load the specified module
