---
title: "AdminService"
weight: 55
---

## Introduction

For system integrators, REDHAWK provides an optional RPM with the necessary scripts and configuration files to manage the REDHAWK core services: Domain Manager, Device Manager (includes devices and services), and waveforms. The RPM is part of the REDHAWK yum repository, but it is not installed by default.

The AdminService groups all configured core services and waveforms by domain and then starts one domain at a time. By default, the start order, defined by the `priority` configuration setting, is Domain Manager, Device Manager, and then waveform. An inverted order is used during system shutdown.

The REDHAWK AdminService is built on [Supervisor](<http://supervisord.org>) and uses an INI style configuration file (name=value) to define the command line arguments and execution environment for the service. The configuration files and service scripts provide enough flexibility to manage the REDHAWK core services for most use cases. For specialized cases, the REDHAWK source repository (`adminservice`, `redhawk-adminservice` branch) provides an RPM spec file, `redhawk-adminservice.spec`, and all the source scripts for integrators to build and customize their own service scripts and installation RPM.

### REDHAWK AdminService System Service Scripts

The following table lists the system service scripts that are used to control the AdminService.

| **Service**            | **System Service Script**                              |
| :--------------------- | :----------------------------------------------------- |
| **CentOS 6 (SysV)**    |                                                        |
| AdminService           | `/etc/rc.d/init.d/redhawk-adminservice`                |
| **CentOS 7 (systemd)** |                                                        |
| AdminService           | `/usr/lib/systemd/system/redhawk-adminservice.service` |
| AdminService Wrapper   | `$OSSIEHOME/bin/adminserviced-start`                   |

 As per the Fedora recommendations for service unit files, the AdminService is not enabled during RPM installation. It is assumed system integrators will enable the service unit file and modify the activation to achieve desired start up and shutdown behavior for their systems.

### REDHAWK AdminService Configuration

 The following table lists the directories and configuration files used by the AdminService.

| **Service**    | **Configuration Directory**   | **File**             |
| :------------- | :---------------------------- | :------------------- |
| AdminService   | `/etc/redhawk`                | `adminserviced.conf` |
| Defaults       | `/etc/redhawk/init.d`         | `*.defaults`         |
| Domain Manager | `/etc/redhawk/domains.d`      | `*.ini`              |
| Device Manager | `/etc/redhawk/nodes.d`        | `*.ini`              |
| Waveform       | `/etc/redhawk/waveforms.d`    | `*.ini`              |

 The AdminService is controlled by a single `adminserviced.conf` file. REDHAWK core services and waveforms that are configured to start on system startup are stored in the corresponding `.d` directory. Each REDHAWK core service has a corresponding `.defaults` file in the `/etc/redhawk/init.d` directory.

 For more information about configuring the AdminService, managing the REDHAWK core services, and the linux support files that run REDHAWK core services, refer to the following sections:

 - [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}})
 - [rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}})
 - [Linux Support Files]({{< relref "manual/appendices/adminservice/support-files.md" >}})
