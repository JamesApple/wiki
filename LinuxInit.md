# Init

## Definitions

init - Initialization
rc - Run Commands
sysinit - System Initialization

## Init startup process

Based off of `System V` unix startup.

Requires scripts to startup in sequential order.

Bootup process

Run level that init is invoked with(?)
0 - Halt
1 - Single user
2 - Multi user (no network)
3 - Multi user ( networking )
4 - unused. Custom runlevel env?
5 - GUI desktop wit networking
6 - Reboot

-> Bootdisk found by bootloader
-> Kernel and ram disk loaded
-> Drivers and setups pulled from ram disk
-> Kernel hands to `/sbin/init`
-> `init` reads config from `/etc/inittab`
-> `init` runs maintenance tasks from `/etc/rc.d/rc.sysinit`
-> `init` starts runlevel 3 and sequentially runs commands in `/etc/rc.d/rc3.d`



*/etc/inittab*
```init
# <identiffier>:<runlevel>:<action>:<process>

id:3:initdefault

si::sysinit:/etc/rc.d/rc.sysinit

10:0:wait:/etc/rc.d/rc 0
11:1:wait:/etc/rc.d/rc 1
12:2:wait:/etc/rc.d/rc 2
13:3:wait:/etc/rc.d/rc 3
14:4:wait:/etc/rc.d/rc 4
15:3:wait:/etc/rc.d/rc 3
16:6:wait:/etc/rc.d/rc 6
```

## Init tools

```sh
# Outputs the current runlevel
runlevel

# Set and query runlevel settings for services.
# This manages the symlinks to the services within /etc/rc.d/rc*.d
chkconfig --list
chkconfig --level 3 bluetooth off
chkconfig --level 3 bluetooth on

# Directly control services
service bluetooth stop
service bluetooth status
service bluetooth start

# Standard gui for managing the current runlevels services
ntsysv 
```

# Upstart

Upstart offers async starting of services to decrease boot times. Replaces
init. Now no longer used (typically)

Works off of real-time events which init cannot do


Init boot sequence

1. `/sbin/init`
2. Fire startup(7) event 
```
    ->. Fire mountall(8)
    ->. Load `/etc/rc-sysinit.conf` 
        -> Fire telinit(8) 
        -> Fire runlevel(7)
    -> /etc/init/rc.conf
```

Init is static. It does not respond to changes to a system.
Upstart can respond to changes.
A change on a linux system is an event


Events trigger jobs

Jobs have two categories

* Tasks
* Services

Job State
```
 post-stop
 ^      |
killed  |
 ^      V
 |  waiting --
 |            V
stopping    starting
 ^            |
 -- running <-
```


# SystemD

Lennart Poettering and Kay Sievers. Redhat and Novell

Personal project

March 2010

## Why not use upstart

Upstart was still incredibly sequential

Potentially any service that could start would start whether it was needed or not

## Inspired by launchd

Listens for a socket so that each daemon doesn't have to

Sockets?

Init services require socket from another daemon that they depend on before it can start

Launchd makes all sockets available at once instead of waiting for each to start sequentially

There are fewer direct dependencies between daemons due to sockets being created at once

## Sockets

All sockets for services are created at once so that they can all be enabled at
once. A bit like a publish subscribe messaging system.

```
----------------
|   SystemD    |
----------------
  O          O
syslog     avahi
```

SystemD manages closing sockets if no messages are sent between processes. If
new messages are received for a closed socket then they will be reopened and
queued by SystemD.


## Advantages

Faster boot
Less interdependency of services through sockets
Improved robustness
Lower overhead (occasionally)
Imroved troubleshooting logging system (journald).

## Components

## Utilities

* systemctl
* journalctl
* analyze
* cgls
* cgtop
* loginctl
* nspawn

## Daemons

* systemd
* journald
* networkd
* logind
* user session

## Targets

* bootmode
* basic
* shutdown
* reboot

* Multi user
    * dbus
    * telephony
    * dlog
    * logind
* graphical
    * user-session
* user-session
    * display-service
    * tizen service


## Core

* manager
* systemd

* Unit
    * service
    * timer
    * mount
    * target
    * automount
    * scope
    * snapshot
    * path
    * socket
    * swap device
    * slice

* Login
    * multiseat
    * inhibit
    * session
    * pam

* namespace
* log
* cgroup
* dbus

## Libraries

* dbus-1
* lippam
* libcap
* libcryptsetup
* tcpwrapper
* libaudit
* libnotify

## Kernel

* cgroups
* autofs
* kdbus


## Process management

All processes are nmow managed in control groups (cgroups)

Kernel feature

Collections of services that are grouped together in ahierarchal manner
each cgroup can have its resources limited such as RAM CPU etc

Groups of resource constrained processes are called slices

/sys/fs/cgroup/systemd

Slices are a partitioning group for services

system.slice
    * tmp.mount
    * crond.service
    * httpd.service
    * ...
user.slice
    * jamesapple.slice

Scopes are sets of pocesses that are started by other processes and they register with systemd for management

Systemd does not directly start these processes

Once these processes are launched they register themselves with systemd

Scopes contain runtime parameters, not execution parameters

Created by the processes themselves and are dynamic

A system admin can not create a scope since they are generated at runtime

EG PID for scopes

```
systemd-cgls 
## OUTPUT
Control group /:
Control group /:
-.slice
  user.slice
    user-1000.slice
     =user@1000.service
        pulseaudio.service
          1326 /usr/bin/pulseaudio --daemonize=no
          1371 /usr/lib/pulse/gsettings-helper
        init.scope
          863 /usr/lib/systemd/systemd --user
          864 (sd-pam)
        at-spi-dbus-bus.service
          1302 /usr/lib/at-spi-bus-launcher
          1308 /usr/bin/dbus-daemon --config-file=/usr/share/defaults/at-spi2/accessibility.c>
        dbus.service
          1275 /usr/bin/dbus-daemon --session --address=systemd: --nofork --nopidfile --syste>
      session-1.scope
          563 login -- jamesapple
          869 -zsh
         1261 sway
...
```

```
systemd-cgtop
# --- Outputs resource usage by control group
```


## Autofs

`/etc/fstab`

Traditional init would bring up each file system one at a time and a fsck
(filesystemcheck) would be run on each one.

All filesystems have to be mounted before boot occurs



SystemD uses autofs to defer loading a filesystem until a service requires it.

Once the autofs service is no longer waiting to be mounted it unmounts itself

Allows multiple FS to be brought up in parralel

`man 5 system.resource-countrol`

## SystemD Downsides


Binary logfiles

Perception of feature creep

## Unit files

Everything in SystemD is a unit

A unit is denoted as something.unit

EG:
```
    session-3.scope
    user.slice
    dbus.scope
```

```sh
# Loaded: .*  disabled; vendor preset: disabled
# Shows documentation links
systemctl status docker
vim /usr/lib/systemd/system/docker.service

# Show documentation(where supported) such as man pages
systemctl help docker

# Execute remotely
systemctl status docker -h myserver:8080


# Creates a symlink
systemctl enable docker

# Prints a tree of statuses
systemctl status

# Prints all units on the system
systemctl
```

## Systemd Journal

* Kernel logs
* systemlogs (syslog)
* system services
* audit records for SELinux


```sh
vim /etc/systemd/journald.conf
man 5 journald.conf
# Create a systemd journal that will persist
mkdir -p /var/log/journal
systemd-tmpfiles --create --prefix /var/log/journal
```

```sh
# Oldest first
journalctl 
journalctl -r 
journalctl -n 10

# Tails journal
journalctl -f 

# Docker unit only
journalctl -u docker.service

# formats output to json
journalctl -o  json-pretty

# Send command output to journal
echo "Hi journalctl" | systemd-cat

# Filter to log lines with text
journalctl -x mylog:w

# equivalent of dmesg. Kernel messages
journalctl -k

# Output since boot
journalctl --list-boot
journalctl -b <boot-number>

journalctl --since "yesterday"
journalctl --until "now"
journalctl --until "2018-02-009 12:12:00"

journalctl --disk-usage
journalctl --rotate
```

## Misc tools

```sh
# Describe boot times
systemd-analyze

timedatectl
timedatectl set-timezone "America/New_York"

# Display hostname and location
hostnamectl

# Prevent sleeping
systemd-inhibit wget https://site.com/bigfile.zip
```

## Unit files

Init used to rely on bash shell scripts. This is slow relative to the speed of
booting directly with compiled C code.

Instead of bash init files systemd uses unit files

In this precedence

Provided by package installations: 
`/usr/lib/systemd/system`

Sys admin unit files
`/etc/systemd/system`

Runtime unit files
`/run/systend/system`

`systemctl list-unit-files`

Use `systemctl edit --full docker` to create the required unit file and directory.
```systemd
# man 5 systemd.unit
[Unit]
Description=OpenSSH Daemon
Documentation=man:systemd.special(7)
Requires=basic.target
# Does not have to be started
Wants=network.target
Conflicts=rescue.service rescue.target
after=basic.target
```

Drop in files are located in the `.d` folder and are applied like patches to
the overridden systemd unit.
EG:
`/etc/systemd/system/udevmon.service.d/my-change.conf`

Use 
`systemd-delta` to view the overridden and extended unit files
You must run `systemd-reload` to reload the unit files and apply changes

Use `systemctl cat docker.service` to see the final output of all changes

`systemctl list-unit-files` shows down units and all units

Unit files use an INI Format


## Target units

Describe a state of the system

Often used to bring a computer to a new state

```sh
man 5 systemd.target
man 7 systemd.special

multi-user.target # multi user (runlevel 3)
graphical.target # multi-user sstem with desktop (runlevel 5)
rescue.target # Rescue shell with basic system
basic.target # Utilized before boot
sysint.target # System initialization

systemctl cat graphical.target

systemctl list-units -t target

# Default target
systemctl get-default 

# Change default target
systemctl set-default

# Change from the current state to another state
systemctl isolate multiuser.target

systemctl reboot
systemctl poweroff

# Allows root to repair system
systemctl rescue
```

Targets sync other units when the computer boots or changes states

Often used to bring the system to a new state

Slices and scopes associate with targets

## Service unit file

```sh
man 5 systemd.service
systemctl list-unites -t service
systemctl enable docker.service
systemctl restart docker.service
systemctl reload docker.service

# ALWAYS prevents startup by linking unit file to /dev/null ?
systemctl mask docker.service
# Reverse the previous
systemctl unmask docker.service
```

```systemd
[Service]
# simple - started process is the main service 
# forking - will fork into child processes and exit Add PIDFile= option so sytemd can track parent
# oneshot - Process must exit before systemd will start following units
# dbus - indiates the service will have a name for it on the d-bus bus specified by BusName=
# notify - send a notify signal to systemd after it has started to start follow ups
# idle - delayed startup. ~5 seconds
Type=simple
ExecStart=
[Install]
WantedBy=
```

## Timer unit

man 5 systemd.timer
man 5 systemd.time

*docker.timer* must have a *docker.service*

## Timer types

Monotonic - OnBootSec=, OnActiveSec
Realtime - OnCalendar=

Used instead of cron/at

Transient timers:
`systemd-run`

```sh
systemctl list-timers --all
```

## Simple timer

`script-run.service`
```systemd
[Unit]
Description=Run the script
ExecStart=/bin/script.sh
```

`script-run.timer`
```systemd
[Unit]
Description=Fire off script

[Timer]
OnCalendar=*-*-* 21:06:00
Persistent=true
Unit=script-run.service

[Install]
WantedBy=multi-user.target
```

## SystemD Containers

No need for multiple file systems, embedded bio etc

`systemd-nspawn` 

manages cntainers built into systemd

Originally developed for the creation of systemd

Has access to standard debug tooling

## Building

Place an OS tree in `/var/lib/machines/<container-name>`

start with `systemd-nspawn -M <container-name>`

```sh
# Start / background a container
machinectl enable testing
machinectl start testing

machinectl pull-raw --verify=checksum https://example.com/example-os.raw.gz

systemctl enable systemd-nspawn@<container-name>

systemd-nspawn -D /var/lib/machines/<container-name>

machinectl list <container-id>
machinectl login <container-id>
machinectl status <container-id>
machinectl reboot <container-id>
machinectl poweroff <container-id>
machinectl start <container-id>
machinectl enable <container-id>
machinectl remove <container-id>
```
