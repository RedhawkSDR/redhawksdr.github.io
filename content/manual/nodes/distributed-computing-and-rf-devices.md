---
title: "Distributed Computing and RF Devices"
weight: 20
---

### Dependencies

A component can be described as [consuming an arbitrary amount of capacity from a device]({{< relref "manual/nodes/running-a-node.md#creating-a-component-that-consumes-resources">}}); this relationship is called a **dependency**. Dependencies are generalized, so it is possible to create a dependency based on attributes (properties of kind allocation) of devices irrespective of the specific nature of a device.

A dependency is defined by how much of a particular device resource is required. For example, a component dependency could be a requirement of 1.7 units of device property **some_id**. Testing this description requires the runtime environment to only determine whether a device contains an allocation property with the ID **some_id**; this concept can be generalized to any physical constraint that a device might have, requiring only a convention between the component developer and the device developer regarding a device’s property ID and units, both of which are publicized in a device’s PRF  file (`prf.xml`).

### Allocation Properties

Allocation properties are device properties that are associated with an action and are used by the runtime environment to determine whether or not a device can support a particular component dependency. This action can either be **external** or some logical operation (e.g., **eq** (equals) or **lt** (less than)).

#### Non-External Properties

Non-external properties are evaluated by the runtime environment without querying the device. For example, a component dependency on an allocation property with operation **eq** might be *blue*. The runtime environment then checks the device’s PRF file (`prf.xml`) and the Device Manager’s configuration file for the deployment of that device (`dcd.xml`) to determine whether that property’s value is equal to *blue*.

#### External Properties

External properties, on the other hand, are handled by the device. For example, a component dependency on an allocation property with operation **external** might be *yellow*. The runtime environment then makes a function call on that device with two arguments: the allocation property ID and the value *yellow*. The device then, at its own discretion, determines whether to provide a positive response or a negative one. The algorithm used by the device to determine whether or not it has sufficient capacity to allocate against a value of *yellow* is left up to the device developer.

### Allocation Usage

Capacity allocation can be defined at either the component level or the application level. When defining the capacity allocation at a component level, that means that every time this component is deployed, that capacity must be available. When defining the capacity allocation at a application level, that means that every time that this application is deployed, that capacity must be available, irrespective of which components make up the application.

Even though capacity allocation can be used to describe any dependency that a component may have, in reality there are two kinds of capacities that concern REDHAWK: [RF]({{< relref "manual/appendices/acronyms.md#rf" >}}) and computing.

#### RF Allocation

The allocation of RF capacities is with properties that are defined as part of the [FrontendInterfaces]({{< relref "manual/appendices/fei.md">}}). It generally makes more sense to define the RF capacity allocation at the application level rather than the component level, since the application understands the context for the deployment. However, RF capacity dependencies can be declared at either the component level or the application level.

#### Computing Allocation

Determining capacity for computation is different from determining RF needs. RF needs are fairly straightforward (i.e.: I need to tune to 100 MHz with a 200 kHz bandwidth). Computing resources, on the other hand, can vary substantially. A program (i.e.: a component) consumes different amount of computing resources depending on the nature of the computing platform. Things like L2 cache size and the layout of the processor (from a [NUMA]({{< relref "manual/glossary/_index.md#numa" >}}) perspective) can have a dramatic impact on the computational load a program has on the processor.

Due to the variability intrinsic to computing resources, REDHAWK does not rely on hardcoded estimates for the selection of a computing platform. Instead, [GPP]({{< relref "manual/glossary/_index.md#gpp" >}}) implements a reservation mechanism. When a component is loaded onto the GPP, the GPP reserves an arbitrary, and tunable, amount of resources. GPP tracks the state of the component, and when the component changes state from stopped to started, the reservation is tabled, and GPP inspects the actual usage by the component. When the state of the component changes from started to stopped, the tabled reservation is applied again.

Using this reservation/observation behavior, components can be automatically distributed over an arbitrary number of computers with no planning needed on the part of the user.

The user can force components to deploy to specific GPPs. To force a specific placement, use the device assignment fields when creating the application.

#### The Deployment Process

The runtime environment scans all non-busy executable devices registered in the domain to determine which GPP matches the processor/operating system dependency. A device is busy when its state is returned as **BUSY**, otherwise it is either **IDLE** or **ACTIVE**. When a device is found that satisfies all component dependencies, it marks that device as assigned to the deployment of that component and moves on to whatever other components make up the waveform. Once all components have been found an assigned GPP, the components are deployed to all those GPPs.

#### Distribution of Files

All component binaries and descriptor files reside in `$SDRROOT/dom` on whatever host is running the Domain Manager. In every host that is running a Device Manager, a cache directory is created in `$SDRROOT/dev/.<Device Manager name>`, with a directory entry for each device that the Device Manager manages. When a component runs on any given device, the binary (or module or [JAR]({{< relref "manual/appendices/acronyms.md#jar" >}}) file) is copied from `$SDRROOT/dom` to the device’s cache directory. Using this mechanism, the device can start the component process on a remote host.

#### Dependency Management

A difficulty in deploying a component is that, as a program, it might have dependencies like C/C++ libraries, Python modules, or Java JARs. REDHAWK allows for the creation of soft package dependencies, where a library, module, or JAR can be associated with its own profile. Components that have a runtime dependency with this library, module, or JAR, can declare this library profile as a dependency. When the component is loaded over the network, this dependency is also loaded, and before the component is forked, the component’s local running environment is changed to include the library in `$LD_LIBRARY_PATH`, module in `$PYTHONPATH`, or JAR in `$CLASSPATH`. The allocation/dependency requirements associated with the component are also applied to the library; for example, if a component is designed to run on an x86_64 platform, the runtime environment checks that dependency runs on an x86_64 platform.
