---
title: "Nodes"
weight: 100
---
In [The Runtime Environment]({{< relref "manual/runtime-environment/_index.md" >}}), the [Device Manager]({{< relref "manual/glossary/_index.md#device-manager" >}}) is introduced along with its primary role: to deploy and manage [device]({{< relref "manual/glossary/_index.md#device" >}}) proxies in whatever host the Device Manager has been launched. The term [Node]({{< relref "manual/glossary/_index.md#node" >}}) is used to refer to the Device Manager and its associated devices and [services]({{< relref "manual/glossary/_index.md#service" >}}).

The concept of a device and its relationship with the deployment of a set of [components]({{< relref "manual/glossary/_index.md#component" >}}) in an application is one of the more complicated concepts in REDHAWK. A device is a program that the Domain Manager interacts with to determine whether or not a component can run on the host where that device is located. If it is determined that the device can host that component, the device provides the Domain Manager with the API necessary to load the component binaries over the network and execute them. In short, the device provides the remote host discovery mechanism to the Domain Manager and the mechanism needed to load and execute whatever component files are needed to run the component on the device host.

This chapter provides a description of how to interact with devices and nodes using the REDHAWK IDE.

{{% children depth="999" %}}
