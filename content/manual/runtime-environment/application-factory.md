---
title: "The Application Factory"
weight: 50
---

The Application Factory is responsible for the creation of applications within a domain. Whenever an application is installed by the Domain Manager, an Application Factory is created from tags in the application’s SAD file, in order to deploy components of the application to devices based on their implementation dependencies.

When the `create()` function is called, the Application Factory uses the SPD `implementation` element to locate devices that are capable of loading and executing the given component. The Application Factory does this by first assembling a list of all of the allocation properties required by the components that make up its application. It then searches through each of the candidate devices for properties whose `kindtype` is `allocation` and `action` is not `external`. It attempts to use that devices `allocateCapacity()` function in order to compare the requested capacities with the devices available resources.

The creation of the application fails if the Application Factory is unable to deploy all of the components in compliance with the components’ dependencies and host-collocation requirements given the available devices.

Once the resource marshaling has been successfully completed, the filemanager copies the appropriate component files into the specific Device Manager’s File System and the Application Factory performs the `load()` and `execute()` operations in order to launch the component on its assigned device. It then continues to initialize, connect and configure the components. properties can also be overridden from the `componentproperties` tags in the waveform’s SAD file.
