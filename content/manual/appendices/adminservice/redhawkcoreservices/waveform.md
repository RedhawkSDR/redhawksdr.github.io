---
title: "REDHAWK Waveform Service"
weight: 30
---

This section covers managing a single REDHAWK Waveform service.  For additional information on managing service configurations and lifecycle management, refer to the appropriate section of the AdminService.


### Create a Waveform Service Configuration

To create a waveform service configuration run the following command:

```sh
rhadmin config waveform >  <output file>.ini
```
This creates a sample configuration that requires the DOMAIN_NAME and WAVEFORM configuration property to be set and the section's name. The section name can be used with rhadmin commands. For additional configuration property settings consult the [Waveform Configuration File]({{< relref "manual/appendices/adminservice/configuration/domainmanager.md" >}}) . For the file to be recognized by the AdminService, the file is required to have an .ini extension and be installed into the proper service directory: `/etc/redhawk/waveforms.d`.  

### Display a configuration

To display the current configuration for a service:

```sh
rhadmin getconfig  <Domain Name>:<section name>
```

### Starting a service

To start a single Waveform service run the following command:

```sh
rhadmin start <Domain Name>:<section name>
```

### Stopping a service

To stop a single Waveform service run the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```

### Requesting status of a service

To status a single Waveform service run the following command:

```sh
rhadmin status <Domain Name>:<section name>
```

### Restarting a service

Restarting a single Waveform service:

```sh
rhadmin restart <Domain Name>:<section name>
```


### Example session

The following example creates, activates, starts, and status a Waveform service for the domain `REDHAWK_PROD` and identified by the section name `controller`.

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
