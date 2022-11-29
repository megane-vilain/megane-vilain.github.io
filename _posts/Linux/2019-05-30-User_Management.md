---
layout: post
title : User Management
author: Mégane Vilain
category: Linux
---

## Group management
### Create a groupe
```
groupadd groupeName
```

### View the groups a user account is assigned to

```
groups username
```

### Add an user to a group

Example: wheel group

```
usermod -aG wheel user
```

### Change a user’s primary group
While a user account can be part of multiple groups, one of the groups is always the “primary group” and the others are “secondary groups”. The user’s login process and files and folders the user creates will be assigned to the primary group.

```
usermod -g groupname username
```
