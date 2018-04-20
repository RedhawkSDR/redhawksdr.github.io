---
title: "DomainManager Configuration"
weight: 20
---

The REDHAWK DomainManager service is controlled by files in the `/etc/redhawk/domains.d` directory. The [rhadmin]({{< relref "manual/appendices/adminservice/_index.md#rhadmin-client" >}}) can generate an example DomainManager configuration with the complete set of parameters that can be used to control the setup and execution of a REDHAWK DomainManager.

The following section describes all the available configuration parameters for the DomainManager process. System-wide default configuration settings can be stored in `/etc/redhawk/init.d/domain.defaults`.

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
description: Domain name to be assigned to the DomainManager process.

parameter: `DMD_FILE`  
required: No  
default: `/domain/DomainManager.dmd.xml`  
format: $SDRROOT/dom relative path to a DMD file.  
description: Fully qualified path to a DMD file (`DomainManager.dmd.xml` file).

parameter: `FORCE_REBIND`  
required: No  
default value: 0 (no rebind)  
format: false : no rebind, true : rebind  
description: If the naming context already exists for the `DOMAIN_NAME`, rebinds the DomainManager to an existing naming context in the NamingService.

parameter: `PERSISTENCE`  
required: No  
default value: False (no persistence)  
format: true enables persistence option, false disables  
description: Enables persistence for the domain. Requires REDHAWK to be compiled with persistence.

parameter: `DB_URL`  
required: No  
default value: None  
format: Absolute path to a database file.  
description: URL to database file (for example, `/data/mysqlite.db`). Requires REDHAWK to be compiled with persistence.

parameter: `USELOGCFG`  
required: No  
default value: None  
format: true : enables option, blank: disables option  
description: Enables the use of `$OSSIEHOME/lib/libsossielogcfg.so` to resolve <br>`LOGGING_CONFIG_URI` command line argument. For more information, refer to .

parameter: `BINDAPPS`  
required: No  
default value: blank  
format: true : enables option, blank: disables option  
description: All running Applications and Components will bind to the DomainManager Process instead of the NamingService. This assists with high frequency deployments of Waveforms and Components.

parameter: `LOGGING_CONFIG_URI`  
required: No  
default value: `defaults.logging.properties`  
format: Absolute path to a file or file name.  
description: Logging configuration file for DomainManager to use. Simple file names are resolved to files in `/etc/redhawk/logging` directory. All others, are resolved as a qualified path to a logging properties files.

parameter: `DEBUG_LEVEL`  
required: No  
default value: INFO  
format: FATAL, ERROR, WARN, INFO, DEBUG, TRACE  
description: DomainManager’s logging level at startup.

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Absolute path to the SDR directory.  
description: Path to use as the `SDRROOT` for this DomainManager.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Absolute path to the REDHAWK installation directory.  
description: Path to use as the `OSSIEHOME` for this DomainManager.

parameter: `LD_LIBRARY_PATH`  
required: No  
default value: User’s environment  
format: Standard shell path environment variable.  
description: Path for loader to resolve shared object files, overrides `LD_LIBRARY_PATH` environment variable.

parameter: `PYTHONPATH`  
required: No  
default value: User’s environment  
format: Standard shell path environment variable.  
description: Path used by python interpreter to load modules, overrides `PYTHONPATH` environment variable.

parameter: `ORB_CFG`  
required: No  
default value: None  
format: Standard shell environment variable.  
description: Used to set `OMNIORB_CONFIG` variable before running the process. Consult omniORB documentation for further details.

parameter: `ORB_ENDPOINT`  
required: No  
default value: None  
description: Set the endpoint definition for the DomainManager process. Can be used with persistence enabled. Consult omniORB documentation for further details.

parameter: `ORB_INITREF`  
required: No  
default value: none  
description: Used as omniORB ORBInitRef command line argument when starting process.  
(For example, `NameService=corbaname::127.0.0.1:2809` <br>`EventService=corbaloc::127.0.0.1:11169/omniEvents` would override orb config files for specific service locations). Consult omniORB documentation for further details.

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

parameter: `autostart`  
required: No  
default value: True  
description: Automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: 5  
description: Number of seconds the DomainManager has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: 1  
description: Number of seconds to wait for the previously ordered process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: False  
description: Abort starting this domain if `waitforprevious` has expired and the previous process hasn't been declared started yet.

parameter: `startretries`  
required: No  
default value: 0  
description: Number of times to try restarting the DomainManager on startup.

parameter: `autorestart`  
required: No  
default value: False  
description: Automatically restart this process when an abnormal termination is detected. Abnormal is any exit code not in `exitcodes`.

parameter: `exitcodes`  
required: No  
default value: 0,2  
description: Expected exit codes from the process.

parameter: `stopwaitsecs`  
required: No  
default value: 10  
description: Number of seconds to wait when stopping the DomainManager.

parameter: `stopsignal`  
required: No  
default value: TERM  
description: Signal to send when stopping the DomainManager.

parameter: `stopasgroup`  
required: No  
default value: False  
description: Send stop signal to the unix group when stopping.

parameter: `killasgroup`  
required: No  
default value: False  
description: Send SIGKILL to the unix group when forcibly killing the DomainManager.

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

parameter: `priority`  
required: No  
default value: 100  
description: Priority of this domain relative to other configured domains.  Controls which domain gets started first on the system.

parameter: `nicelevel`  
required: No  
default value: None  
description: Runs process using nicelevel. Consult nice documentation for further details.

parameter: `affinity`  
required: No  
default value: None  
description: Enables numactl processing, any valid numa control directives are passed on command line when starting the process. Consult numactl documentation for further details.

parameter: `corefiles`  
required: No  
default value: 0  
description: Controls core file generation from the REDHAWK Domain Service.

parameter: `ulimit`  
required: No  
default value: User’s enviroment  
description: Provide any additional ulimit settings before running the process. Consult ulimit documentation for further details.

parameter: `environment`  
required: No  
default value: None  
description: Provide additional environment variables to set before executing the service. Format is NAME=Value, for Value that contains spaces, it must be enclosed with double quotes.

parameter: `directory`  
required: No  
default value: `$SDRROOT`  
description: Change directory to `directory` before running service.

parameter: `run_detached`  
required: No  
default value: True  
description: Run the DomainManager as a daemon, not a child of the AdminService process.

parameter: `logfile_directory`  
required: No  
default value: `/var/log/redhawk/domain-mgr`  
description: Absolute path to the logging directory.

parameter: `stdout_logfile`  
required: No  
default value: `<domain name>.stdout.log`  
description: Name of a file that captures the stdout from the process. If not specified, the default value list above is used.

parameter: `stderr_logfile`  
required: No  
default value: `<domain name>.stderr.log`  
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
description: Path to file or directory to search for scripts to be run to determine if a DomainManager is started properly.

parameter: `status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to get the status for the DomainManager.
