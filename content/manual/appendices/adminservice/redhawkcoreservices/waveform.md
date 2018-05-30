---
title: "REDHAWK Waveform Service"
weight: 30
---

This section explains how to manage a single REDHAWK Waveform service.  For additional information on managing service configurations and lifecycle management, refer to [Domain Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domainmanager.md" >}}), [Device Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/devicemanager.md" >}}), and [Managing Entire Domains ]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domains.md" >}}).


### Creating a Waveform Service Configuration

To create a waveform service configuration, enter the following command:

```sh
rhadmin config waveform >  <output file>.ini
```
A sample configuration is created, which requires the `DOMAIN_NAME` and `WAVEFORM` configuration properties and the section’s name be specified. The section name may be used with `rhadmin` commands. For additional configuration property settings, refer to the [Waveform Configuration File]({{< relref "manual/appendices/adminservice/configuration/domainmanager.md" >}}) . For the file to be recognized by the AdminService, the file must have an .ini extension and be installed into the proper service directory: `/etc/redhawk/waveforms.d`.  

### Displaying a Configuration

To display the current configuration for a service, enter the following command:

```sh
rhadmin getconfig  <Domain Name>:<section name>
```

### Starting a Service

To start a single Waveform service, enter the following command:

```sh
rhadmin start <Domain Name>:<section name>
```

### Stopping a Service

To stop a single Waveform service, enter the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```

### Requesting Status of a Service

To status a single Waveform service, enter the following command:

```sh
rhadmin status <Domain Name>:<section name>
```

### Restarting a Service

To restart a single Waveform service, enter the following command:

```sh
rhadmin restart <Domain Name>:<section name>
```


### Example Session

The following example creates, activates, starts, and status a Waveform service for the domain `REDHAWK_PROD`, which is identified by the section name `controller`.

```sh
# generate a waveform configuration file
rhadmin config waveform >  redhawk_prod_controller.ini

# edit the ini file and change waveform1 to controller in [waveform:waveform1],
# and set the following properties: DOMAIN_NAME=REDHAWK_PROD, WAVEFORM=controller
vi redhawk_prod_controller.ini

cp redhawk_prod_controller.ini /etc/redhawk/waveforms.d

rhadmin update REDHAWK_PROD
rhadmin start REDHAWK_PROD:controller
rhadmin status REDHAWK_PROD:controller

# produces the following output
REDHAWK_PROD:controller              RUNNING   pid 3456, uptime 0:00:10
```
