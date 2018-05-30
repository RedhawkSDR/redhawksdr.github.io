---
title: "REDHAWK Device Manager Service"
weight: 20
---

This section explains how to manage a single REDHAWK Device Manager service.  For additional information on managing service configurations and lifecycle management, refer to [Domain Manager Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domainmanager.md" >}}),
 [Waveform Service]({{< relref "manual/appendices/adminservice/redhawkcoreservices/waveform.md" >}}), and [Managing Entire Domains ]({{< relref "manual/appendices/adminservice/redhawkcoreservices/domains.md" >}}).



### Creating a Device Service Configuration

To create a node service configuration, enter the following command:

```sh
rhadmin config node >  <output file>.ini
```
A sample configuration is created, which requires the `DOMAIN_NAME` and `NODE_NAME` configuration properties and the section's name be specified. The section name may be used with `rhadmin` commands. For additional configuration property settings, refer to the [Device Manager Configuration File]({{< relref "manual/appendices/adminservice/configuration/devicemanager.md" >}}) . For the file to be recognized by the AdminService, the file must have an .ini extension and be installed into the proper service directory: `/etc/redhawk/nodes.d`.  

### Displaying a Configuration

To display the current configuration for a service, enter the following command:

```sh
rhadmin getconfig  <Domain Name>:<section name>
```

### Starting a Service

To start a single Device Manager service, enter the following command:

```sh
rhadmin start <Domain Name>:<section name>
```

### Stopping a Service

To stop a single Device Manager service, enter the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```

### Requesting Status of a Service

To status a single Device Manager service, enter the following command:

```sh
rhadmin status <Domain Name>:<section name>
```

### Restarting a Service

To restart a single Device Manager service, enter the following command:

```sh
rhadmin restart <Domain Name>:<section name>
```

### Example Session

The following example creates, activates, starts, and statuses a Device Manager service for the domain `REDHAWK_PROD`, which is identified by the section name `gpp_node`.

```sh
# generate a node configuration file
rhadmin config node >  redhawk_prod_svr1_gpp.ini

# edit the ini file and change node1 to prodsvr1_gpp in [node:node1],
# and set the following properties: DOMAIN_NAME=REDHAWK_PROD, NODE_NAME=ProdSvr1_GPP
vi redhawk_prod_svr1_gpp.ini

cp redhawk_prod_svr1_gpp.ini /etc/redhawk/nodes.d

rhadmin update REDHAWK_PROD
rhadmin start REDHAWK_PROD:prodsvr1_gpp
rhadmin status REDHAWK_PROD:prodsvr1_gpp

# produces the following output
REDHAWK_PROD:prodsvr1_gpp            RUNNING   pid 2345, uptime 0:00:10
```
