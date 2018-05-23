---
title: "AdminService"
weight: 10
---

## AdminService Overview

REDHAWK AdminService manages the lifecycle of the REDHAWK core services (Domain Manager, Device Manager, and waveforms) using simple INI-style configuration files to define the execution environment of the service. It provides system integrators with a host system service agnostic way of defining REDHAWK domains. The REDHAWK core services definitions are the same regardless of whether systemd or init is used.

When the AdminService starts up, it reads all the INI files from the service configuration directories under the `/etc/redhawk` directory. These files define the configuration for controlling execution and determining status of each service.

Once all configurations are read, the AdminService logically groups the services by their DOMAIN_NAME configuration property.  After the groups are determined, the AdminService will determine the start order of each domain based on the Domain Manager's `priority` configuration value; the lowest priority domain group is started first (1 being the lowest).  With in a domain group, the AdminService will again use the `priority` value to determine the start order for all the services defined for a domain group. A typical start order priority will define Domain Manager first, followed by Device Manager(s), and finally waveform(s).

Conversely, on the host system shutdown, the AdminService with terminate the domain  and services in the domain group starting with the highest priority first

### Managing the REDHAWK Core Services Using the `rhadmin` Script

Independent of system start up and shutdown, `rhadmin` is a command line utility to manage the REDHAWK core services' lifecycle.  `rhadmin` supports the following commands to manage the service lifecycle: `start`, `stop`, `restart`, and `status` as an individual service, or groupings by service type or logical domain.  The `rhadmin` utility can also be used to generate new configuration files for all the REDHAWK core service types.

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
