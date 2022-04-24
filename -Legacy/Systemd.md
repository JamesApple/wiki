# SystemD

Init replacement.

Lennart Poettering (Redhat) and Kay Sievers(Novell)

## Definitions / Glossary

|Term|Definition|
|---|---|
|MachineCTL|Built in container management|
|Timer|SystemD's equivalent of a CRON/At job|
|Unit|Everything in SystemD is a unit. Slices, timers, targets, and services are all examples.|
|Target|Target state for a machine defined in a .target unit file|
|Autofs|Deferred filesystem that dramatically improves boot speed by only mounting filesystems when they're needed and asynchronously|
|cgroup|Control Groups. Groups of processes that have resource constraints|
|Scope|Sets of pocesses that are started by other processes and they register with systemd for management.  Systemd does not directly start these processes|
|Slice|Groups of resource constrained processes are called slices|
|Socket|(?)A publish/subscribe message system to communicate with other services. These are managed by SystemD and may be closed if they don't appear to be required|


## Applications

### So

### Slices

Can be viewed at `/sys/fs/cgroup/systemd`

* system.slice
    * tmp.mount
    * crond.service
    * httpd.service
    * ...
* user.slice
    * jamesapple.slice

### CGroups

```sh
# All control groups as tree
systemd-cgls 
# Control group resource use
systemd-cgtop
```


### JournalCTL

`man 5 journald.conf`

JournalCTL records

* Kernel logs
* systemlogs (syslog)
* system services
* audit records for SELinux

JournalCTL config file: `/etc/systemd/journald.conf`

```sh
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

### Misc tools

All SystemD commands can be run against a remote server by specifying
`<command> -h host:port`

```sh
# Describe boot times
systemd-analyze
# Describe timezone
timedatectl
timedatectl set-timezone "America/New_York"
# Display hostname and location
hostnamectl
# Prevent sleeping
systemd-inhibit wget https://site.com/bigfile.zip
```

### Units

Drop in folders are located in the override directory with a suffix of `.d`
with the name of the service being overridden.
`/etc/systemd/system/udevmon.service.d/my-change.conf`


```sh
systemctl list-unit-files
systemctl edit --full docker
systemctl edit docker
# All patches / overrides
systemd-delta
# Reload unit files
systemd-reload
# Show final version of a service
systemctl cat docker.service
# Show all units
systemctl list-unit-files
```

|Dir|Use|
|---|---|
|`/usr/lib/systemd/system`|System and package install location|
|`/etc/systemd/system`|Override folder|
|`/run/systend/system`|Runtime folder. Cannot be edited|



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

### Services

```sh
man 5 systemd.service
systemctl list-unites -t service
systemctl enable docker.service
systemctl restart docker.service
systemctl status docker.service
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

### Targets

```sh
man 5 systemd.target
man 7 systemd.special
```

Allows coordination between services to achieve a state.

Includes machine states as well as special states such as bluetooth becoming enabled

```sh
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

```systemd
# /usr/lib/systemd/system/graphical.target
[Unit]
Description=Graphical Interface
Documentation=man:systemd.special(7)
Requires=multi-user.target
Wants=display-manager.service
Conflicts=rescue.service rescue.target
After=multi-user.target rescue.service rescue.target display-manager.service
AllowIsolate=yes
                      
```


### Timers

`man 5 systemd.timer`

Timers _can_ replace cron/at jobs.

Timers can be configured to wake the computer from being suspended.


#### Monotonic Timer Types

Monotonic timers are paused when the computer is paused / down.

|setting|meaning|
|------|-------|
|OnActiveSec|Timer relative to the moment the timer is activated|
|OnBootSec|Timer relative to when the machine booted|
|OnStartupSec|Timer relative to when the service manager started|
|OnUnitActiveSec|Defines a timer relative to when the unit this timer is activating was last activated|
|OnUnitInactiveSec|Defines a timer relative to when the timer unit is activating was last deactivated|

#### Realtime Timer Types

|setting|meaning|
|---|---|
|OnCalendar|Defines wallclock timers with calendar event expressions|
|AccuracySec|Defaults to 60. Defines how much leeway the calendar timer has|
```sh
# List timers
systemctl list-timers --all
# Arbitrary run
systemd-run --on-boot=10 /bin/bash -c 'ls'
```

#### Built in Tmpfile clean job
`/usr/lib/systemd/system/systemd-tmpfiles-clean.service`
```systemd
[Unit]
Description=Cleanup of Temporary Directories
Documentation=man:tmpfiles.d(5) man:systemd-tmpfiles(8)
DefaultDependencies=no
Conflicts=shutdown.target
After=local-fs.target time-set.target
Before=shutdown.target

[Service]
Type=oneshot
ExecStart=/usr/bin/systemd-tmpfiles --clean
SuccessExitStatus=DATAERR
IOSchedulingClass=idle
```

`/usr/lib/systemd/system/systemd-tmpfiles-clean.timer`
```systemd
[Unit]
Description=Daily Cleanup of Temporary Directories
Documentation=man:tmpfiles.d(5) man:systemd-tmpfiles(8)

[Timer]
OnBootSec=15min
OnUnitActiveSec=1d
Unit=systemd-tmpfiles-clean.service
```

### MachineCTL Containers

 Manage containers built into SystemD. Originally designed for the development
 of SystemD. Less overhead than traditional container tools (docker)

```sh
# OS Tree must be in /var/lib/machines/<container-name>
systemd-nspawn -M <container-name>
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


## Components

```
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
```
