---
title: "Stopping a Process"
weight: 20
---

Continuing from the start example, when we check the status we have the following output:
```
REDHAWK_DEV:GppNode              RUNNING   pid 17582, uptime 0:00:21
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 17492, uptime 0:00:27
REDHAWK_DEV:Wave                 RUNNING   pid 17656, uptime 0:00:15
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

### Stopping the Entire Domain
To stop an entire domain, use the `rhadmin stop <domain name>` command. To stop the `REDHAWK_DEV` domain, issue the following command:
```sh
rhadmin stop REDHAWK_DEV
```
It should output:
```
REDHAWK_DEV:GppNode: stopped
REDHAWK_DEV:REDHAWK_DEV_mgr: stopped
```

The status should show:
```
REDHAWK_DEV:GppNode              STOPPED   May 11 11:31 AM
REDHAWK_DEV:REDHAWK_DEV_mgr      STOPPED   May 11 11:31 AM
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```

### Stopping Part of the Domain
The stop command supports the `type` option (`domain`, `nodes`, `waveforms`). To stop all waveforms in a domain, issue the following command:
```sh
rhadmin stop waveforms REDHAWK_DEV
```

The status should show:
```
REDHAWK_DEV:GppNode              RUNNING   pid 17582, uptime 0:58:01
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 17492, uptime 0:58:07
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```

You can stop all waveforms on all configured domains by changing `REDHAWK_DEV` to `all` in the above command.
```sh
rhadmin stop waveforms all
```
