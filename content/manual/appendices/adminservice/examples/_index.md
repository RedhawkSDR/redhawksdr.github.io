---
title: "Examples"
weight: 80
---

## Setup
The following examples assume a default REDHAWK and AdminService install and are based on the following configuration:

| Type     | Name        | Quantity |
| :------- | :---------- | :------- |
| Domain   | REDHAWK_DEV | 1        |
| Node     | GppNode     | 1        |
| Waveform | Wave        | 1        |

There must be a Device Manager named `GppNode` and a waveform named `Wave` installed in `$SDRROOT`.

### Domain Manager Configuration
File: `/etc/redhawk/domains.d/domain.ini`
```
[domain:REDHAWK_DEV_mgr]
DOMAIN_NAME=REDHAWK_DEV
```

### Device Manager Configuration
File: `/etc/redhawk/nodes.d/node.ini`
```
[node:GppNode]
DOMAIN_NAME=REDHAWK_DEV
NODE_NAME=GppNode
```

### Waveform Configuration
File: `/etc/redhawk/waveforms.d/wave.ini`
```
[waveform:Wave]
DOMAIN_NAME=REDHAWK_DEV
WAVEFORM=Wave
```
