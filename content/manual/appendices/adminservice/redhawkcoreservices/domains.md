---
title: "Managing Services By Domains and Types"
weight: 40
---

This section covers managing a REDHAWK core services on a per domain context or by type (domain, nodes, waveforms).  For additional information on managing service configurations and life cycle management, refer to the appropriate section of the AdminService.

### Managing Services for a Domain

* Reload and restart all services for a domain from current configuration files. 

```sh
rhadmin update <Domain Name>
```

* Start all services in a domain 

```sh
rhadmin start <Domain Name>
```

* Stop all the services in a domain

```sh
rhadmin stop <Domain Name>

```

* Status all the services in a domain

```sh
rhadmin status <Domain Name>
```

* Customized status query for all the services in a domain

```sh
rhadmin query <Domain Name>
```

* Restart all the services for a domain

```sh
rhadmin restart <Domain Name>
```

### Manage Services By Type

Each life cycle management command (start, stop, status, query, restart)  has an optional `type` parameter (domain, nodes, waveforms), which restricts the command to execute against the specific type of service for a domain.  In addition, the value `all` can be substituted for the Domain Name argument which will execute the command for a specific service type, regardless of their domain. The following list describes how to execute the `start` command  and the `type` option. The same command syntax is support for all the life cycel commands (stop, status, query, restart).

* Start a specific Domain Manager service.  

```sh
rhadmin start domain <Domain Name>
```

* Start a all defined Domain Manager services

```sh
rhadmin start domain all
```

* Start a all Device Manager services for a specific domain.

```sh
rhadmin start nodes <Domain Name>
```

* Start all Device Manager services

```sh
rhadmin start nodes all
```

* Start a all Waveform services for a specific domain.

```sh
rhadmin start waveforms <Domain Name>
```

* Start all Waveform services

```sh
rhadmin start waveforms all
```

* Start a all Waveform services for a specific domain.

```sh
rhadmin start waveforms <Domain Name>
```

* Start all Waveform services

```sh
rhadmin start waveforms all
```