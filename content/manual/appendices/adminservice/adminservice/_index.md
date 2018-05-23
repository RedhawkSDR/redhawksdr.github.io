---
title: "AdminService"
weight: 10
---

## AdminService Overview

REDHAWK AdminService manages the lifecycle of the REDHAWK core services (Domain Manager, Device Manager, and waveforms) using simple INI-style configuration files. It provides system integrators with a ...

The REDHAWK AdminService is built on [Supervisor](<http://supervisord.org>) and uses an INI style configuration file (name=value) to define the command line arguments and execution environment for the service. The configuration files and service scripts provide enough flexibility to manage the REDHAWK core services for most use cases. For specialized cases, the REDHAWK source repository (`adminservice`, `redhawk-adminservice` branch) provides an RPM spec file, `redhawk-adminservice.spec`, and all the source scripts for integrators to build and customize their own service scripts and installation RPM.

use of Service Configurations -- configure and control stop and start of REDHAWK core services

The AdminService groups all configured core services and waveforms by domain and then starts one domain at a time. By default, the start order, defined by the `priority` configuration setting, is Domain Manager, Device Manager, and then waveform. An inverted order is used during system shutdown.

### Managing the REDHAWK Core Services Using the `rhadmin` Script

`rhadmin` is a script used outside of system startup/shutdown to manage the REDHAWK core services lifecycle from the command line. The `rhadmin` script connects to the AdminService through a socket and supports the following commands to manage the lifecycle of the REDHAWK core services: `start`, `stop`, `restart`, and `status`.

   -- add something about generating configuration files for each service

For more information about `rhadmin`, refer to [rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}).

### Configuring and Controlling the REDHAWK AdminService

The `/etc/redhawk/adminserviced.conf` file provides the default configuration for the AdminService and the `rhadmin` client script; these values should be sufficient for most cases. By default, the AdminService uses a Unix socket for remote control, but it has an option for a TCP socket connection. The [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}}) section describes the available configuration parameters for the AdminService.

The AdminService is started and stopped as a Linux system service.  

For more information about configuring and controlling the AdminService, refer to [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}}).
