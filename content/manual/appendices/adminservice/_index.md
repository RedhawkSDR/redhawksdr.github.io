---
title: "REDHAWK System Services"
weight: 55
---

REDHAWK provides an optional RPM, `redhawk-adminservice`, to manage the REDHAWK core services (Domain Manager, Device Managers, and waveforms). These core services are managed as Linux system services using INI configuration files. The RPM is part of the REDHAWK yum repository; however, it is not installed by default.  Additional support files are included in the RPM to assist with tuning common operating system parameters and configuring the REDHAWK logging subsystem.

For more information about the AdminService and Linux support files, refer to the following:

- [AdminService]({{< relref "manual/appendices/adminservice/adminservice/_index.md" >}})  
- [Linux Support Files]({{< relref "manual/appendices/adminservice/support-files.md" >}})
