---
title: "AdminService"
weight: 10
---

The REDHAWK AdminService manages the lifecycle of the REDHAWK core services (Domain Manager, Device Manager, and waveforms) using simple INI-style configuration files to define the execution environment of each core service. It provides a host system service agnostic way of defining REDHAWK domains as a service. The REDHAWK core services definitions are the same regardless of whether `systemd` or `init` control service is used. The AdminService itself follows the normal Linux system service lifecycle and is controlled using the operating system's service control.

## Installing the AdminService

```sh
yum install redhawk-adminservice
```

## Startup Process

At system startup, the AdminService performs the following sequence of actions:

    1. Process all the INI files in the service configuration directories under the `/etc/redhawk` directory.
    2. Create domain groups using the `DOMAIN_NAME` configuration property from each service configuration file.
    3. Determine the start order of each domain group using the `priority` configuration property of each groups Domain Manager service.
    4. Determine the start order of each core service within a domain group using the `priority`. The typical start order will define Domain Manager first, followed by DeviceManager(s), and finally waveforms.
    5. Launch each core service for a domain group using the configuration defintion and perform an initial status check of the service.
    6. Repeat the above step for all remaining domain groups.

Conversely, on the host system shutdown, the AdminService with terminate the domain and services in the domain group starting with the highest priority first

### Managing the REDHAWK Core Services Using the `rhadmin` Script

Independent of system start up and shutdown, `rhadmin` is a command line utility to manage the REDHAWK core services' lifecycle. `rhadmin` supports the following commands to manage the service lifecycle: `start`, `stop`, `restart`, and `status` as an individual service, or groupings by service type or logical domain. The `rhadmin` utility can also be used to generate new configuration files for all the REDHAWK core service types.

For more information about `rhadmin`, refer to [rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}).

###  REDHAWK AdminService Lifecycle

The AdminService is configured to startup and shutdown using the operating system's service control. The following table lists the system service scripts that are used to control the AdminService lifecycle.

| **Service**            | **System Service Script**                              |
| :--------------------- | :----------------------------------------------------- |
| **CentOS 6 (SysV)**    |                                                        |
| AdminService           | `/etc/rc.d/init.d/redhawk-adminservice`                |
| **CentOS 7 (systemd)** |                                                        |
| AdminService           | `/usr/lib/systemd/system/redhawk-adminservice.service` |
| AdminService Wrapper   | `$OSSIEHOME/bin/adminserviced-start`                   |

As per the Fedora recommendations for service unit files, the AdminService is not enabled during RPM installation. System integrators may enable the service unit file and modify the activation to achieve desired start up and shutdown behavior for their systems.
