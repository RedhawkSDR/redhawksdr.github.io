---
title: "Waveform Deployment and Computing Resources"
weight: 30
---

Components are processes that run on a computer. As such, each component takes up some arbitrary, often time-varying amount of spare capacity (for example, CPU computing load, memory, network I/O). REDHAWK manages computing resources on every computer under its domain to minimize the likelihood that computing resources are over-subscribed. Each computer under REDHAWK’s domain is managed through the GPP process.

### GPP Device

The GPP device is a specialized REDHAWK device that manages the deployment of components onto the computer and can be inspected like any other REDHAWK device.

GPP can be in three possible usage states: `IDLE`, `ACTIVE`, or `BUSY`.

  - `IDLE` indicates the GPP is not running any REDHAWK components.
  - `ACTIVE` indicates the GPP is running at least one component, but has spare capacity to run additional components.
  - `BUSY` indicates the GPP does not have any spare capacity to run additional components.

The usage state of any device can be accessed through its `usageState` member. To inspect the usage state:

  - In the IDE, select the “Advanced” tab of the “Properties” tab for the deployed device.
  - In a Python session, run the following script (assuming a domain is running):

    ```py
    >>> from ossie.utils import redhawk
    >>> dom=redhawk.attach()
    >>> dev = dom.devMgrs[0].devs[0]
    >>> print dev._get_usageState()
    ```

The GPP contains several properties that are critical for the deployment of components to a device. When a waveform is deployed, the framework scans through all `IDLE` and `ACTIVE` GPP devices. The component’s `os` and/or `processor` elements from its SPD file are compared against the GPP’s `os_name` and/or `processor_name` properties, respectively. If there is a match, the component is assigned to that GPP. This search/assignment is performed for all components in the waveform. Once all components have been assigned to specific GPPs, the deployment process begins.

#### Deployment Process

The deployment of a component to a GPP begins by copying all relevant files from `$SDRROOT/dom/components/<component>/` to the GPP’s cache. The GPP cache is a temporary directory on the computer used to run each component (recall that REDHAWK is designed to support distributed processing). The cache is located in the local computer’s `$SDRROOT/dev/.<Device Manager name>/<Device Name>`. Note that the component’s working directory is the GPP’s cache directory.

After all files are copied, the component is started in its own process space using the `fork` system call. At program startup, the component registers with the Domain Manager and the initial property state is applied to the component’s property values. Next, connections are created as defined in the waveform file. The waveform is successfully deployed when all waveform components are registered with the Domain Manager and all component initialization is complete.

#### Using Valgrind to Debug Components

The GPP supports launching components using Valgrind, an open source tool that helps detect memory errors and leaks.

The `VALGRIND` environment variable controls this behavior:

  - If `VALGRIND` is not set, components launch as usual.
  - If `VALGRIND` is set but has no value, the GPP searches the path for `valgrind`.
  - If `VALGRIND` has a value, it is assumed to be the full path to the `valgrind` executable.

Valgrind log files are written to the same directory as the component entry point within the GPP’s cache. For example, if the component, `MyComponent`, has an entry point of `cpp/MyComponent`, the logs are written to `MyComponent/cpp`. The log file name follows the pattern `valgrind.<PID>.log`, where PID is the process ID of the component.

### Monitoring Computing Resources

The GPP monitors the following resources:

  - System: CPU utilization, memory usage, nic usage, total number of threads, and number of open files
  - For each component: CPU usage, memory usage, and process state

When a component fails (for example, segfault), the GPP issues a `AbnormalComponentTerminationEventType` message on the `IDM_Channel`. The message contains the component ID of the component that failed, the application ID for the component’s waveform, and the device ID for the GPP that is running the component.

The GPP contains a `thresholds` structure with five elements. If at any point any of these thresholds is exceeded (because of any process’s usage, not just those forked by the GPP), the GPP usage state changes to `BUSY`.

The following table describes the elements of the `thresholds` structured property:

##### Thresholds Structured Property
| **Element**       | **Description**                                                                            |
| :---------------- | :----------------------------------------------------------------------------------------- |
| `mem_free`        | Amount of free memory available that triggers a threshold condition (default units in MB). |
| `nic_usage`       | Amount of network capacity used by the NIC interface that triggers a threshold condition.  |
| `threads`         | Percentage of threads available to the GPP that triggers a threshold condition.            |
| `files_available` | Percentage of file handles remaining to the GPP that triggers a threshold condition.       |
| `cpu_idle`        | Amount of CPU idle percentage that triggers a threshold condition.                         |


Setting any element of the `thresholds` structure to -1 disables the GPP’s response to that particular element. For example, to ignore the processor’s usage, set `thresholds.cpu_idle` to -1. The GPP state will be `IDLE` if no components are deployed, and `ACTIVE` if one or more component is deployed, irrespective of the CPU idle percentage.

Setting any element of the `thresholds` structure to 0 changes the GPP usage state to `BUSY`, and thus, the GPP cannot launch any addtional components.

The GPP contains a `utilization` structured property that shows the overall system utilization at any time. The following table describes the elements of the `utilization` structured property.

##### Utilization Structured Property
| **Element**      | **Description**                                                                                                                                                                                          |
| :--------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `description`    | Human-readable description of the content. In the case of CPU usage, this element’s content is “CPU cores”. The numbers in this structure reflect the effective number of cores that are being utilized. |
| `component_load` | Overall load by all the components that were forked by the GPP.                                                                                                                                          |
| `system_load`    | Total load from every process, irrespective of whether or not it was forked by this GPP.                                                                                                                 |
| `subscribed`     | Sum of the total system load and however much is reserved. (Refer to [Capacity Reservation](#capacity-reservation)).                                                                                                                                  |
| `maximum`        | Maximum load that the GPP will accept before switching to a BUSY state.                                                                                                                                  |

### Capacity Reservation

A non-reservation strategy assumes that each process is running at full capacity at the time of execution. This is clearly not the case when the waveform is initially deployed because all of the waveform’s components are in a `stopped` state, thus taking little to no system resources. Only after the waveform is switched to a `started` state do the components begin to consume system resources. Those system resources that are monitored for threshold changes by the GPP will directly affect the Device’s `usageState`: `IDLE`, `ACTIVE`, or `BUSY`.

A non-reservation strategy with a deployment pattern where multiple waveforms are launched and not initially started but are delayed due to system events or predefined behavior, can create an over-utilized system. For example, a system may initially create twenty instances of a waveform and later, start them as a result of an aperiodic event. All twenty waveforms are created, but starting the 15th instance causes the CPU utilization for the entire system to reach 100%. When the next five waveforms are started, the system will be over utilized, and all running components will be affected leading to degraded system performance, such as data loss or lack of timely response to message events.

To mitigate this issue, the GPP maintains a reservation-based strategy to properly forecast the CPU load for each component it manages. During initial waveform deployment, the forecasted load for each component of the waveform is reserved against the current system’s available CPU capacity. As components are moved to the `start` state, the forecasted load is tabled, and the actual load or a minimum utilitization load (whichever is greater), is used for determining the system load, and thus, the available CPU capacity. The available CPU capacity is one of the system resources that will directly determine the system’s `usageState`, (i.e., `IDLE`, `ACTIVE`, or `BUSY` ). The GPP’s `utilization.subscribed` property maintains the runtime value of the total reservation for the system.

The Property, `reserved_capacity_per_component`, is the default forecasted load for every component deployed by the GPP. A finer resolution forecast load can be provided when the waveform is created. Currently, this is only available through the REDHAWK Python package when the waveform is created. The following example describes how to set the reserved forecasted load for a component. In this example, the component, `my_comp_1`, on the waveform, `my_wave`, has a reservation floor of `4` CPU cores.

```py
from ossie.utils import redhawk
from ossie.cf import CF
from omniORB import any, CORBA
dom=redhawk.attach()
dev = dom.devMgrs[0].devs[0]
extra_reservation = 4  # number of cores to reserve/utilize
_value=any.to_any(extra_reservation)
_value._t=CORBA.TC_double  # make sure that the number is packed as a double
app_1=dom.createApplication('/waveforms/my_wave/my_wave.sad.xml','my_wave',[CF.DataType(id='SPECIALIZED_CPU_RESERVATION',value=any.to_any([CF.DataType(id='my_comp_1',value=any.to_any(_value))]))])
print dev._get_usageState()  # view the reservation effect
```
