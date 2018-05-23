---
title: "REDHAWK Device Manager Service"
weight: 20
---

This section covers managing a single REDHAWK Device Manager service.  For additional information on managing service configurations and life cycle management, refer to the appropriate section of the AdminService.


### Create a Device Service Configuration

To create a node service configuration run the following command:

```sh
rhadmin config node >  <output file>.ini
```
This creates a sample configuration that requires the DOMAIN_NAME and NODE_NAME configuration properties to be set, and the section's name. The section name can be used with rhadmin commands. For additional configuration property settings consult the [Device Manager Configuration File]({{< relref "manual/appendices/adminservice/configuration/devicemanager.md" >}}) . For the file to be recognized by the AdminService, the file is required to have an .ini extension and be installed into the proper service directory: /etc/redhawk/nodes.d.  

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

To start a single Device Manager service run the following command:

```sh
rhadmin start <Domain Name>:<section name>
```
### Stopping a service

To stop a single Device Manager service run the following command:

```sh
rhadmin stop <Domain Name>:<section name>
```
### Requesting status of a service

To status a single Device Manager service run the following command:

```sh
rhadmin status <Domain Name>:<section name>
```
### Restarting a service

Restarting a single Device Manager service:

```sh
rhadmin restart <Domain Name>:<section name>
```

### Example session
The following example creates, activates, starts, and status a Device Manager service for the domain `REDHAWK_PROD` and identified by section `gpp_node`.

```sh
# generate a node configuration file
rhadmin config node >  redhawk_prod_svr1_gpp.ini

# edit the ini file and change node1 to prosvr1_gpp in [node:node1], and set the following properties: DOMAIN_NAME=REDHAWK_PROD, NODE_NAME=ProdSvr1_GPP
vi redhawk_prod_svr1_gpp.ini

cp redhawk_prod_svr1_gpp.ini /etc/redhawk/nodes.d

rhadmin reread
rhadmin add  REDHAWK_PROD:prodsvr1_gpp
rhadmin start REDHAWK_PROD:prodsvr1_gpp
rhadmin status REDHAWK_PROD:prodsvr1_gpp

# produces the following output
REDHAWK_PROD:redhawk_prod            RUNNING   pid 1234, uptime 0:20:00
REDHAWK_PROD:prodsvr1_gpp            RUNNING   pid 2345, uptime 0:00:10

```
