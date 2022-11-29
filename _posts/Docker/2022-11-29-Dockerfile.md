---
layout: post
title : Dockerfile
author: Mégane Vilain
category: Docker
---

# Dockerfile

Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. This page describes the commands you can use in a Dockerfile.

## Instructions 

### FROM

The FROM instruction initializes a new build stage and sets the Base Image for subsequent instructions. As such, a valid Dockerfile must start with a FROM instruction. 

```
FROM [--platform=<platform>] <image> [AS <name>]
```

### ENV
Sets the environment variable <key> to the value <value>. This value will be in the environment for all subsequent instructions in the build stage and can be replaced inline in many as well. The value will be interpreted for other environment variables, so quote characters will be removed if they are not escaped. Like command line parsing, quotes and backslashes can be used to include spaces within values.

```
ENV abc=hello
```

### ARG
Provides values for the image building process.
FROM instructions support variables that are declared by any ARG instructions that occur before the first FROM.

```
ARG  CODE_VERSION=latest
FROM base:${CODE_VERSION}
CMD  /code/run-app

FROM extras:${CODE_VERSION}
CMD  /code/run-extras
```

An ARG declared before a FROM is outside of a build stage, so it can’t be used in any instruction after a FROM. To use the default value of an ARG declared before the first FROM use an ARG instruction without a value inside of a build stage:

```
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```
  
Predefined ARGs

Docker has a set of predefined ARG variables that you can use without a corresponding ARG instruction in the Dockerfile.

- HTTP_PROXY
- http_proxy
- HTTPS_PROXY
- https_proxy
- FTP_PROXY
- ftp_proxy
- NO_PROXY
- no_proxy
- ALL_PROXY
- all_proxy

To use these, pass them on the command line using the --build-arg flag, for example:
```
 docker build --build-arg HTTPS_PROXY=https://my-proxy.example.com .
```

### RUN 
The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.

```
RUN echo $VERSION > image_version
```

### CMD
The main purpose of a CMD is to provide defaults for an executing container. 
There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect.

```
CMD ["executable","param1","param2"]
CMD echo "This is a test." | wc -
```

### EXPOSE

The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. You can specify whether the port listens on TCP or UDP, and the default is TCP if the protocol is not specified.
The EXPOSE instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container, about which ports are intended to be published. 

By default, EXPOSE assumes TCP. You can also specify UDP:
```
EXPOSE 80/udp
```
To expose on both TCP and UDP, include two lines:
```
EXPOSE 80/tcp
EXPOSE 80/udp
```

### HEALTHCHECK
  
The HEALTHCHECK instruction has two forms:
``` 
HEALTHCHECK [OPTIONS] CMD command (check container health by running a command inside the container)
HEALTHCHECK NONE (disable any healthcheck inherited from the base image)
```
  
The HEALTHCHECK instruction tells Docker how to test a container to check that it is still working. This can detect cases such as a web server that is stuck in an infinite loop and unable to handle new connections, even though the server process is still running.

The options that can appear before CMD are:

- --interval=DURATION (default: 30s)
- --timeout=DURATION (default: 30s)
- --start-period=DURATION (default: 0s)
- --retries=N (default: 3)
 
The command’s exit status indicates the health status of the container. The possible values are:

- 0: success - the container is healthy and ready for use
- 1: unhealthy - the container is not working correctly
- 2: reserved - do not use this exit code
  
### USER
 The USER instruction sets the user name (or UID) and optionally the user group (or GID) to use as the default user and group for the remainder of the current stage. 
 The user needs to be created beforehand
  
```
USER <user>[:<group>]

or

USER <UID>[:<GID>]

```
  
### WORKDIR
  
The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.
The WORKDIR instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction.
  
```
WORKDIR /path/to/workdir
```
  
### VOLUME
The VOLUME instruction creates a mount point with the specified name and marks it as holding externally mounted volumes from native host or other containers. The value can be a JSON array, VOLUME ["/var/log/"], or a plain string with multiple arguments, such as VOLUME /var/log or VOLUME /var/log /var/db.
The docker run command initializes the newly created volume with any data that exists at the specified location within the base image. 
```
FROM ubuntu
RUN mkdir /myvol
RUN echo "hello world" > /myvol/greeting
VOLUME /myvol  
```    

### ADD
  
ADD has two forms:

```
ADD [--chown=<user>:<group>] [--checksum=<checksum>] <src>... <dest>
ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
```
  
The latter form is required for paths containing whitespace.
The ADD instruction copies new files, directories or remote file URLs from <src> and adds them to the filesystem of the image at the path <dest>.
  
ADD obeys the following rules:

- The <src> path must be inside the context of the build; you cannot ADD ../something /something, because the first step of a docker build is to send the context directory (and subdirectories) to the docker daemon.
- If <src> is a URL and <dest> does not end with a trailing slash, then a file is downloaded from the URL and copied to <dest>.
- If <src> is a URL and <dest> does end with a trailing slash, then the filename is inferred from the URL and the file is downloaded to <dest>/<filename>. For instance, ADD http://example.com/foobar / would create the file /foobar. The URL must have a nontrivial path so that an appropriate filename can be discovered in this case (http://example.com will not work).
- If <src> is a directory, the entire contents of the directory are copied, including filesystem metadata.
- If <dest> doesn’t exist, it is created along with all missing directories in its path.
  
