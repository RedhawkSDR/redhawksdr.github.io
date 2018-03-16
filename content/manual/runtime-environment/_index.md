---
title: "The Runtime Environment"
weight: 120
---

The REDHAWK runtime environment provides a mechanism for managing the life cycle (creation/tear down/initialization) of  [components]({{< relref "manual/glossary/_index.md#component" >}}) as well as the interconnection of deployed components.

The primary goal of the runtime environment is to support the necessary infrastructure to deploy and manage interconnected components as a running [application]({{< relref "manual/glossary/_index.md#application" >}}).

The runtime environment is personified by two binaries, a  [Domain Manager]({{< relref "manual/glossary/_index.md#domain-manager" >}}) program and a [Device Manager]({{< relref "manual/glossary/_index.md#device-manager" >}}) program. The Domain Manager program hosts an instance of a `DomainManager` class as well as supporting objects ([ApplicationFactory]({{< relref "manual/glossary/_index.md#application-factory" >}}), Application, [FileManager]({{< relref "manual/glossary/_index.md#file-manager" >}})). The Device Manager program hosts an instance of a `DeviceManager` object and an instance of a File System. The only reason why the major classes making up these programs are imported is because their API is remotely available, so a user can arbitrarily interact with these objects. Irrespective of their API, these objects exclusively reside in either the Domain Manager or Device Manager programs.

A single REDHAWK system instance has one Domain Manager and an arbitrary number of Device Managers. The Domain Manager’s role is as a central bookkeeper as well as a single point where applications can be deployed or torn down. A Device Manager is present in each host in the REDHAWK network area; in a purely processing context, there would be a single Device Manager in each computer that is intended to host running components. Each Device Manager has associated with it a set of devices that act as proxies for whatever hardware is running on the computer; in a purely processing context, there is a single [device]({{< relref "manual/glossary/_index.md#device" >}}) proxy describing the microprocessor for each hardware platform Device Manager.

The Domain Manager program:

  - receives an XML file describing a [waveform]({{< relref "manual/glossary/_index.md#waveform" >}})
that is to be deployed.
  - scans all running devices on all Device Managers for a suitable place to deploy the components making up the waveform.
  - uses the File Manager / File System to copy whatever files are necessary to run the components to the target Device Managers.
  - remotely invokes the component processes.
  - interconnects components over the network.
  - tears down applications appropriately.

{{% children depth="999" %}}
