---
title: "Service Configurations"
weight: 50
---

## File locations (INI)



For more information about the configuration files used to control the REDHAWK core services, refer to the following sections:

AdminService Configuration
Domain Manager Configuration File
Device Manager Configuration File
Waveform Configuration File

## Creating Configuration files

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

## Key Topics

--process name, priority

### Environment Variables

When the AdminService is started, its environment variables are stored and available for use when configuring processes. Any processes started by the AdminService are started with these values in their environment.

Environment variables may also be referenced in the configuration files by using the Python string expression syntax `%(ENV_X)s` as the value for that parameter:
```
loglevel=%(ENV_LOGLEVEL)s
```
In the above example, the log level for the Domain Manager is set to the environment variable `$LOGLEVEL`.

Environment variables may be overridden by using the `environment` parameter. However, due to the way the configuration file is processed, these overrides are only available for the parameters whose names are all UPPERCASE.

## Managing Configurations
