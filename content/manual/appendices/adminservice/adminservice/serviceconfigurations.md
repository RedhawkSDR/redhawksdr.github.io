---
title: "Service Configurations"
weight: 50
---

## File locations (INI)

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

--process name, priority

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

## Managing Configurations

--Add/Reread  

## Rereading the Configuration Files
To reread all configuration files and start the new processes, enter the following command:
```sh
rhadmin update REDHAWK_DEV
```

The following output is displayed:
```
REDHAWK_DEV: updated process group
```

AdminService reloaded its configuration files and then started the new process for `Wave2` in the `REDHAWK_DEV` domain. If any configurations were removed (eg. deleted the `wave.ini` file), the processes corresponding to the removed configuration would be stopped. Please note that when the update occurs, the status threads get restarted; therefore a call to `rhadmin status` will look like everything was stopped and started. This is *not* the case, only new or removed processes are affected by this.

To verify that the `Wave2` waveform configuration was added and started, enter the following command:
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

Update  

## Adding Custom Service Definitions

It is possible to have the AdminService show the status of an existing running process that is started outside of the AdminService. The process to monitor must have an associated pid file and then the process can be configured by putting a file in `/etc/redhawk/extras.d`. For example, create the `omniEvents.ini` file with the following contents:
```
[process:omniEvents]
command=/usr/sbin/omniEvents
run_detached=True
pid_file=/var/run/omniEvents.pid
enabled=True
autostart=False
autorestart=False
```
