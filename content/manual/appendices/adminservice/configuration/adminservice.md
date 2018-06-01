---
title: "AdminService Configuration"
weight: 20
---

#### Creating a Custom AdminService Configuration

To create a new AdminService configuration file and start the service, enter the following commands.
```sh
cd /etc/redhawk
rhadmin config admin > adminserviced.cfg
vi adminserviced.cfg
adminserviced
```
{{% notice note %}}
The configuration files are located in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}

##### REDHAWK Service Configuration File Sections

| **Section**                                                | **Description**                                               |
| :--------------------------------------------------------- | :------------------------------------------------------------ |
| [[`unix_http_server`]({{< relref "#unix-http-server" >}})] | Defines the Unix socket interface to the AdminService.        |
| [[`rhadmin`]({{< relref "#rhadmin" >}})]                   | Specifies settings for running the `rhadmin` client.          |

## unix_http_server

The `unix_http_server` section of the AdminService configuration file defines the local socket that the `rhadmin` can use for remote control of the AdminService. The following section describes all available configuration parameters.

### Configuration Parameters

parameter: `file`  
required: No  
default value: `/var/run/redhawk/adminserviced.sock`  
description: The absolute path to a Unix domain socket used to listen for remote commands.

parameter: `username`  
required: No  
default value: `redhawk`  
description: The username for remote control of the AdminServer.

parameter: `password`  
required: No  
default value: `redhawk`  
format: Cleartext password or may be specified as a SHA-1 hash if prefixed by the string `{SHA}`. For example, `{SHA}82ab876d1387bfafe46cc1c8a2ef074eae50cb1d` is the SHA-stored version of the password `thepassword`.  
description: The password for remote control of the AdminServer.

## rhadmin

The `rhadmin` control script can be configured in the main AdminService configuration file.

The following section describes all available configuration parameters.

### Configuration Parameters

parameter: `serverurl`  
required: No  
default value: `unix:///var/run/redhawk/adminserviced.sock`  
format: `unix:///path/to/file.sock`  
description: The path to the Unix domain socket used by the AdminServer.

parameter: `username`  
required: No  
default value: `redhawk`  
description: The username to use when connecting to the AdminServer.

parameter: `password`  
required: No  
default value: `redhawk`  
format: Clear text  
description: The password to use when connecting to the AdminServer.
