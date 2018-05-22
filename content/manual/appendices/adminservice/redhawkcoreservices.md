---
title: "REDHAWK Core services"
weight: 20
---
## Domain Manager Service

cfg  
lifecycle

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
The start command supports the `type` option (`domain`, `nodes`, `waveforms`). To start just the Domain Manager, issue the following command:
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


## Device Manager Service

cfg  
lifecycle

## Waveform Service

cfg  

To add a new waveform to be controlled by a running AdminService, start by creating the new waveform INI file:

### Waveform Configuration
File: `/etc/redhawk/waveforms.d/wave2.ini`
```
[waveform:Wave2]
DOMAIN_NAME=REDHAWK_DEV
WAVEFORM=Wave
INSTANCE=Second_Wave
```

lifecycle
