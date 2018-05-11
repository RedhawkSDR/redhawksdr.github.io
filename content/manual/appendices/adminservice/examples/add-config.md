---
title: "Add a New Configuration"
weight: 30
---

To add a new waveform to be controlled by a running AdminService, start by creating the new waveform INI file:

### Waveform Configuration
File: `/etc/redhawk/waveforms.d/wave2.ini`
```
[waveform:Wave2]
DOMAIN_NAME=REDHAWK_DEV
WAVEFORM=Wave
INSTANCE=Second_Wave
```

The configuration file won't be picked up automatically, as seen by running the `status` command:
```
REDHAWK_DEV:GppNode              RUNNING   pid 19600, uptime 0:48:56
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 19302, uptime 0:50:46
REDHAWK_DEV:Wave                 RUNNING   pid 19676, uptime 0:48:50
```
or `avail` command:
```
Name                             In Use    Autostart Enabled   Priority
REDHAWK_DEV:GppNode              in use    auto      Enabled   100:400
REDHAWK_DEV:REDHAWK_DEV_mgr      in use    auto      Enabled   100:100
REDHAWK_DEV:Wave                 in use    auto      Enabled   100:900
```

## Reread the Configuration Files
To reread all configuration files and start the new processes, type the following:
```sh
rhadmin update REDHAWK_DEV
```

This will output:
```
REDHAWK_DEV: updated process group
```

AdminService reloaded its configuration files, started the new process for `Wave2` in the `REDHAWK_DEV` domain. If any configurations were removed(eg. deleted the `wave.ini` file), the processes corresponding to the removed configuration would be stopped. Please note that when the update occurs, the status threads get restarted, so a call to `rhadmin status` will look like everything was stopped and started. This is *not* the case, only new or removed processes are effected by this.

To verify that the `Wave2` waveform configuration was added and started, type the following:
```sh
rhadmin status
```

This will output:
```
REDHAWK_DEV:GppNode              RUNNING   pid 31316, uptime 0:00:46
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 31185, uptime 0:00:52
REDHAWK_DEV:Wave                 RUNNING   pid 31745, uptime 0:00:30
REDHAWK_DEV:Wave2                RUNNING   pid 31408, uptime 0:00:41
```
