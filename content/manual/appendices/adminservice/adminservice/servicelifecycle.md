---
title: "Service Life Cycle"
weight: 60
---

When the AdminService is started at system startup, all enabled services will be started.  This section covers the commands used to manage the lifecycle of a REDHAWK core service process after system startup has occurred.

### Getting a Service's status
To inspect the status of a REDHAWK core service use the `status` command.
```sh
rhadmin status service_name
or
rhadmin status domain_name
or
rhadmin status
or
rhadmin status [type]
```
Where optional `[type]` is `domain`, `nodes`, `waveforms`.

If `service_name` is provided, process the command against a specific service. If `domain_name` is provided, process the command against the specified domain group. If no argument is provided, process the command against all services. If the optional `[type]` is specified then restrict the command to a specific core service type.

```sh
rhadmin status
```
output the `status` from all activated services:
```
REDHAWK_DEV:GppNode              STOPPED   May 11 11:31 AM
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 19302, uptime 0:00:16
REDHAWK_DEV:Wave                 STOPPED   May 11 11:30 AM
```
The following table describes the information displayed for the configured services.

| Column    | Description  |
| :-------- | :----------- |
| 1         | The `service name` that may be used for other commands. The format used is `<domain name>:<section name>`. |
| 2         | The state of the service process: `RUNNING` or `STOPPED` |
| 3         | Process id of the actual service. Not shown in `STOPPED` state. |
| 4         | uptime is for running processes, or date time service was stopped |


### Starting a Service
To start a service use the following command:
```sh
rhadmin start service_name
or
rhadmin start domain_name
or
rhadmin start all
or
rhadmin start [type] all
```
Where optional `[type]` is `domain`, `nodes`, `waveforms`.

If `service_name` is provided, process the command against a specific service. If `domain_name` is provided, process the command against the specified domain group. If 'all' is provided, process the command against all services. If the optional `[type]` is specified then restrict the command to a specific core service type.

The following example will start the Domain Manager service `REDHAWK_DEV:REDHAWK_DEV_mgr`:

```sh
rhadmin start REDHAWK_DEV:REDHAWK_DEV_mgr
```
It should output:
```
REDHAWK_DEV:REDHAWK_DEV_mgr: started
```
### Stopping a service
To stop a service use the following command:
```sh
rhadmin stop service_name
or
rhadmin stop domain_name
or
rhadmin stop all
or
rhadmin stop [type] all
```
Where optional `[type]` is `domain`, `nodes`, `waveforms`.

If `service_name` is provided, process the command against a specific service. If `domain_name` is provided, process the command against the specified domain group. If 'all' is provided, process the command against all services. If the optional `[type]` is specified then restrict the command to a specific core service type.


The following example will stop the waveform service `REHDHAWK_DEV:Wave`:

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

### Restarting a service
To restart a service use the following command:
```sh
rhadmin restart service_name
or
rhadmin restart domain_name
or
rhadmin restart all
or
rhadmin restart [type] all
```
Where optional `[type]` is `domain`, `nodes`, `waveforms`.

If `service_name` is provided, process the command against a specific service. If `domain_name` is provided, process the command against the specified domain group. If 'all' is provided, process the command against all services. If the optional `[type]` is specified then restrict the command to a specific core service type.


The following example will restart all the services for the domain group `REHDHAWK_DEV`:

```sh
rhadmin restart REDHAWK_DEV
```
It should output:
```
REDHAWK_DEV:Wave             stopped
REDHAWK_DEV:GppNode          stopped
REDHAWK_DEV:REDHAWK_DEV_mgr  stopped
REDHAWK_DEV:REDHAWK_DEV_mgr  started
REDHAWK_DEV:GppNode          started
REDHAWK_DEV:Wave             started
```

The status should show:
```
REDHAWK_DEV:GppNode              RUNNING   pid 20124, uptime 0:00:20
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 20123, uptime 0:00:30
REDHAWK_DEV:Wave                 RUNNING   pid 20125, uptime 0:00:10
```
