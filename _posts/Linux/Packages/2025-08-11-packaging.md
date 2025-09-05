---
layout: post
title : Package management
author: Mégane Vilain
category: Linux
subcategory: Packages
---

## Snaps 
Snap is a software packaging and deployment system made by Canonical for Linux. 

- **Snaps** - Packages
- **snapd** - Deamon that manages and maintains snaps

They run in a container with limited access to the host system. Using **Interfaces**, users can give an application mediated access to additional features of the host (usb devices, recording audio...)
Snap sandbow also support sharing date and unix sockets between snaps. Often used to share common libraries and aplication frameworks to reduce the size of snaps by avoiding duplication.

### Updates
Multiple time a day, snapd checks for available updates of all snaps and install them in the backgrounds.
Publishers can release and update multiple versions of their software in parallet using channels. Each channel has specific track and risk, which indicate the version and stability. By default snapd use the latest/stable channel. 

Schedule, frequency and timing of automatic updates can be configures by users, they can also pause automatic updates for a certain perdiod of time, or indefinitely. (This feature was added in november 2022)
