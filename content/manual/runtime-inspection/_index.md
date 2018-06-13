---
title: "Runtime Environment Inspection"
tags:
  - sandbox
weight: 120
---

During runtime, there are a large number of operations that are handled under the hood by the REDHAWK Core Framework. At times though, it becomes necessary to take a closer look at these underlying parts to ensure that they are working properly or to inspect what kind of a state they are currently in. REDHAWK provides tools to help accomplish this task.

## REDHAWK Module

A Python module called REDHAWK is provided with the capability to interact with running domains, devices, waveforms and components. This allows for individual control and assessment over all aspects of a runtime environment.

In order to use the REDHAWK utility module, begin a Python session from a terminal and enter the following command:

```python
>>> from ossie.utils import redhawk
```

The REDHAWK module is built on the same foundation as the Sandbox, and provides a compatible interface for domain objects. Sandbox objects, including plots, can be dynamically connected to devices, waveforms and components running in the domain.

Refer to [Working with Components, Devices, and Services]({{< relref "manual/sandbox/python/working_with_components.md" >}}) for more information.

### Attach

The module provides the ability to attach to a running domain.

This allows the user access to the underlying API of the Domain Manager in addition to other useful functionality:
  - `createApplication()` - install/create a particular waveform
  - `removeApplication()` - release a particular waveform

To attach to an existing domain, the name must be passed as an argument.

Assuming the domain name is `MY_DOMAIN`, start a Python script and enter:

```python
>>> domain = redhawk.attach("MY_DOMAIN")
```

Note that if there is only 1 Domain visible, no argument is needed for the attach call:

```python
 >>> domain = redhawk.attach()
```

Once attached to the running domain, waveforms that are installed in the `$SDRROOT` can easily be launched using the `createApplication()` function:

```python
>>> wave = domain.createApplication("/waveforms/wave/wave.sad.xml")
```

Upon success, the above call returns an `Application` object which gives access to the external resource API. This allows for manual operation of the application. In addition, functions exist to allow the user to connect and disconnect ports. Finally, in order to inspect the current conditions of the waveform, an API function call is available. This shows any external ports, components that are in the application, and their properties.

```py
>>> wave
<ossie.utils.redhawk.core.App object at 0x2bfb350>
>>> wave.api()
Waveform [wave_025_090314360_1]
---------------------------------------------------
External Ports ==============
Provides (Input) Ports ==============
Port Name       Port Interface
---------       --------------
external_in     IDL:BULKIO/dataChar


Uses (Output) Ports ==============
Port Name       Port Interface
---------       --------------
external_out    IDL:BULKIO/UsesPortStatisticsProvider


Components ==============
1. Sink
2. Source (Assembly Controller)


Properties ==============
Property Name    (Data Type)      [Default Value]   Current Value
-------------    -----------      ---------------   -------------
sample_size      (long/SL/32t)    None              None
```
Once finished, the waveform needs to be removed from the domain by using the `removeApplication()` method:

```py
>>> domain.removeApplication(wave)
```

### Starting a Domain from within a Python session

Normally, the REDHAWK Python package is used to either interact with a running domain, or to launch some [Sandbox]({{< relref "manual/sandbox/_index.md" >}}) components. However, sometimes it may be required to launch a domain from a Python script.

To help in such a scenario, the REDHAWK Python package includes some helper functions. The `kickDomain` feature allows for an easy way to launch domain and Device Managers from within a Python script. With no arguments, the function launches and returns the Domain Manager that is installed in `$SDRROOT`. Additionally, all Device Managers in `$SDRROOT/dev/nodes` are started.

Other arguments to the function exist to control different aspects to how the domain is started. A list of specific Device Managers to start can be passed if the user does not want to start all available. If the `$SDRROOT` that the user wishes to use is not in the standard location, a path can be supplied to direct the function to the desired place in the file system. Standard out and logging to a file can also be set.

```py
>>> domain = redhawk.kickDomain()
INFO:DomainManager - Starting Domain Manager
INFO:DeviceManager - Starting Device Manager with
    /nodes/DevMgr_localhost.localdomain/DeviceManager.dcd.xml
INFO:DomainManager - Starting ORB!
INFO:DeviceManager_impl - Connecting to Domain Manager
    MY_DOMAIN/MY_DOMAIN
INFO:DeviceManager - Starting ORB!
>>> INFO:DCE:9d9bcc38-d654-43b1-8b74-1dc024318b6f:Registering
    Device
INFO:DeviceManager_impl - Registering device GPP_localhost_
    localdomain on
    Device Manager DevMgr_localhost.localdomain
INFO:DeviceManager_impl - Initializing device GPP_localhost_
    localdomain on
    Device Manager DevMgr_localhost.localdomain
INFO:DeviceManager_impl - Registering device GPP_localhost_
    localdomain on
    Domain Manager

>>> domain
<ossie.utils.redhawk.core.Domain object at 0x2da6710>
```

The ability to search for domains is also available through the scan function which searches the [Naming Service]({{< relref "manual/glossary/_index.md#naming-service" >}}).

```py
>>> redhawk.scan()
['MY_DOMAIN']
```

### Domain State

The domain proxy tracks the current state of the domain. For aspects of the domain state that may take a long time to process, scanning is deferred until the first access. If the odm Channel is available, devices, Device Managers, and waveforms are tracked as they are added to and removed from the domain. Otherwise, the domain is re-scanned on access to ensure that the state is accurate.

#### Applications

The domain proxy provides a list of waveforms currently running via the `apps` attribute:

```py
>>> domain.apps
[<ossie.utils.redhawk.core.App object at 0x<hex address>>, ...]
```

The `App` class supports many of the Sandbox interfaces, such as properties, `connect()` and `api()`.

The components within the waveform are available via the `comps` attribute:

```py
>>> app = domain.apps[0]
>>> app.comps
[<ossie.utils.redhawk.component.Component object at 0x<hex address>>, ...]
```

The REDHAWK module `component` class supports the same interfaces as [Sandbox]({{< relref "manual/sandbox/_index.md" >}}) components. Much like in the Sandbox, ports, properties, and any other feature of a component is equally accessible from the Python environment, regardless of how the component was deployed.

For example, if one were to want to plot the output of a component running on the Domain:

```py
>>> from ossie.utils import redhawk, sb
>>> dom=redhawk.attach()
>>> for app in dom.apps:
...  if app.name == "my_app":
...   break

>>> for comp in app.comps:
...  if comp.name == "my_comp":
...   break

>>> plot = sb.LinePlot()
>>> plot.start()
>>> comp.connect(plot)
```

#### Device Managers

The domain proxy provides a list of Device Managers registered with the domain via the `devMgrs` attribute:

```py
>>> domain.devMgrs
[<ossie.utils.redhawk.core.DeviceManager object at 0x<hex address>>, ...]
```

Each Device Manager maintains a list of its devices, accessible via the `devs` attribute:

```py
>>> domain.devMgrs[0].devs
[<ossie.utils.redhawk.device.ExecutableDevice object at 0x<hex address>>, ...]
```

#### Devices

The domain proxy provides a list of all devices registered with the domain via the `devices` attribute:

```py
>>> domain.devices
[<ossie.utils.redhawk.device.ExecutableDevice object at 0x<hex address>>, ...]
```

The device list is the concatenation of the devices for all Device Managers.

The REDHAWK module `device` class supports the same interfaces as Sandbox devices.

### Using With the Sandbox

As shown briefly in section [Applications](#applications), Sandbox and domain objects are inter-operable and can be connected together. This allows for inspection of different parts of the domain and more sophisticated testing of components.

For example, to use a Sandbox plot to view the data coming from a device, enter the following commands:

```py
>>> from ossie.utils import sb
>>> plot = sb.LinePlot()
>>> domain.devices[1].connect(plot)
>>> plot.start()
```

Use the following commands to capture an approximately one second cut from a waveform to a file:

```py
>>> import time
>>> sink = sb.FileSink("/data/tuner_out.dat")
>>> domain.apps[0].connect(sink, usesPortName="tuner_out")
>>> sink.start()
>>> time.sleep(1.0)
>>> sink.stop()
```

When the Sandbox exits, any connections made between Sandbox objects and domain objects are broken to limit interference with normal domain operation.

For more details on using the Sandbox, refer to [Sandbox]({{< relref "manual/sandbox/_index.md" >}}).
