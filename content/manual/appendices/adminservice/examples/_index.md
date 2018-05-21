---
title: "Examples"
weight: 60
---

## Setup
The following examples assume a default REDHAWK and AdminService install and are based on the following configuration:

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

## Viewing What Is Configured in the AdminService
To view what services are currently configured in the AdminService, enter the following command:
```sh
rhadmin avail
```

The configured Domain Manager, Device Manager, and waveform services are displayed. The following output is displayed:
```
Name                             In Use    Autostart Enabled   Priority
REDHAWK_DEV:GppNode              in use    auto      Enabled   100:400
REDHAWK_DEV:REDHAWK_DEV_mgr      in use    auto      Enabled   100:100
REDHAWK_DEV:Wave                 in use    auto      Enabled   100:900
```
The following table describes the information displayed for the configured services.

| Column    | Description  |
| :-------- | :----------- |
| Name      | The `process name` that may be used for other commands. The format used is `<Domain>:<Configuration Name>`. |
| In Use    | The status of the process configuration, which indicates whether the configuration has been activated and is operable. |
| Autostart | Specifies whether the process will automatically start when the AdminService starts. |
| Enabled   | Specifies whether the process configuration can be started. This setting may be overridden on the start command with the `-f` flag. |
| Priority  | The priority of the process' configuration. The format used is `<domain priority>:<process priority>`. In a multiple domain scenario, the lowest value for the `domain priority` is started first.  When starting the domain itself, the lowest value for `process priority` is started first.

## What's Running
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
