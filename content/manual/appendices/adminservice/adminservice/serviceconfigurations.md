---
title: "Service Configurations"
weight: 50
---

## File locations (INI)
The AdminService reads configuration files from several directories on startup. The following table lists the locations and the files read:

| **Service**    | **Configuration Directory**  | **File**        |
| :------------- | :--------------------------- | :-------------- |
| Defaults       | `/etc/redhawk/init.d`        | `*.defaults`    |
| Custom         | `/etc/redhawk/extras.d`      | `*.ini`         |
| Domain Manager | `/etc/redhawk/domains.d`     | `*.ini`         |
| Device Manager | `/etc/redhawk/nodes.d`       | `*.ini`         |
| Waveform       | `/etc/redhawk/waveforms.d`   | `*.ini`         |

For more information about the configuration files used to control the REDHAWK core services, refer to the following sections:

- [Domain Manager Configuration File]({{< relref "manual/appendices/adminservice/configuration/domainmanager.md" >}})  
- [Device Manager Configuration File]({{< relref "manual/appendices/adminservice/configuration/devicemanager.md" >}})  
- [Waveform Configuration File]({{< relref "manual/appendices/adminservice/configuration/waveform.md" >}})  


## Creating Configuration files

To create and start new configuration files, enter the following commands replacing `<type>` with `domain`, `node` or `waveform` as appropriate.
```sh
cd /etc/redhawk/<type>s.d

# This will generate a generic configuration file
rhadmin config <type> > <type>.ini

vi <type>.ini
sudo service redhawk-adminservice restart
```
{{% notice note %}}
The configuration files are located in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

## Key Topics

### Process Name

The `rhadmin` script operates with the AdminService referring to domain names or process names for most interactions. A process name takes the form of `<domain name>:<section name>`, where `<domain name>` is the value of the `DOMAIN_NAME` configuration parameter and section name is the `name` value from the section header in the controlling INI file.

The process name for the following example is `REDHAWK_DEV:MyNode`:
```
[node:MyNode]
DOMAIN_NAME=REDHAWK_DEV
NODE_NAME=NODE
```

### Priority

The `priority` setting is the main contributing factor in how the AdminService starts and stops the REDHAWK core services. Every core service has a default priority so that the system will be start Domain Managers, then Device Managers, then waveforms. These values can be overridden in the controlling INI file to customize the start order. If there are multiple domains defined, all processes in the domain with the lowest numerical priority Domain Manager will be started first.

When shutting down a domain, the process with the highest numerical priority will be stopped first.

### Environment Variables

When the AdminService is started, its environment variables are stored and available for use when configuring processes. Any processes started by the AdminService are started with these values in their environment.

Environment variables may also be referenced in the configuration files by using the Python string expression syntax `%(ENV_X)s` as the value for that parameter:
```
loglevel=%(ENV_LOGLEVEL)s
```
In the above example, the log level for the Domain Manager is set to the environment variable `$LOGLEVEL`.

{{% notice note %}}
Environment variables may be overridden by using the `environment` parameter. However, the configuration file only allows parameters with uppercase names (for example, `PYTHONPATH`) to use overridden environment variables.
{{% /notice %}}


## Viewing What Is Configured in the AdminService
To view what services are currently configured in the AdminService, enter the following command:
```sh
rhadmin list
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


## Managing Configurations

The `rhadmin update` command is used to add or remove configurations from the AdminService. This command will reread the configuration files, start any new processes that were defined and stop any currently running processes that are not defined anymore (eg. deleted an INI file). The status threads that monitor processes with the `run_detached=True` setting will be restarted, but the underlying system process will *not* be restarted.

## Rereading the Configuration Files
After adding a new `wave2.ini` file to the waveform configuration directory, to read the new configuration and start the new processes, enter the following command:
```sh
rhadmin update REDHAWK_DEV
```

The following output is displayed:
```
REDHAWK_DEV: updated process group
```

The AdminService reloaded its configuration files and then started the new process for `Wave2` in the `REDHAWK_DEV` domain. To verify that the `Wave2` waveform configuration was added and started, enter the following command:
```sh
rhadmin status
```

The following output is displayed:
```
REDHAWK_DEV:GppNode              RUNNING   pid 31316, uptime 0:00:46
REDHAWK_DEV:REDHAWK_DEV_mgr      RUNNING   pid 31185, uptime 0:00:52
REDHAWK_DEV:Wave                 RUNNING   pid 31745, uptime 0:00:30
REDHAWK_DEV:Wave2                RUNNING   pid 31408, uptime 0:00:41
```
