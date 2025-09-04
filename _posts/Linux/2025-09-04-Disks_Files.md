---
layout: post
title : Files and disks
author: Mégane Vilain
category: Linux
---

## Files 

### Ls 
List files and directories within the file system 

|Option|Description|
|---|---|
|***-l***| Print files in a vertical listing format -
 |***-a***|Show hidden files|
 |***-R***| Display the contents of the subdirectories recursively|

## Disks 

### lsblk

List all disks with their partitions and mount points
```bash
$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0  74.5G  0 disk
├─sda1   8:1    0  37.3G  0 part /
├─sda2   8:2    0   9.3G  0 part [SWAP]
└─sda3   8:3    0    28G  0 part /apps
```
