---
title: "Linux Support Files"
weight: 90
---


## Linux Support Files

In addition to running REDHAWK core services, the following support files are provided for REDHAWK logging properties, managing logging output files, system limit definitions, and kernel setup.

The following table descibes the support files.

| **Support File**                                        | **Description**                                        |
| :------------------------------------------------------ | :----------------------------------------------------- |
| `/etc/cron.d/redhawk`                                   | Cron entry for root to run `logrotate` every 5 minutes with the configuration file `/etc/redhawk/logging/logrotate.redhawk`.             |
| `/etc/redhawk/logging/logrotate.redhawk`                | `logrotate` configuration file to manage logfiles generated from REDHAWK services. Rotates all files in `/var/log/redhawk/*.log` when size exceeds 100 MB. |
| `/etc/redhawk/logging/default.logging.properties`       | Default logging configuration for a REDHAWK system. Defines all messages of INFO level or higher to append to the console, which is captured by the Domain Manager and Device Manager services. |
| `/etc/redhawk/logging/example.logging.properties`       | Logging configuration with example appenders that are supported by the REDHAWK logging subsystem. |
| `/etc/redhawk/security/limits.d/99-redhawk-limits.conf` | Controls file and process limits associated with the `redhawk` group. |
| `/etc/redhawk/sysctl.d/sysctl.conf`                     | Common kernel tuning parameters for network buffers and core file generation. |
