---
title: "Status Only Process"
weight: 90
---

It is possible to have the AdminService show the status of an existing running process. The process to monitor needs must have an associated pid file and then the process can be configured by putting a file in `/etc/redhawk/extras.d`. For example, create the `omniEvents.ini` file with the following contents:
```
[process:omniEvents]
command=/usr/sbin/omniEvents
run_detached=True
pid_file=/var/run/omniEvents.pid
enabled=True
autostart=False
autorestart=False
```
If you restart the AdminService or run `rhadmin update`, you can then check the status with `rhadmin status`. You should see a line similar to:
```
omniEvents                       RUNNING   pid 24806, uptime 0:00:05
```

In this scenario, `omniEvents` won't be started by AdminService, but it will show a `RUNNING` or `STOPPED` status in the list.
