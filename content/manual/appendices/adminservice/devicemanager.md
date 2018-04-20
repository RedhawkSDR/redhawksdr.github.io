---
title: "DeviceManager Configuration"
weight: 30
---

The REDHAWK DeviceManagers are controlled by files in the `/etc/redhawk/nodes.d` directory. The [rhadmin]({{< relref "manual/appendices/adminservice/_index.md#rhadmin-client" >}}) can generate an example DeviceManager configuration with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK DeviceManager.

The following section describes all the available configuration parameters for the DeviceManager process. System-wide default configuration settings can be stored in `/etc/redhawk/init.d/node.defaults`.

## Configuration Parameters

{{% notice note %}}
 For configuration parameters that are controlled with boolean constructs (True or False), the following values can used or no value will disable the feature:  
True: 1 or true  
False: 0 or false
{{% /notice %}}


parameter: `DOMAIN_NAME`  
required: Yes  
default value: None  
format: Name with no spaces or periods (for example, `REDHAWK_DEV`).  
description: Domain name to associate with this DeviceManager.

parameter: `NODE_NAME`  
required: Yes  
default value: None  
format: Name with no spaces or periods.  
description: Name of the Node to launch with this DeviceManager. Must be a valid directory name in `$SDRROOT/dev/nodes`.

parameter: `DCD_FILE`  
required: No  
default: `$SDRROOT/dev/nodes/$NODE_NAME/DeviceManager.dcd.xml`  
format: Absolute path to a DCD file.  
description: Absolute path to a DCD file (`DeviceManger.dcd.xm`l file).

parameter: `SDRCACHE`  
required: No  
default value: None  
format: Absolute path to use as cache directory for DeviceManager and its devices.  
description: Absolute path to use as cache directory for DeviceManager and its devices.

parameter: `SPD`  
required: No  
default value: `$SDRROOT/dev/mgr/DeviceManager.spd.xml`  
format: Absolute path to file the `DeviceManager.spd.xml` file. description: Absolute path to the DeviceManager’s SPD file.

parameter: `CLIENT_WAIT_TIME`  
required: No  
default value: 10000 (milliseconds)  
format: A number description: Wait time, in milliseconds, for the DeviceManager when making remote calls.

parameter: `USELOGCFG`  
required: No  
default value: None  
format: true : enables option, blank: disables option  
description: Enables the use of `$OSSIEHOME/lib/libsossielogcfg.so` to resolve <br>`LOGGING_CONFIG_URI` command line argument.

parameter: `LOGGING_CONFIG_URI`  
required: No  
default value: `defaults.logging.properties`  
format: Absolute path to a file or file name.  
description: Logging configuration file for DeviceManager to use. Simple file names will be resolved to files in `/etc/redhawk/logging` directory. All others, will be resolved as a qualified path to a logging properties files.

parameter: `DEBUG_LEVEL`  
required: No  
default value: INFO  
format: FATAL, ERROR, WARN, INFO, DEBUG, TRACE  
description: DeviceManager’s logging level at startup.

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Absolute path to the SDR directory.  
description: Path to use as the `SDRROOT` for this DeviceManager.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Absolute path to the REDHAWK installation directory.  
description: Path to use as the `OSSIEHOME` for this DeviceManager.

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

parameter: `ORB_ENDPOINT`  
required: No  
default value: None  
description: Set the endpoint definition for this process. Consult omniORB documentation for further details.

parameter: `ORB_INITREF`  
required: No  
default value: None  
description: Used as omniORB ORBInitRef command line argument when starting process. (For example, `NameService=corbaname::127.0.0.1:2809` <br>`EventService=corbaloc::127.0.0.1:11169/omniEvents` would override orb config files for specific service locations.) Consult omniORB documentation for further details.

parameter: `ORB_ENDPOINT`  
required: No  
default value: None  
description: Used as omniORB ORBendPoint command line argument when starting process.  
(For example, `giop:tcp:127.0.0.1:2809` would override orb config files for the specific endpoint to listen.) Consult omniORB documentation for further details.

parameter: `enable`  
required: No  
default value: True  
format: true (enabled) or false (disabled) (see exception with `conditional_config`)  
description: Specifies if service is started.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of services based on the `enable` parameter and the contents of this conditional config file’s `enable` field. If the value of both `enable` parameters are the same, the service will start; otherwise, the service is skipped. For example, `enable=primary` causes the `conditional_config` file to be examined for an `enable=primary` statement to match against when starting a service on the host. If no match is found, the service is skipped.

parameter: `priority`  
required: No  
default value: 400  
description: Relative priority of the DeviceManager in the group of processes to start for this domain.

parameter: `autostart`  
required: No  
default value: True  
description: Automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: 5  
description: Number of seconds the DeviceManager has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: 45  
description: Number of seconds to wait for the previously ordered process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: True  
description: Abort starting this domain if `waitforprevious` has expired and the previous process hasn't been declared started yet.  This is useful to make sure the DomainManager is started before launching the DeviceManager.

parameter: `startretries`  
required: No  
default value: 0  
description: Number of times to try restarting the DeviceManager on startup.

parameter: `autorestart`  
required: No  
default value: True  
description: Automatically restart this process when an abnormal termination is detected. Abnormal is any exit code not in `exitcodes`.

parameter: `exitcodes`  
required: No  
default value: 0,2  
description: Expected exit codes from the process.

parameter: `stopwaitsecs`  
required: No  
default value: 10  
description: Number of seconds to wait when stopping the DeviceManager.

parameter: `stopsignal`  
required: No  
default value: TERM  
description: Signal to send when stopping the DeviceManager.

parameter: `stopasgroup`  
required: No  
default value: False  
description: Send stop signal to the unix group when stopping.

parameter: `killasgroup`  
required: No  
default value: False  
description: Send SIGKILL to the unix group when forcibly killing the DeviceManager.

parameter: `environment`  
required: No  
default value: None  
format: KEY='value',KEY2='value2'  
description: Additional environment variables to add before starting the DeviceManager.

parameter: `user`  
required: No  
default value: redhawk  
description: Executes process with User ID.

parameter: `group`  
required: No  
default value: redhawk  
description: Executes process with Group ID.

parameter: `umask`  
required: No  
default value: None  
description: umask for the process.

parameter: `nicelevel`  
required: No  
default value: None  
description: Runs process using nicelevel.

parameter: `affinity`  
required: No  
default value: None  
description Enables numactl processing, any valid numa control directives will be passed on command line when starting the process.  Consult numactl documentation for further details.

parameter: `corefiles`  
required: No  
default value: 0  
description: Controls core file generation from the REDHAWK DeviceManager Service.

parameter: `ulimit`  
required: No  
default value: user’s enviroment  
description: Provide any additional ulimit settings before running the process.

parameter: `environment`  
required: No  
default value: None  
description: Provide additional environment variables before executing service. Format is NAME=Value, for Value that contain spaces it must be enclosed with double quotes.

parameter: `directory`  
required: No  
default value: `$SDRROOT`  
description: Change directory to `directory` before running this service.

parameter: `run_detached`  
required: No  
default value: True  
description: Run the DeviceManager as a daemon, not a child of the AdminService process.

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
description: If $redirect_stderr is False, name of a file that captures the stderr from the process. If not specified, the default value list above is used.

parameter: `redirect_stderr`  
required: No  
default value: True  
description: Write stdout and stderr to the same file.

parameter: `start_pre_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run before the service is started.

parameter: `start_post_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run after the service is started.

parameter: `stop_pre_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run before the service is stopped.

parameter: `stop_post_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run after the service is stopped.

parameter: `started_status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to determine if a service is started properly.

parameter: `status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to get the status for the service.
