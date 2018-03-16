---
title: "Console View"
weight: 120
---

The **Console** view is a part of Eclipse and the basic use is well documented by the [Eclipse documentation](http://help.eclipse.org/oxygen/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fviews%2Fconsole%2Fref-console_view.htm). The REDHAWK IDE uses multiple consoles for different purposes.

The primary consoles found in the REDHAWK IDE are:

  - **Domain & Device Manager**: When a domain or Device Manager is launched, a new **Console View** is created. These new consoles contain an instance of a running `nodeBooter`. By default, log messages, standard out, and standard error messages for components and devices are written to the Device Manager console. Domain Manager related messages, including waveform creation and resource allocation are written to the Domain Manager’s console.

  - **C++ builds**: Compiling a C++ device or component creates a C-build console, which runs the component/device build script. While Eclipse does a relatively good job of parsing build output and providing in-context error indicators, it is sometimes helpful to view the output of the build from the console.

  - **Code Generators**: When automatically generating code for a component or device, the appropriate code generator console is displayed and contains information pertaining to the code generation.

  - **Python Sandbox**: The [Python Sandbox]({{< relref "manual/sandbox/python/_index.md" >}}) is a powerful tool for testing and interacting with components and devices. A Python Sandbox may be created from within the IDE to aid in testing.

  - **Other Consoles**: Additional consoles (not described in this section) are available, including PyDev Scripting, Java Stack Trace, etc. These are part of Eclipse or part of third party plug-ins such as PyDev.
