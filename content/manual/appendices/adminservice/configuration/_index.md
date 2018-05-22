---
title: "Configuration Files"
weight: 30
---

## REDHAWK Core Services

The Domain Manager and Device Manager configurations directly correspond to a process that runs on a system. On the other hand, the waveform configurations tell the AdminService how to interact with the running REDHAWK Domain to start and stop waveforms.

To create and start new configuration file, perform the following commands substituting `domain`, `node` or `waveform` for `<type>`.
```sh
cd /etc/redhawk/<type>s.d

# This will generate a generic configuration file
rhadmin config <type> > <type>.ini

vi <type>.ini
sudo service redhawk-adminservice restart
```
{{% notice note %}}
The configuration files reside in a system privileged directory. Ensure that you have proper privileges to create and edit files in those directories.
{{% /notice %}}
