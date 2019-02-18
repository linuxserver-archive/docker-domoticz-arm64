[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://www.domoticz.com
[hub]: https://hub.docker.com/r/lsioarmhf/domoticz-aarch64/

THIS IMAGE IS DEPRECATED. PLEASE USE THE MULTI-ARCH IMAGES AT `linuxserver/domoticz`

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/domoticz-aarch64
[![](https://images.microbadger.com/badges/version/lsioarmhf/domoticz-aarch64.svg)](https://microbadger.com/images/lsioarmhf/domoticz-aarch64 "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/domoticz-aarch64.svg)](http://microbadger.com/images/lsioarmhf/domoticz-aarch64 "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/domoticz-aarch64.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/domoticz-aarch64.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/arm64/arm64-domoticz)](https://ci.linuxserver.io/job/Docker-Builders/job/arm64/job/arm64-domoticz/)

[Domoticz][appurl] is a Home Automation System that lets you monitor and configure various devices like: Lights, Switches, various sensors/meters like Temperature, Rain, Wind, UV, Electra, Gas, Water and much more. Notifications/Alerts can be sent to any mobile device

[![domoticz](https://github.com/domoticz/domoticz/raw/master/www/images/logo.png)][appurl]

## Usage

```
docker create \
  --name=domoticz \
  --net=bridge \
  -v <path to data>:/config \
  -e PGID=<gid> -e PUID=<uid>  \
  -e TZ=<timezone> \
  -p 1443:1443 \
  -p 6144:6144 \
  -p 8080:8080 \
  --device=<path to device> \
  lsioarmhf/domoticz-aarch64
```

You can choose between using tags, latest (default, and no tag required), or a specific stable version of domoticz.

Add one of the tags, if required, to the linuxserver/domoticz line of the run/create command in the following format, linuxserver/domoticz:stable-4.9700

#### Tags

+ **stable-4.9700** : latest stable version.
+ **stable-3.8153** : no longer updated old stable version.

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 1443` - the port(s)
* `-p 6144` - the port(s)
* `-p 8080` - the port(s)
* `-v /config` - location for the config files
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `--device` - for passing through USB devices
* `-e TZ` - for timezone information *eg Europe/London, etc*

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it domoticz /bin/bash`.

### Passing Through USB Devices

To get full use of Domoticz, you probably have a USB device you want to pass through. To figure out which device to pass through, you have to connect the device and look in dmesg for the device node created. Issue the command 'dmesg | tail' after you connected your device and you should see something like below.

```
usb 1-1.2: new full-speed USB device number 7 using ehci-pci
ftdi_sio 1-1.2:1.0: FTDI USB Serial Device converter detected
usb 1-1.2: Detected FT232RL
usb 1-1.2: FTDI USB Serial Device converter now attached to ttyUSB0
```

As you can see above, the device node created is ttyUSB0. It does not say where, but it's almost always in /dev/. The correct tag for passing through this USB device is '--device=/dev/ttyUSB0'

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application
`IMPORTANT... THIS IS THE ARM64 VERSION`

To configure Domoticz, go to the IP of your docker host on the port you configured (default 8080), and add your hardware in Setup > Hardware.
The user manual is available at [www.domoticz.com][appurl]

## Info

* Shell access whilst the container is running: `docker exec -it domoticz /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f domoticz`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' domoticz`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/domoticz-aarch64`

## Versions

+ **02.07.18:** Add openssh package.
+ **01.07.18:** Fix backup/restore in webgui.
+ **03.04.18:** Add dependencies for BroadlinkRM2 plugin.
+ **20.01.18:** Move telldus core to repo to prevent build fail when source site goes down.
+ **18.01.18:** Remove logging to syslog in the run command to prevent double logging.
+ **04.01.18:** Initial Release.
