---
title: "Managing Services By Domains and Types"
weight: 40
---

This section explains how to manage REDHAWK core services either by domain or by type (`domain`, `nodes`, or `waveforms`).  For additional information on managing service configurations and lifecycle management, refer to [Domain Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domainmanager.md" >}}), [Device Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/devicemanager.md" >}}), and [Waveform Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/waveform.md" >}}).

### Managing Services for a Domain

Services for a domain can be managed using the following commands.

* Reloading and restarting all services for a domain from current configuration files

```sh
rhadmin update <Domain Name>
```

* Starting all services in a domain

```sh
rhadmin start <Domain Name>
```

* Stoping all the services in a domain

```sh
rhadmin stop <Domain Name>

```

* Inspecting the status of all the services in a domain

```sh
rhadmin status <Domain Name>
```

* Customizing the status query for all the services in a domain

```sh
rhadmin query <Domain Name>
```

* Restarting all the services for a domain

```sh
rhadmin restart <Domain Name>
```

### Managing Services by Type

Each lifecycle management command (`start`, `stop`, `status`, `query`, and `restart`)  has an optional `type` parameter (`domain`, `nodes`, and `waveforms`), which restricts the command to execute against the specific type of service for a domain.  In addition, the value `all` can be substituted for the `<Domain Name>` argument, which executes the command for a specific service type, regardless of the service type's domain. The same command syntax is supported for all lifecycle commands (`start`, `stop`, `status`, `query`, `restart`):

```sh
rhadmin command type <Domain Name>|all
```

The following commands demonstrate how to execute the `start` command  using the `type` option.

* Starting a specific Domain Manager service

```sh
rhadmin start domain <Domain Name>
```

* Starting all defined Domain Manager services

```sh
rhadmin start domain all
```

* Starting all Device Manager services for a specific domain.

```sh
rhadmin start nodes <Domain Name>
```

* Starting all Device Manager services

```sh
rhadmin start nodes all
```

* Starting all waveform services for a specific domain.

```sh
rhadmin start waveforms <Domain Name>
```

* Starting all waveform services

```sh
rhadmin start waveforms all
```
