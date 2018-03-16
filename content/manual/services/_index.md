---
title: "Services"
weight: 80
---

In the context of REDHAWK, infrastructure software is managed by the [Device Manager]({{< relref "manual/glossary/_index.md#device-manager" >}}). Infrastructure software can take two forms: [hardware management]({{< relref "manual/nodes/_index.md" >}}) and system services. System services are the equivalent to Linux services like an HTTP server; the service does not directly control hardware but is some facility that becomes automatically available when the host is started.

## Management

A service is managed through the Device Manager.  [Nodes]({{< relref "manual/nodes/_index.md" >}}) are described as a Device Manager instance and the set of devices and services associated with that Device Manager instance. When the Device Manager instance is created, it reads its configuration file (`dcd.xml`), which contains a list of all devices and services that the node contains. The devices and services are forked as individual processes and remain running until the Device Manager is shut down. Devices are bound to the [Naming Service]({{< relref "manual/glossary/_index.md#naming-service" >}}) under the Device Manager’s context. Services, on the other hand, are bound to the domain’s naming context.

## Files Defining a Service

A service, much like a component or device, is a binary file and a set of XML descriptors, normally just the [SPD]({{< relref "manual/appendices/acronyms.md#spd" >}}) (describing the service’s software package) and [SCD]({{< relref "manual/appendices/acronyms.md#scd" >}}) (describing the service’s interfaces).  The service file package resides in `$SDRROOT/dev`, usually in a `services` subdirectory or the `devices` subdirectory.

## Service API

REDHAWK does not include any standard service interfaces; the [SCA]({{< relref "manual/appendices/acronyms.md#sca" >}})  specification includes a logging service, but in [REDHAWK logging]({{< relref "manual/logging/_index.md" >}}) is performed through `log4j`/`log4cxx`, not a service.  To use a service, it either needs to use one of REDHAWK’s standard interfaces or it needs to implement a specialized interface supporting some specific behavior.

A service is [created and torn-down](#management) by the Device Manager.  Because there is no defined interface for the service, services do not support the `LifeCycle` interface, and more importantly, the `releaseObject()` method. Since there is no release method, the Device Manager issues operating-system level signals to terminate the service. It is the service developer’s task to perform whatever cleanup functionality needs to be performed in response to the receipt of a signal from the operation system.

## Finding a Service

[Application]({{< relref "manual/glossary/_index.md#application" >}}) descriptor files (`sad.xml`) are described in [The Runtime Environment]({{< relref "manual/runtime-environment/_index.md" >}}); these files describe a logical collection of components that are deployed to support some system-level application. In the application descriptor file, connections between deployed entities can be described in a variety of ways. These connection descriptions are how an application, or more specifically a component in an application, can interact with a service.

The [**Find By** feature]({{< relref "manual/runtime-environment/applications.md#using-the-find-by-feature" >}}) enables users to find services through either their (globally unique) name or by the interface that they support. Using a globally unique name as the search key allows a developer to specify an single instance of a service to connect to. On the other hand, by searching for a service through its supported interface, a developer can create a generalized search for a service that satisfies a particular need, irrespective of how it is associated with the Domain.
