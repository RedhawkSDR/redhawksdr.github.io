---
title: "Devices"
weight: 30
---

Devices in the Sandbox support all of the features of components, plus additional features specific to devices. A Sandbox device instance always supports the base `CF::Device` allocation and deallocation interfaces. If the device supports the `CF::LoadableDevice`, `CF::ExecutableDevice` or `CF::AggregateDevice` interfaces, the methods for those interfaces are also available.

#### Capacity Allocation

The Sandbox provides a simplified interface for capacity allocation and deallocation. The `allocateCapacity()` and `deallocateCapacity()` methods can take a Python dictionary of allocation property names and values. The values are automatically converted to the correct data type in the same manner as configure properties.

The following code demonstrates allocation and deallocation of multiple properties, including a struct property:

 ```py
>>> caps = {"long_cap": 1000,
...         "float_cap": 1.0,
...         "struct_cap": {"ushort_item": 0,
...                        "bool_item": False}}
>>> my_dev.allocateCapacity(caps)
True
>>> my_dev.deallocateCapacity(caps)
```

If an allocation is successful, `allocateCapacity()` returns `True`; if the device does not have sufficient capacity, it returns `False`.

#### Allocation Properties

The `api()` method for devices shows the allocation properties in addition to the ports and configure properties. The names, types and actions of the allocation properties are given:

```py
>>> my_dev.api()
Component [MyDevice]:
Provides (Input) Ports ==============
Port Name       Port Interface
---------       --------------
None

Uses (Output) Ports ==============
Port Name       Port Interface
---------       --------------
None

Properties ==============
Property Name   (Data Type)     [Default Value] Current Value
-------------   -----------     --------------- -------------
config_prop     (float/SF/32f)  1.0             1.0
DeviceKind      (string)        MyDevice        MyDevice
long_cap        (long)          None            1000
float_cap       (float)         None            100.0

Allocation Properties ======
Property Name   (Data Type)     Action
-------------   -----------     ------
DeviceKind      (string)        eq
long_cap        (long)          external
float_cap       (float)         external
struct_cap      (struct)        external
  ushort_item   (ushort)
  bool_item     (boolean)
```

Only properties with an action of “external” may be used for the `allocateCapacity()` and `deallocateCapacity()` methods.
