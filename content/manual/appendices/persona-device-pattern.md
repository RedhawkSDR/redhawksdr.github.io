---
title: "REDHAWK Persona Device Pattern"
weight: 70
---

In REDHAWK, you can manage and maintain the lifecycle of programmable devices, such as FPGAs, by implementing a specific design pattern: the Persona Pattern. This design pattern is rooted heavily in the concept of REDHAWK devices, proxies used to interface physical hardware with the REDHAWK Framework.

To accurately represent the dynamic nature of programmable hardware, two unique roles have been established: the Programmable role and the Persona role. These roles are represented in REDHAWK as two separate REDHAWK devices: Programmable Devices and Persona Devices.

### Potential Benefits

Persona Devices provide the following benefits:

  - Enables the REDHAWK Framework to manage and maintain the lifecycle of programmable hardware.
  - Ability to add/remove programmable hardware loads without the need to modify/rebuild source.
  - Reduced development time and costs.
  - Support for reuse and portability.
  - Scalability across embedded, lightweight, and heavyweight architectures.

## Theory of Operation

### The Programmable and Persona Roles

The Programmable role and the Persona role attempt to model the polymorphic behavior of programmable hardware within the REDHAWK Framework. The Programmable role can be thought of as a controller that may also provide generic functionality, whereas the Persona role can be thought of as a standalone representation of a single hardware load. This delineation provides two distinct areas to accurately define functionality and assign responsibilities.

#### The Programmable Role

The Programmable role has a handful of responsibilities associated with it, but most important are its controller responsibilities. The Programmable role has the ability to control which persona has permission to load the programmable hardware while also blocking subsequent personas from attempting to re-program loaded, running hardware. This simple controller role may also be extended to contain generic functionality that is common among all personas or that does not apply to the Persona role.

#### The Persona Role

The Persona role is responsible for defining the hardware load and any interfaces pertaining to that specific load. This may include register definitions and/or data IO channels. The Persona role supplies the bulk of the control over the programmable hardware, representing the state of the loaded programmable hardware.

### REDHAWK Devices

The Programmable and Persona roles can be implemented via REDHAWK devices. Representing these roles as their own independent devices allows for more granular control and for a more precise representation of each role without needing to shoe-horn the two independent roles together into a single, complex entity.

##### Relationship between Programmable Devices and Persona Devices
![personadiagram](../../images/personadiagram.png)

#### REDHAWK Programmable Device

The Programmable role may be implemented in REDHAWK as a standard REDHAWK executable device. The Device Manager is responsible for launching this device, following the typical REDHAWK device lifecycle. What makes the programmable device unique is that it provides hooks for maintaining and managing the lifecycle of the persona devices registered to it.

REDHAWK programmable devices also implement functionality to instantiate REDHAWK persona devices from shared libraries. This is achieved by using `dlopen` to load the shared library and `dlsym` to access/instantiate the device object contained within the shared library. This mechanism occurs when the following requirements are met:

  - Programmable/Persona devices are associated together using the `compositepartofdevice` tag in DCD file.
  - Aggregate (parent) device is an executable device.
  - Composite (child) device is a shared library.

#### REDHAWK Persona Device

A REDHAWK persona device is simply the XML representation of one specific behavior that may be assigned to a programmable device. This XML representation may include unique properties and/or ports that are only relevant to the persona. Each persona device is visible in the REDHAWK domain, which allows for run-time configuration of persona devices in a standardized way.

REDHAWK persona devices may be built as shared objects rather than executables, therefore, allowing persona devices to exist within the same process space as a programmable device.

##### Dynamic Loading of Persona Devices onto a Programmable Device
![persona_load](../../images/persona_load.png)

### Associating Programmable/Persona Devices

Programmable devices and persona devices are linked together via the REDHAWK DCD files. Within these DCD files, the aggregate device relationship tag defines which persona devices should be associated to a programmable device. The advantage of describing this relationship within the DCD file comes with the added ability to dynamically add/remove/ modify the possible personalities of programmable hardware without the need to rebuild source.

#### Aggregate Persona Device

In the SCA 2.2.2, aggregate devices are defined as devices that are loosely coupled together within a parent-child relationship. The extent of this relationship is left up to the developer and the Persona Pattern attempts to take full advantage of this loose coupling.

Because a single piece of programmable hardware can take on many personalities, the programmable device is said to have a one-to-many relationship with its loads. The DCD file, via the aggregate relationship tags, enables users to properly define this one-to-many relationship between a single programmable device and many available persona devices.

#### DCD File Example

In the following example DCD file, the `compositepartofdevice` tag is used to associate `TestPersonaChild1` and `TestPersonaChild2` to `PersonaParent1`.

```xml
...
<componentfiles>
  <componentfile id="test_persona_parent" type="SPD">
    <localfile name="/devices/programmable/programmable.spd.xml"/>
  </componentfile>
  <componentfile id="test_persona_child" type="SPD">
    <localfile name="/devices/persona_device/persona_device.spd.xml"/>
  </componentfile>
</componentfiles>
<partitioning>
  <componentplacement>
    <componentfileref refid="test_persona_parent"/>
    <componentinstantiation id="persona_node:persona_parent_1">
      <usagename>persona_parent_1</usagename>
    </componentinstantiation>
  </componentplacement>
  <componentplacement>
    <componentfileref refid="test_persona_child"/>
    <compositepartofdevice refid="persona_node:persona_parent_1"/>
    <componentinstantiation id="persona_node:persona_child_1">
      <usagename>test_persona_child_1</usagename>
    </componentinstantiation>
  </componentplacement>
  <componentfileref refid="test_persona_child"/>
    <compositepartofdevice refid="persona_node:persona_parent_1"/>
    <componentinstantiation id="persona_node:persona_child_2">
      <usagename>test_persona_child_2</usagename>
    </componentinstantiation>
  </componentplacement>
</partitioning>
...
```

### Hardware-Accelerated Components

Hardware-accelerated components are REDHAWK components that are proxies into only a portion of hardware. An example for FPGAs is having multiple demodulator components within the fabric that all have their own register sets and data IO channels. Each individual register set and/or data IO channel may be represented within a single REDHAWK component that has a dependency on a specific behavior or persona.

The pattern used for hardware-accelerated components is very similar to the pattern used for persona devices. These components may be built as shared libraries and share state the same way Programmable and Persona devices share state.

##### Dynamic Loading of Hardware-Accelerated Components to a Persona Device
![full_load](../../images/full_load.png)

## Code-Generation Support

The persona code-generation support includes templates for both the persona device and the programmable device. These templates have been setup to allow for the easy redefinition of the entry-point method to share state from device-to-device as well as device-to-component. The code generation provides the following benefits:

  - Provides C++ and Python templates for Persona/Programmable devices and harware-accelerated components.
  - Allows for custom definition of the entry-point method to pass state and/or other parameters from device-to-device as well as device-to-component.
  - Generic shared library functionality tucked into base classes for ease of development.
  - Manages and maintains generic functionality between device-to-device and device-to-component behavior.

## Persona Pattern Development

### Programmable Device Development

Developing REDHAWK programmable devices is based heavily on interfacing with a manufacturer’s driver/API and/or custom drivers/APIs. If the driver/API is to be shared, the programmable device may be responsible for the initial construction/opening of the driver/API and the destruction/closing at the end of the lifecycle. The REDHAWK programmable device must also include properties that exist on that specific hardware (for example, temperatures) and any allocation properties that may exist (for example, tuner cards using FEI 2.0). Any persona device that attempts to reserve the hardware may configure/allocate these properties to put the device into a known usable state for that persona.

### Persona Device Development

Developing persona devices uses the code-generation support to abstract away the hooks and functionality used to associate persona devices with their parent programmable device.

Persona devices, in theory, must contain any ports and/or properties that are strictly related to the hardware personality. These ports and properties must be unique to the load and must not overlap with the programmable device. The persona device may then choose to access the driver/API as needed for operations such as reading/writing registers.

### System Developers

System developers may assign the programmable device behavior on the fly while also maintaining persona sets for a specific programmable device dynamically. The relationship definition between persona devices and programmable devices is defined at the node level within the DCD file. With programmable devices and persona devices defined, a system developer may chose to create/modify DCD files to dynamically add/remove/modify the personas that may be loaded onto the programmable hardware without the need to generate/build source.

## When to Use the Persona Pattern

Persona devices are useful for programming hardware dynamically but may not be ideal for every programmable device situation. The following cases describe some common scenarios and explain why the Persona Pattern may or may not be practical for the scenarios.

  - **Case 1:** You have a single FPGA that you want to always operate as X.
    - **Solution:** Develop a single REDHAWK device without persona devices. In this scenario, a single device is adequate enough to represent both the hardware and the programmable behavior of the hardware. We are essentially treating the hardware and programmable behavior as a single unit because that behavior is static.

  - **Case 2:** You have a single fpga that you want to operate like X, Y, and Z, where X, Y, and Z do not exhibit any of the same behavior.
    - **Solution:** Develop a REDHAWK programmable device that interfaces with the hardware and develop X, Y, and Z REDHAWK persona devices that represent the unique behaviors. Using persona X, Y, and Z allows for all differences between X, Y, and Z to be represented properly while also allowing generic functionality to be retained within the programmable device.

  - **Case 3:** You have a single fpga that you want to operate like X1, X2, and X3, where X1, X2, and X3 exhibit common behavior.
    - **Solution:** There are multiple solutions and the best solution depends on the specific use-case and whether the behaviors are similar. Some possible solutions include:
      - Solution 1: Develop a REDHAWK programmable device that interfaces with the hardware and develop a REDHAWK persona device for each behavior. This requires the most amount of effort but will yield the most amount of flexibility if the behaviors wind up not being as similar as originally expected
      - Solution 2: Develop a single REDHAWK device that interfaces with the hardware and maps the differences in X1, X2, and X3 behavior via the standard REDHAWK properties. This simplest solution can quickly yield "spaghetti" code if the house-keeping becomes more complex.
      - Solution 3: Develop a REDHAWK programmable device that interfaces with the hardware and develop a dynamic REDHAWK persona device that can be configured to represent X1, X2, and/or X3. This approach allows the REDHAWK programmable device to maintain its appropriate role while offloading the housekeeping code to the dynamic REDHAWK persona device. This option also lends itself to adding additional REDHAWK persona devices further down the road to include a new behavior that was not originally intended.

## Sharing Hardware Driver/API

When using a single driver/API, it is possible to pass a single instance of the driver/API to each persona device and/or hardware-accelerated component. The programmable device is the layer closest to the physical hardware and is typically in charge of opening/instantiating the driver/API.

The REDHAWK Framework allows for Softpkg dependencies, packages used to distribute shared libraries/headers amongst numerous resources. The Persona Pattern does not mandate the use of Softpkg, but it may be beneficial to create a REDHAWK Softpkg dependency for the driver/API libraries/headers instead of manually including/linking headers/libraries.

Once the headers and libraries are shared, it is possible to share a single instance of the library/API by using one of the following options.

### Option 1: Modifying the `construct` Method

Because persona devices are shared objects, you can modify the entry-point method into those shared objects and append any additional arguments. The following procedure explains how to modify the `construct` method.

1.  For the programmable device, update any entry-point `typedefs` and pass in additional arguments to the entry-point method.
2.  For the persona devices, update the extern C `construct` method as follows:

```c++
extern "C" {
  Device_impl* construct(int argc,
             char* argv[],
             Device_impl*  myParent,
             MyNewArg1Ptr* myNewArg1,
             MyNewArg2Ptr* myNewArg2){
    .
    .
    // Standard construct logic
    .
    .

    // Use myNewArg1 and myNewArg2 here
    devicePtr->setMyArg1(myNewArg1);
    devicePtr->setMyArg2(myNewArg2);
  }
}
```

### Option 2: Defining a Programmable Device Interface

The following procedure explains how to define a programmable device interface.

1.  Create a programmable device interface that exposes desired state.
2.  Include the interface with the persona devices and cast the parent reference into the interface.
3.  Update the persona device entry-point `construct` method as follows:

```c++
extern "C" {
  Device_impl* construct(int argc,
             char* argv[],
             Device_impl* myParent) {
    .
    .
    // Standard construct logic
    .
    .

    // Cast Programmable Device into interface
    Interface myDevice = static_cast<Interface>(myParent);

    // Give Programmable Device interface to Persona
    devicePtr->setMyDeviceInterface(myDevice);
  }
}
```
