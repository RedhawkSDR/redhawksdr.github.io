---
title: "AdminService"
weight: 55
---

# AdminService

## Introduction

For system integrators, REDHAWK provides an optional RPM with the necessary scripts and configuration files to manage the REDHAWK core services: Domain Manager, Device Manager (includes devices and services), and waveforms. The RPM is part of the REDHAWK Runtime yum respository, but is not installed by default.

The AdminService groups all configured core services and waveforms by domain and then starts one domain at a time. By default, the start order, defined by the `priority` configuration setting, is Domain Manager, Device Manager, and then waveform. An inverted order is used during system shutdown.

The REDHAWK AdminService is built on [Supervisor](<http://supervisord.org>) and uses an INI style configuration file (name=value) to define the command line arguments and execution environment for the service. The configuration files and service scripts provide enough flexibility to manage the REDHAWK core services for most use cases. For specialized cases, the REDHAWK source repository (`adminservice`, `redhawk-adminservice` branch) provides an RPM spec file, `redhawk-adminservice.spec`, and all the source scripts for integrators to build and customize their own service scripts and installation RPM.

The following table describes the configuration directories that contain the INI style configuration files.

##### REDHAWK AdminService Configuration Directories

| **Service**    | **Configuration Directory** |
| :------------- | :-------------------------- |
| AdminService   | /etc/redhawk                |
| Defaults       | /etc/redhawk/init.d         |
| Domain Manager | /etc/redhawk/domains.d      |
| Device Manager | /etc/redhawk/nodes.d        |
| Waveform       | /etc/redhawk/waveforms.d    |

The AdminService is controlled by a single `adminserviced.conf` file residing in `/etc/redhawk`. REDHAWK core services and waveforms that are to start on system startup are stored in the corresponding `.d` directory. Each REDHAWK core service has a corresponding `.defaults` file in the `/etc/redhawk/init.d` directory.

The Domain Manager and Device Manager configurations directly correspond to a process that runs on a system. On the other hand, the waveform configurations tell the AdminService how to interact with the running REDHAWK Domain to start and stop waveforms. The INI content for each specific service is explained in the following sections:

  - [Domain Managers]({{< relref "#domain-managers" >}})

  - [Device Managers]({{< relref "#device-managers" >}})

  - [Waveforms]({{< relref "#waveforms" >}})

### AdminService Configuration File

The `/etc/redhawk/adminserviced.conf` file provides the default configuration for the AdminService, these values should be sufficient for most cases. By default, the AdminService uses a Unix socket for remote control, but it has an option for a TCP socket connection. The [AdminService Configuration File]({{< relref "manual/appendices/adminservice/adminservice.md" >}}) section describes all the available configuration parameters for the AdminService.

To create a new AdminService configuration file and start the service, perform the following commands.  
{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```sh
cd /etc/redhawk
rhadmin config admin > myadminserviced.cfg
vi myadminserviced.cfg
adminserviced -c /etc/redhawk/myadminserviced.cfg
```

##### REDHAWK AdminService System Service Scripts

The following table lists the system service scripts that are used to control the AdminService.

| **Service**            | **System Service Script**                              |
| :--------------------- | :----------------------------------------------------- |
| **CentOS 6 (SysV)**    |                                                        |
| AdminService           | `/etc/rc.d/init.d/redhawk-adminservice`                |
| **CentOS 7 (systemd)** |                                                        |
| AdminService           | `/usr/lib/systemd/system/redhawk-adminservice.service` |
| AdminService Wrapper   | `$OSSIEHOME/bin/adminserviced-start`                   |

 As per the Fedora recommendations for service unit files, the AdminService is not enabled during RPM installation. It is assumed the system integrator will enable the service unit file and modify the activation to achieve desired start up and shutdown behavior for their systems.

### rhadmin

`rhadmin` is a script used outside of system startup/shutdown to manage REDHAWK core service lifecycle from the command line. The `rhadmin` script connects to the AdminService through a socket and supports the following commands to manage the lifecycle of the REDHAWK core services: `start`, `stop`, `restart` and `status`.

The configuration of the `rhadmin` is through the `[rhadmin]` section in the `/etc/redhawk/adminserviced.conf` file. You can use the following command to run the `rhadmin` script using a different configuration file.
```sh
rhadmin -c /etc/redhawk/myadminserviced.conf
```

##### rhadmin Commands

The following table describes the `rhadmin` client script commands that are used to control the AdminService.

| **Command** | **Argument**           | **Description**                                                                                                |
| :---------- | :--------------------- | :------------------------------------------------------------------------------------------------------------- |
| `add`       | Domain or Process      | Activates any updates to the configuration that were made by `reread`.                                         |
| `avail`     |                        | Shows Domains/Processes that can be started.                                                                   |
| `getconfig` | Process                | Displays the current configuration values for the listed process. Can specify multiple arguments.              |
| `maintail`  | `-f`, `-<bytes>`       | Displays the AdminService log. `-f` for continuous or `-<bytes>` for amount of log to retrieve.                |
| `reload`    |                        | Restart the AdminService, implicitly causes it to reread the configuration files.                              |
| `reread`    |                        | Rereads the configuration files, doesn't apply changes.                                                        |
| `restart`   | `all`, Domain, Process | Restart the specified Domain, Process or everything(`all`). Can specify multiple arguments.                    |
| `shutdown`  |                        | Stop the AdminService.                                                                                         |
| `start`     | `all`, Domain, Process | Start the specified Domain, Process or everything(`all`). Can specify multiple arguments.                      |
| `status`    | none, Domain, Process  | Shows the status for the specified Domain, Process or everything(none).                                        |
| `stop`      | `all`, Domain, Process | Stop the specified Domain, Process or everything(`all`). Can specify multiple arguments.                       |
| `update`    | none, Domain           | Reload the configuration and optionally start/stop any domain groupings that have changed. Can specify multiple arguments. |

The `rhadmin` client script can be run in interactive mode with the `-i` flag:
```sh
rhadmin -i
avail
```
or as a one time command by invoking
```sh
rhadmin avail
```

## Environment Variables

When the AdminService is started, its environment variables are stored and available for use when configuring processes. Any processes started by the AdminService are started with these values in their environment.

Environment variables may also be referenced in the configuration files by using the Python string expression syntax `%(ENV_X)s` as the value for that parameter:
```
loglevel=%(ENV_LOGLEVEL)s
```  
In the above example, the log level for the Domain Manager will be set to the environment variable `$LOGLEVEL`.  

Environment variables may be overridden by using the `environment` parameter. But, due to the way the configuration file is processed, these overrides are only available for the parameters whose names are all UPPERCASE.  

## Domain Managers

The REDHAWK Domain Manager configuration is controlled by files in the `/etc/redhawk/domains.d` directory. The `rhadmin` script can generate an example Domain Manager configuration file with the complete set of parameters to manage the execution of a REDHAWK Domain Manager service.

To create and start new Domain Manager services, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```sh
cd /etc/redhawk/domains.d

# This will generate a generic domain configuration
rhadmin config domain > domain.ini
vi domain.ini
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. A Domain Manager is defined within a section that has a `[domain:domainname]` header. By adding multiple sections like this, it is possible to define multiple Domain Manager services in one file. It is recommended that you define only one Domain Manager service per configuration file.

The [Domain Manager Configuration File]({{< relref "manual/appendices/adminservice/domainmanager.md" >}}) section describes all the available configuration parameters for the Domain Manager process.

## Device Managers

The REDHAWK Device Manager configuration is controlled by files in the `/etc/redhawk/nodes.d` directory. The `rhadmin` script can generate an example Device Manager configuration file with the complete set of parameters to manage the execution of a REDHAWK Device Manager service. There are no rules on partitioning nodes for a REDHAWK system. REDHAWK’s only guidance is that you define one GPP per computing host.

To create and start new Device Manager services, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```sh
cd /etc/redhawk/nodes.d

# This will generate a generic node configuration
rhadmin config node > node.ini

vi node.ini
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. A Device Manager is defined within a section that has a `[node:nodename]` header, each section corresponds to a DCD file describing a [Node]({{< relref "manual/nodes">}}). By adding multiple sections like this, it is possible to define multiple Device Manager services in one file. If you have multiple Nodes defined for a computing host, it is recommended that you have one configuration file per Node definition.

The Device Manager can be configured to start after the Domain Manager has started up, or it can start up at the same time as the Domain Manager and it will wait for the domain to be available and register its [Devices]({{< relref "manual/devices">}}) and [Services]({{< relref "manual/services">}}). If there are many Devices or Services that need to start, it is recommended to add a custom script for verifying that the Device Manager has started all Devices and Services and registered them with the Domain Manager (see `start_post_script` in the [Device Manager Configuration]({{< relref "manual/appendices/adminservice/devicemanager.md" >}})).

The [Device Manager Configuration File]({{< relref "manual/appendices/adminservice/devicemanager.md" >}}) section describes all the available configuration parameters for the Device Manager process.

## Waveforms

REDHAWK waveforms are controlled by files in the `/etc/redhawk/waveforms.d` directory. The `rhadmin` script can generate an example waveform configuration with the complete set of parameters to manage the execution of a REDHAWK waveform.

To create and start a new waveform, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```sh
cd /etc/redhawk/waveforms.d

# This will generate a generic waveform configuration
rhadmin config waveform > waveform.ini

vi waveform.ini
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. This file is broken up into `[waveform:waveformname]` sections that correspond to a SAD file describing a waveform.  By adding multiple sections like this, it is possible to define multiple waveform instances in one file.

The waveform can be configured to start after the Device Manager has started up, it can also optionally wait a configurable amount of time for the domain to be available before attempting to start an instance of the waveform. If the waveform depends on Devices or Services, it is recommended to add a custom script to verify that those Devices and Services have started and registered with the Domain Manager (see `start_pre_script` in the [Waveform Configuration]({{< relref "manual/appendices/adminservice/waveform.md" >}})).

The [Waveform Configuration File]({{< relref "manual/appendices/adminservice/waveform.md" >}}) section describes all the available configuration parameters for launching a REDHAWK waveform.

## Linux Support Files

In addition to running REDHAWK core services, the following support files are provided for REDHAWK logging properties, managing logging output files, system limit definitions, and kernel setup.

### Support Files

`/etc/cron.d/redhawk`  
Cron entry for root to run logrotate every 5 minutes with the configuration file `/etc/redhawk/logging/logrotate.redhawk`

`/etc/redhawk/logging/logrotate.redhawk`  
Logrotate configuration file to manage logfiles generated from REDHAWK services. Rotates all files in `/var/log/redhawk/*.log` when size exceeds 100 MB.

`/etc/redhawk/logging/default.logging.properties`  
Default logging configuration for a REDHAWK system. Defines all messages of INFO level or higher to append to the console, which is captured by the Domain Manager and Device Manager services.

`/etc/redhawk/logging/example.logging.properties`  
Logging configuration with example appenders that are supported by the REDHAWK logging subsystem.

`/etc/redhawk/security/limits.d/99-redhawk-limits.conf`  
Controls file and process limits associated with the redhawk group.

`/etc/redhawk/sysctl.d/sysctl.conf`  
Common kernel tuning parameters for network buffers and core file generation.


{{% children depth="999" %}}
