---
title: "Launching the REDHAWK IDE for the First Time"
weight: 10
---

This section describes the basic process for starting the REDHAWK IDE for the first time.

{{% notice note %}}
Before starting the REDHAWK IDE for the first time, the REDHAWK Core Framework and the IDE must be installed ([Installation]({{< relref "manual/installation/_index.md#installing-redhawk-from-rpms" >}})).
{{% /notice %}}

1.  Start the REDHAWK IDE by entering the following command:

```bash
rhide
```

At startup, the IDE may prompt for a workspace location. The workspace stores many of the IDE’s settings and also acts as a logical collection of projects under development. The IDE creates new projects in the workspace by default.

{{% notice warning %}}
If you upgraded from 1.8.x, 1.9.x, or 1.10.x, it is recommended that you use a different workspace location rather than reusing a previous version’s workspace. If you set the workspace to the same workspace that you used in 1.8.x, 1.9.x, or 1.10.x, you must delete the domain from REDHAWK Explorer before launching the domain from Target SDR.
{{% /notice %}}
