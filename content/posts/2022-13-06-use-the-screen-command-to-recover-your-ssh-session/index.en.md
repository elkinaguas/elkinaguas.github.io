---
layout: post
title: "Use the Screen Command to Recover your SSH Session"
date: 2022-06-13 22:17:14
description: Use the Screen Command to Recover your SSH Session
tags: [linux, ubuntu]
comments: true
---

What do you do if you lose your SSH connection to a server and you want to open the same session to continue the work your were doing? You use the command `screen`.

Screen is usually pre-installed in modern Linux distros, but if it's not you can install it using `sudo apt install screen`.

## Using screen
Start a session you don't want to lose:
```bash
screen -S session_name
```

Now you have a session where you can work and come back if your connecton is lost.

## Coming Back to Your Lost Session
Connect to your server and check the list of screen sessions:
```bash
screen -ls
```
Connect to your session using its ID number that you can find next to the session name:
```bash
screen -r session_ID_number
```

## Killing a Screen Session
while in the session press:
```bash
ctrl+a k
```

