---
title: "Domain Manager Service Configuration File"
weight: 30
---

The REDHAWK Domain Manager service is controlled by files in the `/etc/redhawk/domains.d` directory. Default configuration parameters are stored in `/etc/redhawk/init.d/domain.defaults`.

[rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}) can generate an example Domain Manager configuration file with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK Domain Manager service. To generate a generic Domain Manager configuration, enter the following command.
```sh
rhadmin config domain > domain.ini
```
To generate a domain configuration from an existing Domain Manager project, enter the following command.
```sh
rhadmin config domain <path/to/domain>/DomainManager.dmd.xml <optional DomainName> > domain.ini
```

## Configuration Parameters

{{% notice note %}}
Parameter names are case sensitive.  
The following are the valid values for boolean configuration parameters. If no value is present, the feature is disabled.  
True: 1, true, True  
False: 0, false, False
{{% /notice %}}


parameter: `DOMAIN_NAME`  
required: Yes  
default value: None  
format: Name with no spaces or periods (for example, `REDHAWK_DEV`)  
description: Domain name to be assigned to the Domain Manager process

parameter: `DMD_FILE`  
required: No  
default: `/domain/DomainManager.dmd.xml`  
format: `$SDRROOT/dom` relative path to a DMD file  
description: Path to the DMD file (`DomainManager.dmd.xml` file) describing the Domain Manager

parameter: `FORCE_REBIND`  
required: No  
default value: `False` (no rebind)  
format: `False` : no rebind, `True` : rebind  
description: If the naming context already exists for the `DOMAIN_NAME`, rebinds the Domain Manager to an existing naming context in the CORBA NamingService

parameter: `PERSISTENCE`  
required: No  
default value: False (no persistence)  
format: `True` : domain persistence enabled, `False` : disabled  
description: Enables persistence for the domain (requires REDHAWK to be compiled with persistence)

parameter: `DB_FILE`  
required: No  
default value: None  
description: Absolute path to the file to use for domain persistence (for example, `/data/mysqlite.db`) (requires REDHAWK to be compiled with persistence enabled)

parameter: `BINDAPPS`  
required: No  
default value: blank  
format: `True` : enables option, blank: disables option  
description: All running applications and components will bind to the Domain Manager process instead of the NamingService. This assists with high frequency deployments of waveforms and components.

parameter: `USELOGCFG`  
required: No  
default value: None  
format: `True` : enables option, blank : disables option  
description: Enables the use of `$OSSIEHOME/lib/libsossielogcfg.so` to resolve `LOGGING_CONFIG_URI` command line argument

parameter: `LOGGING_CONFIG_URI`  
required: No  
default value: `defaults.logging.properties`  
format: Absolute path to a file, file://\<path\> URI or sca://\<path\> URI  
description: Logging configuration file to be used by the Domain Manager (Simple file names will be resolved to files in `/etc/redhawk/logging` directory. All others will be resolved as an absolute path or URI to a logging properties files.)

parameter: `DEBUG_LEVEL`  
required: No  
default value: `INFO`  
values: `FATAL`, `ERROR`, `WARN`, `INFO`, `DEBUG`, `TRACE`  
description: Domain Manager’s logging level at startup

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Standard shell path environment variable
description: Absolute path to use as the `SDRROOT` for this Domain Manager

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Standard shell path environment variable  
description: Absolute path to use as the `OSSIEHOME` for this Domain Manager

parameter: `LD_LIBRARY_PATH`  
required: No  
default value: `$LD_LIBRARY_PATH`  
format: Standard shell path environment variable  
description: Path for loader to resolve shared object files; overrides `LD_LIBRARY_PATH` environment variable

parameter: `PYTHONPATH`  
required: No  
default value: `$PYTHONPATH`  
format: Standard shell path environment variable
description: Path used by python interpreter to load modules; overrides `PYTHONPATH` environment variable

parameter: `ORB_CFG`  
required: No  
default value: None  
format: Standard shell environment variable
description: Used to set `OMNIORB_CONFIG` variable before running the process (Consult omniORB documentation for further details.)

parameter: `ORB_INITREF`  
required: No  
default value: None  
description: Used as omniORB `ORBInitRef` command line argument when starting process (Consult omniORB documentation for further details.)

parameter: `ORB_ENDPOINT`  
required: No  
default value: None  
description: Used as omniORB `ORBendPoint` command line argument when starting process (Consult omniORB documentation for further details.)

parameter: `enable`  
required: No  
default value: `True`  
format: `True`, `False`, or a string to be matched against `conditional_config`  
description: Specifies if process may be started. `True` or `False` will enable or disable the process. See `conditional_config` below for how a string value gets evaluated.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of processes based on the `enable` parameter and the contents of this conditional config. If the value `enable` is a string, the process will start only if there is a line in the `conditional_config` file that has that exact content; otherwise, the process is skipped. For example, `enable="type=primary"` causes the `conditional_config` file to be examined for a line equal to `type=primary` when starting a process on the host. If there is no `type=primary` line in the file, the process is skipped.

parameter: `priority`  
required: No  
default value: `100`  
description: Priority of this domain relative to other configured domains (Controls which domain gets started first on the system. Lower values will be started earlier (for example, priority 10 will be started before priority 100)).

parameter: `autostart`  
required: No  
default value: `True`  
description: Automatically start this process when the AdminService starts, if `enable` is True

parameter: `startsecs`  
required: No  
default value: `5`  
description: Number of seconds the Domain Manager has to be running to consider it started

parameter: `waitforprevious`  
required: No  
default value: `1`  
description: Number of seconds to wait for the previous higher priority process to start before trying to start this process

parameter: `failafterwait`  
required: No  
default value: `False`  
description: Abort starting this domain if `waitforprevious` has expired and the previous process has not been declared started yet

parameter: `startretries`  
required: No  
default value: `0`  
description: Number of times to try restarting the Domain Manager on startup

parameter: `autorestart`  
required: No  
default value: `False`  
description: Automatically restart this process when an abnormal termination is detected. Abnormal is any exit code not in `exitcodes`

parameter: `exitcodes`  
required: No  
default value: `0,2`  
description: Expected exit codes from the process

parameter: `stopwaitsecs`  
required: No  
default value: `10`  
description: Number of seconds to wait when stopping the Domain Manager

parameter: `started_status_script`  
required: No  
default value: None  
description: Path to file to be run to determine if the Domain Manager started properly

parameter: `status_script`  
required: No  
default value: None  
description: Path to file to be run to get the status for the Domain Manager

parameter: `environment`  
required: No  
default value: None  
format: A list of key/value pairs in the form `key="value",key2="value2"`  
description: Override existing environment variables or set new ones to be used when starting the Domain Manager

parameter: `user`  
required: No  
default value: `redhawk`  
description: Executes process with User ID

parameter: `group`  
required: No  
default value: `redhawk`  
description: Executes process with Group ID

parameter: `umask`  
required: No  
default value: None  
description: umask for the process

parameter: `nicelevel`  
required: No  
default value: None  
description: Runs the process using `nice` specifying this niceness level

parameter: `affinity`  
required: No  
default value: None  
description: Enables `numactl` processing (Any valid NUMA control directives will be passed on command line when starting the process. Consult `numactl` documentation for further details.)

parameter: `corefiles`  
required: No  
default value: None  
description: The maximum size of core files created (This value is passed to the `ulimit` command using the `-c` flag when starting the process.)

parameter: `ulimit`  
required: No  
default value: User’s environment  
description: Passes this value directly to the `ulimit` command when starting the process (Consult the `ulimit` documentation for further details.)

parameter: `directory`  
required: No  
default value: `$SDRROOT`  
description: Change directory to `directory` before starting the process

parameter: `run_detached`  
required: No  
default value: `True`  
description: Run the Domain Manager as a daemon, not a child of the AdminService process

parameter: `logfile_directory`  
required: No  
default value: `/var/log/redhawk/domain-mgr`  
description: Absolute path to the logging directory

parameter: `stdout_logfile`  
required: No  
default value: `<domain name>.stdout.log`  
description: Name of a file that captures the stdout from the process (If not specified, the default value list above is used.)

parameter: `stderr_logfile`  
required: No  
default value: `<domain name>.stderr.log`  
description: If `redirect_stderr` is False, name of a file that captures the stderr from the process. If not specified, the default value list above is used.

parameter: `redirect_stderr`  
required: No  
default value: `True`  
description: Write stdout and stderr to the same file

parameter: `stopsignal`  
required: No  
default value: `TERM`  
description: Signal to send when stopping the Domain Manager

parameter: `stopasgroup`  
required: No  
default value: `False`  
description: Send stop signal to the process group when stopping

parameter: `killasgroup`  
required: No  
default value: `False`  
description: Send SIGKILL to the process group when forcibly killing the Domain Manager

parameter: `start_pre_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: Script or directory of scripts to be run before the process is started

parameter: `start_post_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: Script or directory of scripts to be run after the process is started

parameter: `stop_pre_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: Script or directory of scripts to be run before the process is stopped

parameter: `stop_post_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: Script or directory of scripts to be run after the process is stopped
