---
title: "Starting a Process"
weight: 10
---

### Starting a Process
To start a process, use the `rhadmin start <process name>` command. The first column of the output from the `status` command gives the process name.  To start the Domain Manager, issue the following command:

```sh
rhadmin start REDHAWK_DEV:REDHAWK_DEV_mgr
```
It should output:
```
REDHAWK_DEV:REDHAWK_DEV_mgr: started
```

The status should show:
```
REDHAWK_DEV:GppNode              STOPPED   May 11 11:31 AM
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 19302, uptime 0:00:16
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```

### Starting the Entire Domain
To start a domain, use the `rhadmin start <domain name>` command. To start the entire `REDHAWK_DEV` domain, issue the following command:
```sh
rhadmin start REDHAWK_DEV
```
It should output:
```
REDHAWK_DEV:GppNode: started
REDHAWK_DEV:Wave: started
```

The status should show:
```
REDHAWK_DEV:GppNode              RUNNING   pid 19600, uptime 0:00:39
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 19302, uptime 0:01:00
REDHAWK_DEV:Wave                 RUNNING   pid 19676, uptime 0:00:33
```

### Starting Part of the Domain
The stop command supports the `type` option (`domain`, `nodes`, `waveforms`). To start just the Domain Manager, issue the following command:
```sh
rhadmin start domain REDHAWK_DEV
```

The status should show:
```
REDHAWK_DEV:GppNode              STOPPED   May 11 11:31 AM
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 19302, uptime 0:00:16
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```

You can start all Domain Managers for all configured domains by changing `REDHAWK_DEV` to `all` in the above command.
```sh
rhadmin start domain all
```
