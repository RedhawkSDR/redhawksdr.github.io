---
title: "The Device Manager"
weight: 60
---

The Device Manager interface is used to manage a set of logical devices, services and a File System. The Device Manager is responsible for parsing the node’s DCD XML in order to fork processes for all devices and services in the node. Each process gets passed a list of command-line character strings as executable parameters that are node-specific configuration variables read from the DCD file.

Once a child process has been forked, its reference is added to a pending list in the Device Manager, while the child process is initialized and configured (possibly with overloaded property values). After that, it registers itself with the Device Manager, moving its reference from the pending list into a registered list. These lists allow the Device Manager to have knowledge of the status of all devices and services contained within its node. Once all children have been launched, instantiated, and configured, the Device Manager connects the devices as described by the DCD file.

The Device Manager also responds to signals from any of its child processes. Upon exit of a child, the Device Manager cleans up the bookkeeping by removing any references to the process from the pending or registered lists and unbinding its name from the Naming Service.

The Device Manager also has a signal handler for `SIGINT`, `SIGQUIT` and `SIGTERM` that triggers a shutdown of the node. The node shutdown process unregisters and calls `releaseObject` on all services and devices, kills off all of the child processes, and unbinds any remaining names, including itself, from the Naming Service.

### Launching a Device Manager from the Command Line

To launch a device from the command line, a Domain Manager must be up and running.

```bash
nodeBooter -d /nodes/DevMgr_localhost.localdomain/DeviceManager.dcd.xml
INFO:DeviceManager - Starting Device Manager with /nodes/DevMgr_localhost.localdomain/DeviceManager.dcd.xml
INFO:DeviceManager_impl - Connecting to Domain Manager REDHAWK_DEV/REDHAWK_DEV
INFO:DeviceManager - Starting ORB! INFO:DCE:9d9bcc38-d654-43b1-8b74-1dc024318b6f:Registering Device
INFO:DeviceManager_impl - Registering device GPP_localhost_localdomain on Device Manager DevMgr_localhost.localdomain
INFO:DeviceManager_impl - Initializing device GPP_localhost_localdomain on Device Manager DevMgr_localhost.localdomain
INFO:DeviceManager_impl - Registering device GPP_localhost_localdomain on Domain Manager
```

### DCD File

The DCD file is an XML file that contains the necessary information to configure a node, which is a specific instance of a Device Manager, as well as all devices and services associated with that Device Manager instance. The DCD file contains information about the devices and services associated with a Device Manager, where to look for a Domain Manager, and other configuration information for devices and services. The file is named: `DeviceManager.dcd.xml` and is located at: `$SDRROOT/dev/nodes/NODE_NAME`.

Information that is covered in this file includes:

  - Device Manager name
  - Device Manager id
  - References to required devices SPD files
  - References to required services SPD files
  - The Domain Manager’s Naming Service name
  - Any required device connections

### Nodes

Nodes are the collection of devices, services, and connections associated with a single Device Manager instance. There is always only one Device Manager per node. Nodes are deployed on a domain to give the applications the ability to communicate with the systems hardware. Upon creation of an application, the Application Factory attempts to place each required component on a device that is deployed in a node.

### Devices

In REDHAWK, devices are specialized components that perform command and control of hardware. They are proxies that are able to run in the domain and provide a single point to interact with one or more pieces of physical hardware.

Devices communicate with various pieces of hardware in order to keep track of their capacities. When waveforms are deployed and the hardware resources are allocated, the devices keep track of the amount of any specific resource that was used, and what is still currently available. This is a important because it keeps components from attempting to over-allocate on any one piece of hardware.

#### Using Valgrind to Debug Devices

The Device Manager supports launching devices using Valgrind, an open source tool that helps detect memory errors and leaks.

The `VALGRIND` environment variable controls this behavior:

  - If `VALGRIND` is not set, devices launch as usual.
  - If `VALGRIND` is set but has no value, the Device Manager searches the path for `valgrind`.
  - If `VALGRIND` has a value, it is assumed to be the full path to the `valgrind` executable.

Valgrind log files are written to the device’s cache directory (e.g., `$SDRROOT/dev/.MyNode/MyDevice`). The log file name follows the pattern `valgrind.<PID>.log`, where PID is the process ID of the device.
