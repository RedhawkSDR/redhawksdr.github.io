---
title: "Sandbox"
weight: 110
---

[Components can run]({{< relref "manual/components/running-a-component.md" >}}) in either the sandbox or in a domain. The Sandbox is an environment to run components and devices without artifacts like a domain, device manager, name service, or event service.

REDHAWK provides two sandboxes, a Python-based one or an IDE-based one. The Python-based sandbox can run from any Python session on any system with a REDHAWK install. The IDE-based sandbox can host an instance of a Python-based sandbox, with both interlinked, allowing artifacts from the Python environment to interact with those on the graphical UI.

{{% children depth="999" %}}
