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

The following table describes the `rhadmin` client script commands used to control the AdminService.

| **Command**      | **Argument**                             | **Description**                                                                                                |
| :--------------- | :--------------------------------------- |:-------------------------------------------------------------------------------------------------------------- |
| `add`            | `<domain name>`, `<process name>`        | Activates any updates to the configuration that were made by `reread`.                                         |
| `avail`          |                                          | Shows domains/processes that can be started.                                                                   |
| `getconfig`      | `<process name>`                         | Displays the current configuration values for the listed process. Can specify multiple arguments.              |
| `maintail`       | `-f`, `-<bytes>`                         | Displays the AdminService log. `-f` for continuous or `-<bytes>` for amount of log to retrieve.                |
| `reload`         |                                          | Restart the AdminService; implicitly causes it to reread the configuration files.                              |
| `reread`         |                                          | Rereads the configuration files; does not apply changes.                                                       |
| `restart`        | `all`, `<process name>`, `<domain name>` | Restarts everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                  |
| `restart <type>` | `all`, `<domain name>`                   | Use `domain`, `nodes`, or `waveforms` for `<type>` to restart only the Domain Manager, Device Managers or waveforms for the specified domain name or all domains (`all`). |
| `shutdown`       |                                          | Stops the AdminService.                                                                                        |
| `start`          | `all`, `<process name>`, `<domain name>` | Starts everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                     |
| `start <type>`   | `all`, `<domain name>`                   | Use `domain`, `nodes`, or `waveforms` for `<type>` to start only the Domain Manager, Device Managers or waveforms for the specified domain name or all domains (`all`). |
| `status`         | none, `<process name>`, `<domain name>`  | Shows the status of everything (none), a specified process, or all processes in the specified domain.          |
| `status <type>`  | none, `<domain name>`                    | Use `domain`, `nodes`, or `waveforms` for `<type>` to status only the Domain Manager, Device Managers or waveforms for the specified domain name or all domains (none).  |
| `stop`           | `all`, `<process name>`, `<domain name>` | Stops everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                     |
| `stop <type>`    | `all`, `<domain name>`                   | Use `domain`, `nodes`, or `waveforms` for `<type>` to stop only the Domain Manager, Device Managers or waveforms for the specified domain name or all domains (`all`). |
| `update`         | none, `<domain name>`                    | Reloads the configuration and optionally starts/stops any domain groupings that have changed. Can specify multiple arguments. |

{{% notice note %}}
**`<type>`** is one of the following strings: 'domain', 'nodes', 'waveforms'  
**`<process name>`** is composed of the domain name and the `name` specified in the `[<section type>:<name>]` header in the ini file. The format of process name is: `<domain name>:<name>` (eg. `REDHAWK_DEV:GppNode`)
{{% /notice %}}

### Running Commands
All commands can be run in either an interactive console mode or from the command line. To run the `avail` command using the `rhadmin` client script in interactive mode, enter the following:
```sh
rhadmin -i
```
and when the `rh_admin>` prompt is displayed, enter:
```sh
avail
```

To run the same command from the command line, enter the following:
```sh
rhadmin avail
```

### Specifying an Optional type
The `start`, `restart`, `stop` and `status` command support an optional `<type>` argument (`domain`, `nodes`, or `waveforms`). This will run the specified command only on items of that type. For example, the following command will only start the Domain Manager for the `REDHAWK_DEV` domain, the Device Managers and waveforms will not be started:
```sh
rhadmin start domain REDHAWK_DEV
```
To restart all configured Device Managers in all domains, use the following command:
```sh
rhadmin start nodes all
```
