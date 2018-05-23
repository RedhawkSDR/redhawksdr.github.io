---
title: "REDHAWK Domain Manger Service"
weight: 10
---

This section covers managing a single REDHAWK Domain Manager service.  For additional information on managing service configurations and life cycle management, refer to the appropriate section of the AdminService.


### Create a Domain Service Configuration

To create a domain service configuration run the following command:

```sh
rhadmin config domain >  <output file>.ini
```
This creates a sample configuration that requires the DOMAIN_NAME configuration property to be set and the section's name. The section name can be used with rhadmin commands. For additional configuration property settings consult the [Domain Manager Configuration File]({{< relref "manual/appendices/adminservice/configuration/domainmanager.md" >}}) . For the  file to be recognized by the AdminService, the file is required to have an .ini extension and be installed into the proper service directory: /etc/redhawk/domains.d.  

### Activating a configuration

Activating the service configuration requires the following commands:

```sh
rhadmin reread
rhadmin add <Domain Name>:<section name>
```

### Display a configuration

To display the current configuration for a service:

```sh
rhadmin getconfig  <Domain Name>:<section name>
```

### Starting a service

To start a single Domain Manager service run the following command:

```sh
rhadmin start <Domain Name>:<section name>
```

### Stopping a service

To stop a single Domain Manager service run the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```

### Requesting status of a service

To status a single Domain Manager service run the following command:

```sh
rhadmin status <Domain Name>:<section name>
```

### Restarting a service

Restarting a single Domain Manager service:

```sh
rhadmin restart <Domain Name>:<section name>
```

### Example session
The following example creates, activates, starts, and status a Domain Manager service for the domain `REDHAWK_PROD` and identified by section `redhawk_prod`.

```sh
# generate a domain configuration file
rhadmin config domain >  redhawk-prod.ini

# edit the ini file and change redhawk_prod in [domain:domain1], and provide a value for DOMAIN_NAME=REDHAWK_PROD
vi redhawk-prod.ini

cp redhawk-prod.ini /etc/redhawk/domains.d

rhadmin reread
rhadmin add  REDHAWK_PROD:redhawk_prod
rhadmin start REDHAWK_PROD:redhawk_prod
rhadmin status REDHAWK_PROD:redhawk_prod

# produces the following output
REDHAWK_PROD:redhawk_prod            RUNNING   pid 1234, uptime 0:00:10

```
