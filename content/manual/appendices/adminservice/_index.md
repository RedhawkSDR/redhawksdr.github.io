---
title: "REDHAWK System Services"
weight: 55
---

For system integrators, REDHAWK provides an optional RPM, `redhawk-adminservice`, with the necessary scripts and configuration files to manage the REDHAWK core services: Domain Manager, Device Manager (includes devices and services), and waveforms. The RPM is part of the REDHAWK yum repository, but it is not installed by default.

The AdminService allows the REDHAWK core services and waveforms to be configured in a central place and retrieve the running status of the system. The `rhadmin` client script interacts with the AdminService to allow a user in the `redhawk` user group to control the lifecycle of these core services and waveforms. Custom scripts can be used to more accurately determine if a core service or waveform is running properly or to get a more detailed status of their operation.

Linux support files with more appropriate values for kernel tuning, process limits and logging rotation and configuration are provided for a better performing REDHAWK system.

For more information on the AdminService and Linux support files, refer to  

- [AdminService]({{< relref "manual/appendices/adminservice/adminservice/_index.md" >}})  
- [Linux Support Files]({{< relref "manual/appendices/adminservice/support-files.md" >}})
