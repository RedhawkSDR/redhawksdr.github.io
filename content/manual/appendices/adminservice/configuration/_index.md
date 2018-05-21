---
title: "AdminService Configuration"
weight: 10
---

The `/etc/redhawk/adminserviced.conf` file provides the default configuration for the AdminService and the `rhadmin` client script; these values should be sufficient for most cases. By default, the AdminService uses a Unix socket for remote control, but it has an option for a TCP socket connection. The [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md" >}}) section describes the available configuration parameters for the AdminService.

To create a new AdminService configuration file and start the service, perform the following commands.
```sh
cd /etc/redhawk
rhadmin config admin > myadminserviced.cfg
vi myadminserviced.cfg
adminserviced -c /etc/redhawk/myadminserviced.cfg
```

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

## Environment Variables

When the AdminService is started, its environment variables are stored and available for use when configuring processes. Any processes started by the AdminService are started with these values in their environment.

Environment variables may also be referenced in the configuration files by using the Python string expression syntax `%(ENV_X)s` as the value for that parameter:
```
loglevel=%(ENV_LOGLEVEL)s
```
In the above example, the log level for the Domain Manager is set to the environment variable `$LOGLEVEL`.

Environment variables may be overridden by using the `environment` parameter. However, due to the way the configuration file is processed, these overrides are only available for the parameters whose names are all UPPERCASE.

## REDHAWK Core Services

The Domain Manager and Device Manager configurations directly correspond to a process that runs on a system. On the other hand, the waveform configurations tell the AdminService how to interact with the running REDHAWK Domain to start and stop waveforms.

To create and start new configuration file, perform the following commands substituting `domain`, `node` or `waveform` for `<type>`.
```sh
cd /etc/redhawk/<type>s.d

# This will generate a generic configuration file
rhadmin config <type> > <type>.ini

vi <type>.ini
sudo service redhawk-adminservice restart
```

{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

For more information about the configuration files used to control the REDHAWK core services, refer to the following sections:

{{%children%}}
