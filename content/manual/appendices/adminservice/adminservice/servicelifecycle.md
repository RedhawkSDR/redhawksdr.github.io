---
title: "Service Life Cycle"
weight: 60
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

### Stopping a Process
To stop a process, use the `rhadmin stop <process name>` command. The first column of the output from the `status` command gives the process name.  To stop the `Wave` waveform, issue the following command:

```sh
rhadmin stop REDHAWK_DEV:Wave
```
It should output:
```
REDHAWK_DEV:Wave: stopped
```

The status should show:
```
REDHAWK_DEV:GppNode              RUNNING   pid 17582, uptime 0:58:01
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 17492, uptime 0:58:07
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```

### Viewing the Status of a Process

#### REDHAWK Services

The `status` command describes the state of all active configurations. To view the status, enter the following:
```sh
rhadmin status
```

The following output is displayed:
```
REDHAWK_DEV:GppNode              RUNNING   pid 17582, uptime 0:00:21
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 17492, uptime 0:00:27
REDHAWK_DEV:Wave                 RUNNING   pid 17656, uptime 0:00:15
```

### Status a Specific Type
The `status` command also accepts a `type` option on the command line. To view the status of all waveforms, enter the following:
```sh
rhadmin status waveforms all
```

The follwing output is displayed:
```
REDHAWK_DEV:Wave                 RUNNING   pid 17656, uptime 0:00:20
```

If multiple domains are running, `all` can be replaced with the domain name to restrict the status to the specified domain.


#### Non-REDHAWK Services
   Explain how to status non-redhawk services.

### Restart


### Optional  
