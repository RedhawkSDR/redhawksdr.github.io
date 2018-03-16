---
title: "Python Sandbox"
weight: 10
---

The Python-based Sandbox is a Python package that is imported as any other Python package. Included in the Python package are tools that provide a means for passing [BulkIO]({{< relref "manual/appendices/acronyms#bulkio" >}}) data to and from components or devices. Plotting is also supported from the Python package. This section discusses how to instantiate and test a component using the provided Sandbox tools.

The Sandbox is Python-centric, so its use requires some basic Python knowledge.

### Setup

In order to run the Sandbox, REDHAWK must be installed and the `OSSIEHOME` and `SDRROOT` environment variables must be [set correctly]({{< relref "manual/appendices/source-installation.md#installing-the-framework-from-source" >}}).

To use the Sandbox *none* of the following programs need to be running: omniNames, omniEvents, Domain Manager, Device Manager, nodeBooter

The Python-based Sandbox includes basic analytical plotting tools based on the matplotlib Python plotting library. To use these plots, the following Python packages must be installed:

  - `matplotlib`
  - `PyQt4`

### Starting the Sandbox

The Python-based Sandbox, like any other Python module, may be used in another Python program or run directly within the Python interpreter. The following examples assume that the Sandbox is being used within the Python interpreter.

To begin, start a Python interpreter session and import the Sandbox:

```py
>>> from ossie.utils import sb
```

As with any other Python module, the built-in functions `help()` and `dir()` are useful when searching for available commands.

```py
>>> help(sb)
>>> dir(sb)
```

To exit the Sandbox, press `Ctrl+D` at the Python prompt. On exit, the Sandbox terminates and cleans up all of the components and helpers it creates.

### Running the Sandbox

Within the Sandbox, components and helpers are launched in an idle state. In order to begin processing (e.g., `serviceFunction()` in C++ components), each object must be started.

After the desired components and helpers are created, the `start()` method starts all registered objects:

```py
>>> sb.start()
```

It is not necessary to stop the Sandbox prior to exiting, as this is handled as part of normal exit cleanup. However, if you wish to stop processing, the `stop()` method stops all registered objects:

```py
>>> sb.stop()
```

In non-interactive scripts, the Python interpreter exits after the last statement. This may be undesirable, such as in the case where a set of components should run for an arbitrary amount of time, with their output monitored via plots. The built-in Python method `raw_input()` can be used to prevent the interpreter from exiting:

```py
>>> raw_input()
```

{{% notice note %}}
When using plots, calling `raw_input()` instead of `time.sleep()` allows the UI thread to continue updating.
{{% /notice %}}

Items launched in the Sandbox are registered in the Sandbox’s internal state. To view the items that are deployed in the Sandbox, use the `show` command:

 ```py
 >>> sb.show()
 ```

 The items shown by the `show` command are referenced by a unique name. To recover an item by its Sandbox internal name, use the `getComponent` function:

 ```py
 >>> comp = sb.getComponent("name")
 ```

#### Connecting to a Running Domain

 The Python Sandbox allows a developer to not only launch components, but it also allows one to interact with a live Domain. This is accomplished through the [redhawk package]({{< relref "manual/runtime-inspection/_index.md#redhawk-module" >}}).
