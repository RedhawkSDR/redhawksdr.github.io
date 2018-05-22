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
