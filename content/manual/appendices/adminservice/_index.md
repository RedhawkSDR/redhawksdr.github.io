---
title: "AdminService"
weight: 55
---

# AdminService

## Introduction

For system integrators, REDHAWK provides an optional RPM with the necessary scripts and configuration files to manage the REDHAWK core services: DomainManager, DeviceManager (includes devices and services), and Waveforms. The RPM, <br>`redhawk-adminservice-<version>.<os>.noarch.rpm`, is provided as part of the REDHAWK Runtime yum respository, but is not installed by default.

The AdminService groups all configured services and Waveforms by domain and then start one domain at a time. By default, the start order, defined by the `priority` setting, is DomainManager, DeviceManager, and then Waveform. An inverted order is used during system shutdown.

The REDHAWK AdminService is built on [Supervisor](<http://supervisord.org>) and uses an INI style configuration file (name=value) to define the command line arguments and execution environment for the service. The configuration files and service scripts provide enough flexibility to manage the REDHAWK core services for most use cases. For specialized cases, the REDHAWK source repository (`adminservice`, `redhawk-adminservice` branch) provides an RPM spec file, `redhawk-adminservice.spec`, and all the source scripts for integrators to build and customize their own service scripts and installation RPM.

The following table describes the configuration directories that contain the INI style configuration files.

##### REDHAWK AdminService Configuration Directories

| **Service**   | **Configuration Directory** |
| :------------ | :-------------------------- |
| AdminService  | /etc/redhawk                |
| Defaults      | /etc/redhawk/init.d         |
| DomainManager | /etc/redhawk/domains.d      |
| DeviceManager | /etc/redhawk/nodes.d        |
| Waveform      | /etc/redhawk/waveforms.d    |

The AdminService is controlled by a single `adminserviced.conf` file residing in `/etc/redhawk`. REDHAWK core services and Waveforms that are to start on system startup are stored in the corresponding `.d` directory. Each REDHAWK core service has a corresponding `.defaults` file in the `/etc/redhawk/init.d` directory.

The DomainManager and DeviceManager configurations directly correspond to a process that runs on a system. On the other hand, the Waveform configurations tell the AdminService how to interact with the running REDHAWK Domain to start and stop Waveforms. The INI content for each specific service is explained in the following sections:

  - [DomainManagers]({{< relref "#domainmanagers" >}})

  - [DeviceManagers]({{< relref "#devicemanagers" >}})

  - [Waveforms]({{< relref "#waveforms" >}})

### AdminService Configuration File

The `/etc/redhawk/adminserviced.conf` file provides the default configuration for the AdminService, these values should be sufficient for most cases. By default, the AdminService always uses a Unix socket for remote control, but it has the option for a TCP socket connection. The [AdminService Configuration File]({{< relref "manual/appendices/adminservice/adminservice.md" >}}) section describes all the available configuration parameters for the AdminService.

To create a new AdminService configuration file and start the service, perform the following commands.  
{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```
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
| AdminService           | `/etc/init.d/redhawk-adminservice`                     |
| **CentOS 7 (systemd)** |                                                        |
| AdminService           | `/usr/lib/systemd/system/redhawk-adminservice.service` |
| AdminService Wrapper   | `$OSSIEHOME/bin/adminserviced-start`                   |

 As per the Fedora recommendations for service unit files, the AdminService is not enabled during RPM installation. It is assumed the system integrators will enable the  service unit file and modify the activation to achieve desired start up and shutdown behavior for their systems.

### rhadmin Client

The rhadmin script is used outside of system startup/shutdown to manage REDHAWK core service lifecycle from the command line. The rhadmin script connects to the AdminService through a socket and supports the following commands to manage the lifecycle of the REDHAWK core services: `start`, `stop`, `restart` and `status`.

The configuration of the rhadmin is through the `[rhadmin]` section in the `/etc/redhawk/adminserviced.conf` file. You can use the following command to run the rhadmin script using a different configuration file.
```
rhadmin -c /etc/redhawk/myadminserviced.conf
```

##### rhadmin Commands

The following table describes the rhadmin client script commands that are used to control the AdminService.

| **Command** | **Argument**           | **Description**                                                                                                |
| :---------- | :--------------------- | :------------------------------------------------------------------------------------------------------------- |
| `add`       | Domain or Process      | Activates any updates to the configuration that were made by reread.                                           |
| `avail`     |                        | Shows Domains/Processes that can be started.                                                                   |
| `maintail`  | -f,<br> -\<bytes\>     | Displays the AdminService log. -f for continuous or -\<bytes\> for amount of log to retrieve                   |
| `reload`    |                        | Restart the AdminService, implicitly causes it to reread the configuration files.                              |
| `reread`    |                        | Rereads the configuration files, doesn't apply changes.                                                        |
| `restart`   | Domain, Process, all   | Restart the specified Domain, Process or everything. Can specify multiple arguments.                           |
| `shutdown`  |                        | Stop the AdminService.                                                                                         |
| `start`     | Domain, Process, all   | Start the specified Domain, Process or everything. Can specify multiple arguments.                             |
| `status`    | Domain, Process, blank | Shows the status for the specified Domain, Process or everything.                                              |
| `stop`      | Domain, Process, all   | Stop the specified Domain, Process or everything. Can specify multiple arguments.                              |
| `update`    | blank, Domain          | Reload the configuration and start/stop any domain groupings that have changed. Can specify multiple arguments |

The rhadmin client script can be run in interactive mode by invoking:
```
rhadmin<enter>
rh_admin> status
```
or as a one time command by invoking
```
rhadmin status
```

## DomainManagers

The REDHAWK DomainManager configuration is controlled by files in the `/etc/redhawk/domains.d` directory. The rhadmin script can generate an example DomainManager configuration file with the complete set of parameters to manage the execution of a REDHAWK DomainManager service.

To create and start new DomainManager services, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```
cd /etc/redhawk/domains.d
rhadmin config domain > mydom.cfg
vi mydom.cfg
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. A DomainManager is defined within a section that has a `[domain:domainname]` header. By adding multiple sections like this, it is possible to define multiple DomainManager services in one file. It is recommended that you define only one DomainManager service per configuration file.

The [DomainManager Configuration File]({{< relref "manual/appendices/adminservice/domainmanager.md" >}}) section describes all the available configuration parameters for the DomainManager process.

## DeviceManagers

The REDHAWK DeviceManager configuration is controlled by files in the `/etc/redhawk/nodes.d` directory. The rhadmin script can generate an example DeviceManager configuration file with the complete set of parameters to manage the execution of a REDHAWK DeviceManager service. There are no rules on partitioning nodes for a REDHAWK system. REDHAWK’s only guidance is that you define one GPP per computing host.

To create and start new DeviceManager services, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```
cd /etc/redhawk/nodes.d
rhadmin config node > redhawk_dev.gpp.node.cfg
vi redhawk_dev.gpp.node.cfg
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. A DeviceManager is defined within a section that has a `[node:nodename]` header, each section corresponds to a DCD file describing a [Node]({{< relref "manual/nodes">}}). By adding multiple sections like this, it is possible to define multiple DeviceManager services in one file. If you have multiple Nodes defined for a computing host, it is recommended that you have one configuration file per Node definition.

The DeviceManager can be configured to start after the DomainManager has started up, or it can start up at the same time as the DomainManager and it will wait for the domain to be available and register its [Devices]({{< relref "manual/devices">}}) and [Services]({{< relref "manual/services">}}). If there are many Devices or Services that need to start, it is recommended to add a custom script for verifying that the DeviceManager has started all Devices and Services and registered them with the DomainManager(see `start_pre_script` in the [DeviceManager Configuration]({{< relref "manual/appendices/adminservice/devicemanager.md" >}})).

The [DeviceManager Configuration File]({{< relref "manual/appendices/adminservice/devicemanager.md" >}}) section describes all the available configuration parameters for the DeviceManager process.

## Waveforms

REDHAWK Waveforms are controlled by files in the `/etc/redhawk/waveforms.d` directory. The rhadmin script can generate an example Waveform configuration with the complete set of parameters to manage the execution of a REDHAWK Waveform.

To create and start a new Waveform, perform the following commands.

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

```
cd /etc/redhawk/waveforms.d
rhadmin config waveform > redhawk_dev.controller.cfg
vi redhawk_dev.controller.cfg
sudo service redhawk-adminservice restart
```

### Configuration File

The configuration file follows a standard INI file format. This file is broken up into `[waveform:waveformname]` sections that correspond to a SAD file describing a Waveform.  By adding multiple sections like this, it is possible to define multiple Waveform instances in one file.

The Waveform can be configured to start after the DeviceManager has started up, it can also optionally wait a configurable amount of time for the domain to be available before attempting to start an instance of the Waveform. If the waveform depends on Devices or Services, it is recommended to add a custom script to verify that those Devices and Services have started and registered with the DomainManager(see `start_pre_script` in the [Waveform Configuration]({{< relref "manual/appendices/adminservice/waveform.md" >}})).

The [Waveform Configuration File]({{< relref "manual/appendices/adminservice/waveform.md" >}}) section describes all the available configuration parameters for launching a REDHAWK Waveform.

## Linux Support Files

In addition to running REDHAWK services, the following support files are provided for REDHAWK logging properties, managing logging output files, system limit definitions, and kernel setup.

### Support Files

file: `/etc/redhawk/cron.d/redhawk`  
description: Cron entry for root to run logrotate every 5 minutes with the configuration file `/etc/redhawk/logging/logrotate.redhawk`

file: `/etc/redhawk/logging/logrotate.redhawk`  
description: Logrotate configuration file to manage logfiles generated from REDHAWK services. Rotates all files in `/var/log/redhawk/*.log` when size exceeds 100 MB.

file: `/etc/redhawk/logging/default.logging.properties`  
description: Default logging configuration for a REDHAWK system. Defines all messages of INFO level or higher to append to the console, which is captured by the DomainManager and DeviceManager services.

file: `/etc/redhawk/logging/example.logging.properties`  
description: Logging configuration with example appenders that are supported by the REDHAWK logging subsystem.

file: `/etc/redhawk/security/limits.d/99-redhawk-limits.conf`  
description: Controls file and process limits associated with the redhawk group.

file: `/etc/redhawk/security/sysctl.d/sysctl.conf`  
description: Common kernel tuning parameters for network buffers and core file generation.

## Build Customized RPM

The following sequence of commands can be used to build a `redhawk-adminservice` RPM file. In this example, the version is `2.1.3`, the user name is `someuser`, and the `adminservice` repo has been cloned in the directory, `/home/someuser/repos`.

{{% notice note %}}
The following script will remove the `rpmbuild` directory from your home directory.
{{% /notice %}}


```
tar -C ~/repos/adminservice -czf ~/redhawk-adminservice-2.1.3.tar.gz .
rm -rf ~/rpmbuild
mkdir -p rpmbuild/{BUILD,RPMS/noarch,SOURCES,SPECS,SRPMS}
cp ~/redhawk-adminservice-2.1.3.tar.gz ~/rpmbuild/SOURCES/
cp  ~/repo/adminservice/redhawk-adminservice.spec ~/rpmbuild/SPECS/
rpmbuild -bb --define "_topdir /home/someuser/rpmbuild" --define "_sourcedir /home/someuser/rpmbuild/SOURCES"  ~/rpmbuild/SPECS/redhawk-adminservice.spec
```

{{% children depth="999" %}}
