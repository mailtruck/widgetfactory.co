---
title: "Conntect to a Server Running in a Docker Container  From Host OS X (macOS)"
date: 2017-07-02T05:14:31Z
---

The other day, in the spirit of a docker noob, I asked my colleage how to connect to a server that is running inside a container from my mac.

He said just add *net="host"* to the docker run command, and I realized I probably should have tried to google the answer first.

Simple right? Not so fast!

It turns out, on macOS (OO X), docker's *--net="host"* doesn't work.

This probably has something to with docker for mac [uses the OS X hypervisor kernel something something virtualmachine]("https://stackoverflow.com/questions/43383276/how-does-docker-run-a-linux-kernel-under-macos-host").

To access a service on the host os you need to alias an unused ip to loopback.

```
sudo ifconfig lo0 alias 123.123.123.123
```

Make sure that the service on the host is listening on *0.0.0.0:port* because localhost and *127.0.0.1* wont work.

Inside the docker container, the aliased ip will then forward to 0.0.0.0 on the host.

Put this in a startup script because it will reset on reboot.
