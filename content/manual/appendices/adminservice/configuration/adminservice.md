---
title: "AdminService Configuration"
weight: 20
---

#### REDHAWK AdminService System Service Scripts

The following table lists the system service scripts that are used to control the AdminService.

| **Service**            | **System Service Script**                              |
| :--------------------- | :----------------------------------------------------- |
| **CentOS 6 (SysV)**    |                                                        |
| AdminService           | `/etc/rc.d/init.d/redhawk-adminservice`                |
| **CentOS 7 (systemd)** |                                                        |
| AdminService           | `/usr/lib/systemd/system/redhawk-adminservice.service` |
| AdminService Wrapper   | `$OSSIEHOME/bin/adminserviced-start`                   |

As per the Fedora recommendations for service unit files, the AdminService is not enabled during RPM installation. System integrators may enable the service unit file and modify the activation to achieve desired start up and shutdown behavior for their systems.

Because the AdminService is based on Supervisor, all Supervisor [configuration options](<http://supervisord.org/configuration.html>) are available. The following table describes the five main sections in the AdminService configuration file.

#### Creating a Custom AdminService Configuration

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

##### REDHAWK Service Configuration File Sections

| **Section**                                                | **Description**                                               |
| :--------------------------------------------------------- | :------------------------------------------------------------ |
| [[`unix_http_server`]({{< relref "#unix-http-server" >}})] | Defines the Unix socket interface to the AdminService.         |
| [[`inet_http_server`]({{< relref "#inet-http-server" >}})] | Defines the TCP socket interface to the AdminService.          |
| [[`adminserviced`]({{< relref "#adminserviced" >}})]       | Specifies settings for running the AdminService server.                  |
| [[`rhadmin`]({{< relref "#rhadmin" >}})]                   | Specifies settings for running the `rhadmin` client.                     |
| [[`rpcinterface`]({{< relref "#rpcinterface" >}})]         | Defines listeners for external interfaces to the AdminService.|

## unix_http_server

The `unix_http_server` section of the AdminService configuration file defines the local socket that the `rhadmin` can use for remote control of the AdminService. The following section describes all available configuration parameters.

### Configuration Parameters

parameter: `file`  
required: No  
default value: `/var/run/redhawk/adminserviced.sock`  
description: The absolute path to a Unix domain socket used to listen for remote commands.

parameter: `chmod`  
required: No  
default: `0700`  
description: The permissions to set on the socket file on startup.

parameter: `chown`  
required: No  
default value: `redhawk:redhawk`  
description: Specifies the owner of the socket.

parameter: `username`  
required: No  
default value: `redhawk`  
description: The username for remote control of the AdminServer.

parameter: `password`  
required: No  
default value: `redhawk`  
format: Cleartext password or may be specified as a SHA-1 hash if prefixed by the string `{SHA}`. For example, `{SHA}82ab876d1387bfafe46cc1c8a2ef074eae50cb1d` is the SHA-stored version of the password `thepassword`.  
description: The password for remote control of the AdminServer.

## inet_http_server

The AdminService can listen on a local Unix socket or on the network using a TCP socket. The `inet_http_server` section of the AdminService configuration file defines the network socket that the `rhadmin` can use for remote control of the AdminService.

The following section describes all available configuration parameters.

### Configuration Parameters

parameter: `port`  
required: No  
default value: `127.0.0.1:9001`  
description: The IP address and port to listen for remote commands. (Note: `https` is *not* supported.)

parameter: `username`  
required: No  
default value: `redhawk`  
description: The username for remote control of the AdminServer.

parameter: `password`  
required: No  
default value: `redhawk`  
format: Cleartext password or may be specified as a SHA-1 hash if prefixed by the string `{SHA}`. For example, `{SHA}82ab876d1387bfafe46cc1c8a2ef074eae50cb1d` is the SHA-stored version of the password `thepassword`.   
description: The password for remote control of the AdminServer.


## adminserviced

The AdminService configuration is listed in the `[adminserviced]` section of the configuration file.

The following section describes all available configuration parameters.

### Configuration Parameters
{{% notice note %}}
Parameter names are case sensitive.  
The following are the valid values for boolean configuration parameters. If no value is present, the feature is disabled.  
True: `1`, `true`, `True`  
False: `0`, `false`, `False`
{{% /notice %}}

parameter: `pidfile`  
required: No  
default value: `adminserviced.pid`  
format: Absolute path or filename relative to `directory`  
description: The path to the pid file for the AdminService.

parameter: `loglevel`  
required: No  
default value: `info`  
values: `critical`, `error`, `warn`, `info`, `debug`, `trace`, `blather`  
description: The logging level, dictating what is written to the AdminService activity log.

parameter: `logfile`  
required: No  
default value: `adminserviced.log`  
description: The path to the activity log for the AdminService - relative to `directory` or absolute path.

parameter: `logfile_maxbytes`  
required: No  
default value: `50MB`  
description: The maximum size of the activity log before being rotated.

parameter: `logfile_backups`  
required: No  
default value: `10`  
description: The number of backups of the logfiles to keep.

parameter: `childlogdir`  
required: No  
default value: `/var/log/redhawk`  
description: The full path to the directory in which to store the child process log files.

parameter: `childpiddir`  
required: No  
default value: `/var/run/redhawk`  
description: The full path to the directory in which to store the child pid files.

parameter: `user`  
required: No  
default value: `redhawk`  
description: Executes process with User ID.

parameter: `group`  
required: No  
default value: `redhawk`  
description: Executes process with Group ID.

parameter: `environment`  
required: No  
default value: None  
format: A list of key/value pairs in the form `key="value",key2="value2"`  
description: Override existing environment variables or set new ones to be used when starting children.

parameter: `directory`  
required: No  
default value: `$SDRROOT`  
description: Change directory to `directory` before running the AdminService as a daemon.

parameter: `config_dir`  
required: No  
default value: `/etc/redhawk`  
description: The full path to the directory containing the AdminService configuration files.

parameter: `defaults_dir`  
required: No  
default value: `init.d`  
description: The relative path from `config_dir` to the directory containing the AdminService `*.defaults` configuration files.

parameter: `domains_dir`  
required: No  
default value: `domains.d`  
description: The relative path from `config_dir` to the directory containing the Domain Manager `.ini` configuration files.

parameter: `nodes_dir`  
required: No  
default value: `nodes.d`  
description: The relative path from `config_dir` to the directory containing the Device Manager `.ini` configuration files.

parameter: `waveforms_dir`  
required: No  
default value: `waveforms.d`  
description: The relative path from `config_dir` to the directory containing the waveform `.ini` configuration files.

parameter: `umask`  
required: No  
default value: `022`  
description: The umask for the AdminService process.

parameter: `minfds`  
required: No  
default value: `1024`  
description: The minimum number of available file descriptors needed for the AdminService to start.

parameter: `minprocs`  
required: No  
default value: `200`  
description: The minimum number of available process descriptors needed for the AdminService to start.

parameter: `nocleanup`  
required: No  
default value: `False`  
description: Prevent removal of child log files at startup. This only affects processes that run as children of the AdminService, not detached processes.

parameter: `nodaemon`  
required: No  
default value: `False`  
description: Set to True to start the AdminService in the foreground.

parameter: `strip_ansi`  
required: No  
default value: `False`  
description: Strip ANSI escape sequences from child log files of processes that run as children of the AdminService.

parameter: `identifier`  
required: No  
default value: `adminserviced`  
description: The identifier string used by the RPC interface.


## rhadmin

The `rhadmin` control script can be configured in the main AdminService configuration file.

The following section describes all available configuration parameters.

### Configuration Parameters

parameter: `serverurl`  
required: No  
default value: `http://localhost:9001`  
format: `unix:///path/to/file.sock` or `http://\<ip_address\>:\<port\>`  
description: The path to the Unix domain socket or network address used by the AdminServer.

parameter: `username`  
required: No  
default value: `redhawk`  
description: The username to use when connecting to the AdminServer.

parameter: `password`  
required: No  
default value: `redhawk`  
format: Clear text  
description: The password to use when connecting to the AdminServer.

parameter: `prompt`  
required: No  
default value: `redhawk`  
description: Prompt to show when using `rhadmin` in interactive mode (without an option on the command line).

parameter: `history_file`  
required: No  
default value: `~/.rha_history`  
description: The absolute path to a file for storing commands run in interactive mode.


## rpcinterface

The AdminService can be extended to add more functionality with the RPC interface. The interface `rhadmin` uses is implemented as an RPC interface.

The only required parameter is `adminservice.rpcinterface_factory`; however, any additional defined parameters are passed to the described RPC interface.

### Configuration Parameters

parameter: `adminservice.rpcinterface_factory`  
required: Yes  
default value: None  
format: \<package\>:\<function name\>  
description: The package and function to be used by the RPC interface.
