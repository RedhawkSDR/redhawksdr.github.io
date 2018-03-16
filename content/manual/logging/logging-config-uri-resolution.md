---
title: "LOGGING_CONFIG_URI Resolution"
weight: 20
---

For all REDHAWK resources, the command line argument `LOGGING_CONFIG_URI` provides the location of the logging configuration file to load for that resource. This section details how each resource type receives this parameter.

  - **Domain Manager**: The Domain Manager is started via `nodeBooter` with the `-D` option. The additional `nodeBooter` command line argument `-logcfgfile` is passed to the Domain Manager as `LOGGING_CONFIG_URI`. The following lines describe acceptable parameter values and their resolution:
      - `sca://path/to/config/file`: $SDRROOT/dom/path/to/config/file
      - `file:///absolute/path/to/config/file`: /absolute/path/to/config/file

  - **Device Manager**: The Device Manager is started via `nodeBooter` with the `-d` option. The additional `nodeBooter` command line argument `-logcfgfile` is passed to the Device Manager as `LOGGING_CONFIG_URI`. The following lines describe acceptable parameter values and their resolution:
      - `sca://path/to/config/file`: $SDRROOT/dev/path/to/config/file
      - `file:///absolute/path/to/config/file`: /absolute/path/to/config/file

  - **Device**: A device is started by the Device Manager. If the device has a `LOGGING_CONFIG_URI` property with **Pass on command line** enabled, you can set the value via the node’s `dcd.xml` file. This value is passed to the device at startup; otherwise, the device receives the value from the Device Manager’s `LOGGING_CONFIG_URI` command line argument.

  - **Service**: A service is started by the Device Manager. If the service has a `LOGGING_CONFIG_URI` property with **Pass on command line** enabled, you can set the value via the node’s `dcd.xml` file. This value is passed to the service at startup; otherwise, the service receives the value from the Device Manager’s `LOGGING_CONFIG_URI` command line argument.

  - **Component**: When a component is deployed by the Domain Manager, the `LOGGING_CONFIG_URI` command line parameter is resolved by the following sequence: (each step in the sequence can override the value from the prior step)
      - The component defines a `LOGGING_CONFIG_URI` property with **Pass on command line** enabled.
      - The component’s `componentplacement` in a waveform contains a `simpleref` with ID = `LOGGING_CONFIG_URI`. Normally, only defined properties are passed to the component during deployment, the `LOGGING_CONFIG_URI` is passed regardless of being defined.
      - The component’s `componentplacement` contains a `<loggingconfig>` element.
      - The Domain Manager’s is configured with a plugin library and the `-useloglib` option was provided when the Domain Manager was started. Refer to [Logging Configuration Plugin]({{< relref "manual/appendices/logging-config-plugin.md" >}}) for details.
      - If this process does not provide a value, then the Domain Manager’s `LOGGING_CONFIG_URI` command line is used.
