---
title: "AdminService"
weight: 10
---

## AdminService Overview

REDHAWK AdminService manages the lifecycle of the REDHAWK core services (Domain Manager, Device Manager, and waveforms) using simple INI-style configuration files. It provides system integrators with a system startup agnostic way of defining REDHAWK domains. The REDHAWK core services startup definitions are the same regardless of whether systemd or init is used.

The REDHAWK AdminService is built on [Supervisor](<http://supervisord.org>) and uses an INI style configuration file (name=value) to define the command line arguments and execution environment for the service. The configuration files and service scripts provide enough flexibility to manage the REDHAWK core services for most use cases. For specialized cases, the REDHAWK source repository (`adminservice`, `redhawk-adminservice` branch) provides an RPM spec file, `redhawk-adminservice.spec`, and all the source scripts for integrators to build and customize their own service scripts and installation RPM.

When the AdminService starts up, it reads in all INI files in the directories defined within its configuration file. These files define the process configuration for running, statusing and querying core services. Once all configurations are read, the AdminService groups them by domain and prioritizes the domain groupings by the Domain Managers `priority` configuration value; the lowest priority domain group is started first. Inside this domain group, the `priority` value of each process configuration determines the start order, typically Domain Manager, Device Manager, and then waveform. When stopping a domain, the highest priority process is stopped first.

### Managing the REDHAWK Core Services Using the `rhadmin` Script

`rhadmin` is a script used outside of system startup/shutdown to manage the REDHAWK core services lifecycle from the command line. The `rhadmin` script connects to the AdminService through a socket and supports the following commands to manage the lifecycle of the REDHAWK core services: `start`, `stop`, `restart`, and `status`.

The `rhadmin` script can be used to generate default configuration files for the AdminService and REDHAWK core services, or a configuration file for an existing REDHAWK core service.

For more information about `rhadmin`, refer to [rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}).

### Configuring and Controlling the REDHAWK AdminService

The `/etc/redhawk/adminserviced.conf` file provides the default configuration for the AdminService and the `rhadmin` client script; these values should be sufficient for most cases. By default, the AdminService uses a Unix socket for remote control, but it has an option for a TCP socket connection. The [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}}) section describes the available configuration parameters for the AdminService.

The AdminService RPM is packaged with systemd and SysV init scripts that get installed to allow it to start and stop as a Linux system service.  

For more information about configuring and controlling the AdminService, refer to [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}}).
