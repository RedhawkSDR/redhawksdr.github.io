---
title: "Device Manager Service Configuration"
weight: 30
---

The REDHAWK Device Manager service is controlled by files in the `/etc/redhawk/nodes.d` directory. Default configuration parameters are stored in `/etc/redhawk/init.d/node.defaults`. The following section describes all the available configuration parameters for the Device Manager process.

The [rhadmin]({{< relref "manual/appendices/adminservice/_index.md#rhadmin-client" >}}) can generate an example Device Manager configuration file with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK Device Manager service. To generate a generic Device Manager configuration, use the following command.
```sh
rhadmin config node > node.ini
```
To generate a node configuration from an existing Device Manager project, use the following command.
```sh
rhadmin config node <path/to/node>/DeviceManager.dmd.xml <optional DomainName> > node.ini
```

## Configuration Parameters

{{% notice note %}}
Parameter names are case sensitive.  
The following are the valid values for boolean configuration parameters. No value will disable the feature.  
True: 1, true, True  
False: 0, false, False
{{% /notice %}}


parameter: `DOMAIN_NAME`  
required: Yes  
default value: None  
format: Name with no spaces or periods (for example, `REDHAWK_DEV`).  
description: Domain name to associate with this Device Manager.

parameter: `NODE_NAME`  
required: Yes  
default value: None  
format: Name with no spaces or periods.  
description: Name of the Node to launch with this Device Manager. Must be a valid directory name in `$SDRROOT/dev/nodes`.

parameter: `DCD_FILE`  
required: No  
default: `$SDRROOT/dev/nodes/$NODE_NAME/DeviceManager.dcd.xml`  
description: Absolute path to a DCD file (`DeviceManger.dcd.xml` file).  

parameter: `SDRCACHE`  
required: No  
default value: None  
description: Absolute path to use as cache directory for Device Manager and its devices. If no value is specified, the system defaults to making a directory in `$SDRROOT/dev`.  

parameter: `SPD`  
required: No  
default value: `$SDRROOT/dev/mgr/DeviceManager.spd.xml`  
format: Absolute path to the `DeviceManager.spd.xml` file.  
description: Absolute path to the Device Manager’s SPD file.  

parameter: `CLIENT_WAIT_TIME`  
required: No  
default value: `10000` (milliseconds)  
format: number in milliseconds  
description: Wait time, in milliseconds, before the Device Manager times out waiting for a response when making remote calls.  

parameter: `USELOGCFG`  
required: No  
default value: None  
format: `True` : enables option, blank : disables option  
description: Enables the use of `$OSSIEHOME/lib/libsossielogcfg.so` to resolve `LOGGING_CONFIG_URI` command line argument.  

parameter: `LOGGING_CONFIG_URI`  
required: No  
default value: `defaults.logging.properties`  
format: Absolute path to a file, file://\<path\> URI or sca://\<path\> URI.  
description: Logging configuration file for the Device Manager to use. Simple file names will be resolved to files in `/etc/redhawk/logging` directory. All others will be resolved as an absolute path or URI to a logging properties files.

parameter: `DEBUG_LEVEL`  
required: No  
default value: `INFO`  
values: `FATAL`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `TRACE`  
description: Device Manager’s logging level at startup.

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Absolute path to the SDR directory.  
description: Path to use as the `SDRROOT` for this Device Manager.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Absolute path to the REDHAWK installation directory.  
description: Path to use as the `OSSIEHOME` for this Device Manager.

parameter: `LD_LIBRARY_PATH`  
required: No  
default value: User’s environment  
format: Standard shell path environment variable.  
description: Path for link loader to resolve shared object files, overrides `LD_LIBRARY_PATH` environment variable.

parameter: `PYTHONPATH`  
required: No  
default value: User’s environment  
description: Path used by Python interpreter to load modules, overrides `PYTHONPATH` environment variable.

parameter: `JAVA_HOME`  
required: No  
default value: User’s environment  
description: JAVA path to use when launching Devices and Services.

parameter: `PATH`  
required: No  
default value: User’s environment  
format: Standard shell path environment variable.  
description: Search path to use when launching Devices and Services.

parameter: `ORB_CFG`  
required: No  
default value: None  
format: Standard shell environment variable.  
description: Used to set `OMNIORB_CONFIG` variable before running the process. Consult omniORB documentation for further details.

parameter: `ORB_INITREF`  
required: No  
default value: None  
description: Used as omniORB ORBInitRef command line argument when starting process. Consult omniORB documentation for further details.

parameter: `ORB_ENDPOINT`  
required: No  
default value: None  
description: Used as omniORB ORBendPoint command line argument when starting process. Consult omniORB documentation for further details.

parameter: `enable`  
required: No  
default value: `True`  
format: `True`, `False`, or a string to be matched against `conditional_config`  
description: Specifies if process is able to be started.  `True` or `False` will enable or disable the process. See `conditional_config` below for how a string value gets evaluated.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of processs based on the `enable` parameter and the contents of this conditional config file’s `enable` field. If the value of both `enable` parameters are the same, the process will start; otherwise, the process is skipped. For example, `enable=primary` causes the `conditional_config` file to be examined for an `enable=primary` statement to match against when starting a process on the host. If no match is found, the process is skipped.

parameter: `priority`  
required: No  
default value: `400`  
description: Relative priority of the Device Manager in the group of processes to start for this domain. Lower values will be started earlier (for example priority 100 will be started before priority 400).

parameter: `autostart`  
required: No  
default value: `True`  
description: Automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: `5`  
description: Number of seconds the Device Manager has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: `45`  
description: Number of seconds to wait for the previous higher priority process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: `True`  
description: Abort starting this domain if `waitforprevious` has expired and the previous process hasn't been declared started yet.  This is useful to make sure the Domain Manager is started before launching the Device Manager.

parameter: `startretries`  
required: No  
default value: `0`  
description: Number of times to try restarting the Device Manager on startup.

parameter: `autorestart`  
required: No  
default value: `True`  
description: Automatically restart this process when an abnormal termination is detected. Abnormal is any exit code not in `exitcodes`.

parameter: `exitcodes`  
required: No  
default value: `0,2`  
description: Expected exit codes from the process.

parameter: `stopwaitsecs`  
required: No  
default value: `10`  
description: Number of seconds to wait when stopping the Device Manager.

parameter: `started_status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to determine if a process is started properly.

parameter: `status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to get the status for the process.

parameter: `environment`  
required: No  
default value: None  
format: A list of key/value pairs in the form `key="value",key2="value2"`.  
description: Override existing environment variables or set new ones to be used when starting the Device Manager.

parameter: `user`  
required: No  
default value: `redhawk`  
description: Executes process with User ID.

parameter: `group`  
required: No  
default value: `redhawk`  
description: Executes process with Group ID.

parameter: `umask`  
required: No  
default value: None  
description: umask for the process.

parameter: `nicelevel`  
required: No  
default value: None  
description: Runs the process using `nice` specifying this niceness level.

parameter: `affinity`  
required: No  
default value: None  
description: Enables `numactl` processing, any valid NUMA control directives will be passed on command line when starting the process.  Consult `numactl` documentation for further details.

parameter: `corefiles`  
required: No  
default value: None  
description: The maximum size of core files created. This value is passed to the `ulimit` command using the `-c` flag when starting the process.  

parameter: `ulimit`  
required: No  
default value: user’s environment  
description: Passes this value directly to the `ulimit` command when starting the process. Consult the `ulimit` documention for further details.

parameter: `directory`  
required: No  
default value: `$SDRROOT`  
description: Change directory to `directory` before running this process.

parameter: `run_detached`  
required: No  
default value: `True`  
description: Run the Device Manager as a daemon, not a child of the AdminService process.

parameter: `logfile_directory`  
required: No  
default value: `/var/log/redhawk/device-mgr`  
description: Absolute path to the logging directory.

parameter: `stdout_logfile`  
required: No  
default value: `<domain name>.<node name>.stdout.log`  
description: Name of a file that captures the stdout from the process. If not specified, the default value list above is used.

parameter: `stderr_logfile`  
required: No  
default value: `<domain name>.<node name>.stderr.log`  
description: If `redirect_stderr` is False, name of a file that captures the stderr from the process. If not specified, the default value list above is used.

parameter: `redirect_stderr`  
required: No  
default value: `True`  
description: Write stdout and stderr to the same file.

parameter: `stopsignal`  
required: No  
default value: `TERM`  
description: Signal to send when stopping the Device Manager.

parameter: `stopasgroup`  
required: No  
default value: `False`  
description: Send stop signal to the unix group when stopping.

parameter: `killasgroup`  
required: No  
default value: `False`  
description: Send SIGKILL to the unix group when forcibly killing the Device Manager.

parameter: `start_pre_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run before the process is started.

parameter: `start_post_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run after the process is started.

parameter: `stop_pre_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run before the process is stopped.

parameter: `stop_post_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run after the process is stopped.
