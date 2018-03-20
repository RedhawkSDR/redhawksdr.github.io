---
title: "Getting Started"
weight: 20
---

### Installation

Installation of the REDHAWK Core Framework and IDE is handled through a set of RPMs. The use of RPMs allows for automated installation of dependencies required for REDHAWK to run as well as automated installation of the Core Framework and IDE. Greater detail on REDHAWK installation can be found in the [REDHAWK manual]({{< relref "manual/_index.md" >}}).

### Basic Example

The fastest and easiest way to get a REDHAWK application started is through the REDHAWK sandbox, a self contained Python module that is able to run REDHAWK components outside the runtime environment, which limits the components to a single host computer.

In order to do this, first bring up a command line terminal. Begin a Python session and import the sandbox module by typing:

```py
from ossie.utils import sb
```

The Sandbox has commands to list components available for use and to create an instance of a component by passing its name as an argument:

```py
sb.catalog()
```

- Output:

    ```py
    ['rh.HardLimit', 'rh.SigGen']
    ```

```py
sigGen = sb.launch("rh.SigGen")
hardLimit = sb.launch("rh.HardLimit")
```

The REDHAWK IDE plotting tool can be used in the Sandbox to display data graphically. The path to the Eclipse directory of the installed IDE must be specified in the sandbox (this can also be done by setting the RH_IDE environment variable to the REDHAWK path prior to starting the python session):

```py
sb.IDELocation("/path/to/ide/eclipse")
plot = sb.Plot()
```

The two components need to be connected, and HardLimit must be connected to the plotter to display the results.

```py
sigGen.connect(hardLimit)
hardLimit.connect(plot)
```

Once the HardLimit component is connected to the plotter, a window appears that plots any data coming from the output port. Once the sandbox is started, the plot begins to display data:

```py
sb.start()
```

Component properties can be modified as attributes of the component. The HardLimit property upper_limit can be changed to set an upper limit on the data that is displayed:

```py
hardLimit.upper_limit = .8
```

For further reading on application development, the component and sandbox chapters of the REDHAWK manual provide a more thorough description of the makeup of components and how to test them in the REDHAWK environment.
