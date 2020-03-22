---
layout: post
title : User Management
author: Mégane Vilain
category: Linux
---

## Add an user to the wheels group


Check in `/etc/sudoers` if wheels group has the right permissions.

```
usermod -aG wheel user
su - user
```