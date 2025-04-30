---
layout: post
title : User Management
author: Mégane Vilain
category: Linux
---

Users and groups are used on GNU/Linux for access control.
Managing user is done for the purpose of security by limiting access in certain specific ways. 

Any individual may have more than one account as long as they use a different name for each account they create. Further, there are some reserved names which may not be used such as "root".
Users may be grouped together into a "group", and users may be added to an existing group to utilize the privileged access it grants.

## Group management

|Command|Meaning|
|---|---|
|cat /etc/group| List all groups
|groupadd groupName|Create a group|
|gropdel groupName|Delete a group\
|groups username| List the groups an user account is assigned to |
|usermod -aG **additional_groups**  username| Add an user to a group.   If the -a option is omitted in the usermod command above, the user is removed from all groups not listed in additional_groups|
|usermod -g groupname username|While a user account can be part of multiple groups, one of the groups is always the “primary group” and the others are “secondary groups”. The user’s login process and files and folders the user creates will be assigned to the primary group.|


