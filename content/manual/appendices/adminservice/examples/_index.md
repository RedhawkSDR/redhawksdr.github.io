---
title: "Examples"
weight: 60
---

## Setup
These examples assume a default REDHAWK and AdminService install and are based on the following configuration:

| Type     | Name        | Quantity |
| :------- | :---------- | :------- |
| Domain   | REDHAWK_DEV | 1        |
| Node     | GppNode     | 1        |
| Waveform | Wave        | 1        |

There must be a Device Manager named `GppNode` and a waveform named `Wave` installed in `$SDRROOT`.

### Domain Manager Configuration
File: `/etc/redhawk/domains.d/domain.ini`
```
[domain:REDHAWK_DEV_mgr]
DOMAIN_NAME=REDHAWK_DEV
```

### Device Manager Configuration
File: `/etc/redhawk/nodes.d/node.ini`
```
[node:GppNode]
DOMAIN_NAME=REDHAWK_DEV
NODE_NAME=GppNode
```

### Waveform Configuration
File: `/etc/redhawk/waveforms.d/wave.ini`
```
[waveform:Wave]
DOMAIN_NAME=REDHAWK_DEV
WAVEFORM=Wave
```

## What's Configured
To find out what has been configured, type the following:
```sh
rhadmin avail
```

This will output:
```
Name                             In Use    Autostart Enabled   Priority
REDHAWK_DEV:GppNode              in use    auto      Enabled   100:400
REDHAWK_DEV:REDHAWK_DEV_mgr      in use    auto      Enabled   100:100
REDHAWK_DEV:Wave                 in use    auto      Enabled   100:900
```

| Column    | Description  |
| :-------- | :----------- |
| Name      | The `process name` that can be used for other commands. It's in the format of `<Domain>:<Configuration Name>`. |
| In Use    | The status of the process configuration, whether or not its configuration has been activated and operable. |
| Autostart | Whether or not the process will be automatically started when the AdminService starts. |
| Enabled   | Whether or not the process configuration can be started. This can be overridden on the start command with the `-f` flag |
| Priority  | The priority of the process' configuration in the format `<domain priority>:<process priority>`. In a multiple domain scenario, the lowest value for the `domain priority` will be started first.  When starting the domain itself, the lowest value for `process priority` will be started first.

## What's Running
The `status` command describes the state of all active configurations. To get the status, type the following:
```sh
rhadmin status
```

This will output:
```
REDHAWK_DEV:GppNode              RUNNING   pid 17582, uptime 0:00:21
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 17492, uptime 0:00:27
REDHAWK_DEV:Wave                 RUNNING   pid 17656, uptime 0:00:15
```

### Status a Specific Type
The `status` command also accepts a `type` option on the command line. To get the status of all waveforms, type the following:
```sh
rhadmin status waveforms all
```

This will output:
```
REDHAWK_DEV:Wave                 RUNNING   pid 17656, uptime 0:00:20
```

If there were multiple domains running, `all` can be replaced with the domain name to restrict the status to just that domain. 
