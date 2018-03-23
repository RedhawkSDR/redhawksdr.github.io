---
title: "Further Reading"
weight: 40
---

The REDHAWK manual explains the use of REDHAWK to build, deploy, and manage data streaming applications. The principal REDHAWK features are outlined in the following sections, and a reference to the corresponding REDHAWK documentation is provided for further reading.

### References for Application Developers

The following chapters are particularly useful for application developers:

  - **Component development** is introduced in [Components]({{< relref "manual/components/_index.md" >}}). Greater detail related to component development is discussed in the following chapters:

      - [Component Structure]({{< relref "manual/component-structure/_index.md" >}})

      - [Connections]({{< relref "manual/connections/_index.md" >}})

      - [Logging]({{< relref "manual/logging/_index.md" >}})

  - **Waveforms**, including a demonstration of creating a REDHAWK waveform and launching it as an application using the IDE, are discussed in-depth in [Waveforms]({{< relref "manual/waveforms/_index.md" >}})

  - **The Sandbox**, which is used to run components on a local host without any additional runtime infrastructure such as the Domain Manager, is described in-depth in [Sandbox]({{< relref "manual/waveforms/_index.md" >}})

## References for System Developers

The following chapters are useful to system developers:

  - Managing and interacting with hardware through [**Devices**]({{< relref "manual/devices/_index.md" >}})

  - **Devices and Device Managers** make up individual nodes, which are used to deploy and manage devices in a REDHAWK system. Devices are used to determine whether or not a host can deploy any given component. Devices and Device Managers are discussed in-depth in [Devices]({{< relref "manual/devices/_index.md" >}}), [Nodes]({{< relref "manual/nodes/_index.md" >}}), and [The Runtime Environment]({{< relref "manual/runtime-environment/_index.md" >}}).

  - **The Domain Manager and Device Manager** are the foundation for [The Runtime Environment]({{< relref "manual/runtime-environment/_index.md" >}}) for the deployment of distributed applications. [Runtime Environment Inspection]({{< relref "manual/runtime-inspection/_index.md" >}}) describes additional tooling.

  - [**Services**]({{< relref "manual/services/_index.md" >}}), are programs that provide some system-specific always-on software support to components. An example of a service is a web server.
