---
title: "rhadmin"
weight: 30
---

The `rhadmin` script is configured in the `[rhadmin]` section of the `/etc/redhawk/adminserviced.conf` file. The `rhadmin` section of the [AdminService Configuration File]({{< relref "manual/appendices/adminservice/configuration/adminservice.md#rhadmin" >}}) describes the available configuration parameters.

You can use the following command to run the `rhadmin` script using a different configuration file.
```sh
rhadmin -c /etc/redhawk/myadminserviced.conf
```

### Commands

The following table describes the `rhadmin` client script commands used to control the AdminService.

| **Command**      | **Argument**                             | **Description**                                                                                                |
| :--------------- | :--------------------------------------- |:-------------------------------------------------------------------------------------------------------------- |
| `getconfig`      | `<process name>`                         | Displays the current configuration values for the listed process. Can specify multiple arguments.              |
| `list`           |                                          | Shows domains/processes that are configured.                                                                   |
| `query`          | `all`, `<process name>`, `<domain name>` | Queries everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                  |
| `query <type>`   | `all`, `<domain name>`                   | Uses `domain`, `nodes`, or `waveforms` for `<type>` to query only the Domain Manager, Device Managers, or waveforms for the specified domain name or all domains (`all`).   |
| `reload`         |                                          | Restarts the AdminService; implicitly causes it to reread the configuration files.                             |
| `restart`        | `all`, `<process name>`, `<domain name>` | Restarts everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                 |
| `restart <type>` | `all`, `<domain name>`                   | Uses `domain`, `nodes`, or `waveforms` for `<type>` to restart only the Domain Manager, Device Managers, or waveforms for the specified domain name or all domains (`all`). |
| `shutdown`       |                                          | Stops the AdminService.                                                                                        |
| `start`          | `all`, `<process name>`, `<domain name>` | Starts everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                   |
| `start <type>`   | `all`, `<domain name>`                   | Uses `domain`, `nodes`, or `waveforms` for `<type>` to start only the Domain Manager, Device Managers, or waveforms for the specified domain name or all domains (`all`).   |
| `status`         | none, `<process name>`, `<domain name>`  | Shows the status of everything (none), a specified process, or all processes in the specified domain.          |
| `status <type>`  | none, `<domain name>`                    | Uses `domain`, `nodes`, or `waveforms` for `<type>` to status only the Domain Manager, Device Managers or waveforms for the specified domain name or all domains (none).    |
| `stop`           | `all`, `<process name>`, `<domain name>` | Stops everything (`all`), a specified process, or all processes in the specified domain. Can specify multiple `domain name` or `process name` arguments.                    |
| `stop <type>`    | `all`, `<domain name>`                   | Uses `domain`, `nodes`, or `waveforms` for `<type>` to stop only the Domain Manager, Device Managers, or waveforms for the specified domain name or all domains (`all`).    |
| `update`         | none, `<domain name>`                    | Reloads the configuration and optionally starts/stops any domain groupings that have changed. Can specify multiple arguments. |

{{% notice note %}}
**`<type>`** is one of the following strings: 'domain', 'nodes', or 'waveforms'.  
**`<process name>`** is composed of the domain name and the `name` specified in the `[<section type>:<name>]` header in the ini file. The format is: `<domain name>:<name>` (for example, `REDHAWK_DEV:GppNode`).
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
The `start`, `restart`, `stop`, and `status` command support an optional `<type>` argument (`domain`, `nodes`, or `waveforms`), which runs the specified command only on items of that type. For example, the following command only starts the Domain Manager for the `REDHAWK_DEV` domain; the Device Managers and waveforms are not started:
```sh
rhadmin start domain REDHAWK_DEV
```
To restart all configured Device Managers in all domains, use the following command:
```sh
rhadmin start nodes all
```
