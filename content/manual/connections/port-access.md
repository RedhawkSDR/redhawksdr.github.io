---
title: "Port Access"
weight: 30
---

A port belongs to a component or device (devices are specialized components - see [Working With Devices]({{< relref "manual/devices/_index.md" >}}) for additional information). To retrieve a port, an external entity needs to call `getPort()` on the component that owns that port. The argument to the `getPort()` function is the string name for the port, and the return value is a CORBA pointer to that port object. Both uses and provides ports are retrieved from components through this function call. Base supported interfaces are not retrieved through the `getPort()`, because they are not ports. Instead, these references are retrieved directly from an entity like the Domain Manager or the Device Manager.
