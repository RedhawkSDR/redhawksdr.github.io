---
title: "REDHAWK Waveform Service"
weight: 30
---

This section explains how to manage a single REDHAWK waveform service.  For additional information on managing service configurations and life cycle management, refer to [Domain Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domainmanager.md" >}}), [Device Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/devicemanager.md" >}}), and [Managing Entire Domains ]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domains.md" >}}).


### Creating a Waveform Service Configuration Using the `rhadmin` Script

To create a waveform service configuration, enter the following command:

```sh
rhadmin config waveform >  <output file>.ini
```
A sample configuration is created, which requires the `DOMAIN_NAME` and `WAVEFORM` configuration properties and the section’s name to be specified. The section name may be used with `rhadmin` commands. For additional configuration property settings, refer to the [Waveform Configuration File]({{< relref "manual/appendices/adminservice/configuration/domainmanager.md" >}}) . For the file to be recognized by the AdminService, the file must have an .ini extension and be installed into the proper service directory: `/etc/redhawk/waveforms.d`.

#### Creating a Waveform Service Configuration Using the REDHAWK IDE

1. In the REDHAWK IDE, to create a configuration file, click the Generate Waveform button in the SAD editor.
![Generate Waveform Button](../../../../images/GenerateWaveformButton.png)

2. In the Regenerate Files dialog, check the checkbox next to the .ini file to generate it. If the .spec file is also checked, the generated .spec file will include the installation of the .ini file.
![Generate Waveform File Selection](../../../../images/GenerateWaveformSelectIni.png)

### Displaying a Configuration

To display the current configuration for a service, enter the following command:

```sh
rhadmin getconfig  <Domain Name>:<section name>
```

### Starting a Service

Starting a waveform service will install the application factory in the Domain Manager and then start the waveform. To start a single waveform service, enter the following command:

```sh
rhadmin start <Domain Name>:<section name>
```

### Stopping a Service

To stop a single waveform service, enter the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```

### Requesting Status of a Service

To status a single waveform service, enter the following command:

```sh
rhadmin status <Domain Name>:<section name>
```

### Restarting a Service

To restart a single waveform service, enter the following command:

```sh
rhadmin restart <Domain Name>:<section name>
```


### Example Session

The following example creates, activates, starts, and status a waveform service for the domain `REDHAWK_PROD`, which is identified by the section name `controller`.

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
