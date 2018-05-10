---
title: "rhadmin"
weight: 30
---

`rhadmin` is a script used outside of system startup/shutdown to manage REDHAWK core service lifecycle from the command line. The `rhadmin` script connects to the AdminService through a socket and supports the following commands to manage the lifecycle of the REDHAWK core services: `start`, `stop`, `restart`, and `status`.

The `rhadmin` script is configured in the `[rhadmin]` section of the `/etc/redhawk/adminserviced.conf` file. The `rhadmin` section of the [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md#rhadmin" >}}) describes the available configuration parameters.

You can use the following command to run the `rhadmin` script using a different configuration file.
```sh
rhadmin -c /etc/redhawk/myadminserviced.conf
```

### Commands

The following table describes the `rhadmin` client script commands that are used to control the AdminService.

| **Command** | **Argument**                             | **Description**                                                                                                |
| :---------- | :--------------------------------------- | :------------------------------------------------------------------------------------------------------------- |
| `add`       | `<domain name>`, `<process name>`        | Activates any updates to the configuration that were made by `reread`.                                         |
| `avail`     |                                          | Shows domains/processes that can be started.                                                                   |
| `getconfig` | `<process name>`                         | Displays the current configuration values for the listed process. Can specify multiple arguments.              |
| `maintail`  | `-f`, `-<bytes>`                         | Displays the AdminService log. `-f` for continuous or `-<bytes>` for amount of log to retrieve.                |
| `reload`    |                                          | Restart the AdminService, implicitly causes it to reread the configuration files.                              |
| `reread`    |                                          | Rereads the configuration files; does not apply changes.                                                       |
| `restart`   | `all`, `<domain name>`, `<process name>` | Restarts the specified domain, process, or everything (`all`). Can specify multiple arguments.                 |
| `shutdown`  |                                          | Stops the AdminService.                                                                                        |
| `start`     | `all`, `<domain name>`, `<process name>` | Starts the specified domain, process, or everything (`all`). Can specify multiple arguments.                   |
| `status`    | none, `<domain name>`, `<process name>`  | Shows the status for the specified domain, process, or everything (none).                                      |
| `stop`      | `all`, `<domain name>`, `<process name>` | Stops the specified domain, process, or everything(`all`). Can specify multiple arguments.                     |
| `update`    | none, `<domain name>`                    | Reloads the configuration and optionally starts/stops any domain groupings that have changed. Can specify multiple arguments. |

All commands can be run in either an interactive console mode or from the command line. To run the `avail` command using the `rhadmin` client script in interactive mode, enter the following:
```sh
rhadmin -i
```
and when the `rh_admin>` prompt is displayed, enter:
```sh
avail
```

To run the `avail` command from the command line, enter the following:
```sh
rhadmin avail
```
