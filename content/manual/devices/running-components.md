---
title: "Using Devices to Run Components"
weight: 30
---

The Sandbox runs components without needing a device proxy; it forks the component process and manages its lifecycle. When running in a domain, however, the deployment of components in an application requires the domain to search for available host computers that can run the components in the application. The search requires each host computer to have a program that can publicize the host computer’s capabilities (for example, operating system, processor type, or available memory). This proxy is referred to as an Executable device. REDHAWK includes a default Executable device, the General Purpose Processor (GPP), which is automatically configured by the `create_node.py` script whenever a new node is installed on the host computer. GPP is written in C++ and can serve any node that supports x86 32 and 64-bit architecture. In cases where a more specialized proxy is required, REDHAWK includes base classes that can be extended in Python or C++. However, a detailed discussion of this process is beyond the scope of this document.
