---
layout: post
title : Debian
author: Mégane Vilain
category: Linux
subcategory: Packaging
---

### dput

Upload, to the Debian package upload queue, the files constituting the package specified in each CHANGESFILE. <br>
Requires a configuration file **dput.cf** that will be created by default during insttalation of the package

```bash 
dput	[-DPUVdflosu] [-c CONFIGFILE] [-e DAYS] [HOSTNAME] CHANGESFILE
```

#### Options 

|Option|Description|
|---|---|
|-d --debug|Display debugging messages|
|-f --force|Force upload if the package was already uploaded before|
|-H --host-list|Display the list of hosts known|
|-o --check-only|Don't upload, only performs the check.|


##### Confiration dput.cf

**dput.cf** consists of different groups of configuration options, one for each host where you want to be able to upload packages. Hosts are defined using an identifier header with a short name for the host, enclosed in square brackets.

Example for mentors
```
[mentors]
fqdn = mentors.debian.net
incoming = /upload
method = https
allow_unsigned_uploads = 0
progress_indicator = 2
# Allow uploads for UNRELEASED packages
allowed_distributions = .*
```
