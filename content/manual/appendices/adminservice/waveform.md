---
title: "Waveform Configuration"
weight: 40
---

REDHAWK Waveforms are controlled by files in the `/etc/redhawk/waveforms.d` directory. The [rhadmin]({{< relref "manual/appendices/adminservice/_index.md#rhadmin-client" >}}) can generate an example Waveform configuration with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK Waveform.

Default service configuration parameters are stored in `/etc/redhawk/init.d/waveform.defaults`. The following section describes all the available configuration parameters when launching a REDHAWK Waveform.

## Configuration Parameters

{{% notice note %}}
Parameter names are case sensitive.
For boolean configuration parameters, the following values can used or no value will disable the feature:  
True: 1, true, True  
False: 0, false, False
{{% /notice %}}


parameter: `DOMAIN_NAME`  
required: Yes  
default value: None  
format: A name with no spaces or periods (for example, `REDHAWK_DEV`).  
description: Domain name to associate with this Waveform.

parameter: `WAVEFORM`  
required: Yes  
default value: None  
format: Valid directory under `$SDRROOT/dom/waveforms`.  
description: Name of the Waveform to launch (for example, `rh.MyWaveform`)

parameter: `INSTANCE_NAME`  
required: No  
format: A name with no spaces or periods (for example, `REDHAWK_DEV`)  
description: Used to rename the Waveform when launching in a Domain.

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Absolute path to the SDR directory.  
description: Path to use as the SDRROOT for this Waveform.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Absolute path to the REDHAWK installation directory.  
description: Path to use as the OSSIEHOME for this Waveform.

parameter: `PYTHONPATH`  
required: No  
default value: User’s environment  
description: Path used by Python interpreter to load modules, overrides `PYTHONPATH` environment variable.

parameter: `start_delay`  
required: No  
default value: 30  
description: Amount of time for the waveform control script to wait for the Domainmanager to be available before failing startup.

parameter: `start_cmd_option`  
required: No  
default value: `-o create`  
description: Option to use with the waveform control script to start the waveform.

parameter: `status_cmd_option`  
required: No  
default value: `-o status`  
description: Option to use with the waveform control script to status the waveform.

parameter: `stop_cmd_option`  
required: No  
default value: `-o remove`  
description: Option to use with the waveform control script to stop the waveform.

parameter: `enable`  
required: No  
default value: True  
format: True (enabled) or False (disabled) (see exception with `conditional_config`)  
description: Specifies if service is started.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of services based on the `enable` parameter and the contents of this conditional config file’s `enable` field. If the value of both `enable` parameters are the same, the service will start; otherwise, the service is skipped. For example, `enable=primary` causes the `conditional_config` file to be examined for an `enable=primary` statement to match against when starting a service on the host. If no match is found, the service is skipped.

parameter: `priority`  
required: No  
default value: 900  
description: Relative priority of the Waveform in the group of processes to start for this domain. Lower values will be started earlier - priority 800 will be started before priority 900.

parameter: `autostart`  
required: No  
default value: True  
description: Automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: 5  
description: Number of seconds the waveform has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: 45  
description: Number of seconds to wait for the previously ordered process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: False  
description: Abort starting this domain if `waitforprevious` has expired and the previous process hasn't been declared started yet.  This is useful to make sure the DeviceManager is started and all devices are ready before launching the waveform.

parameter: `startretries`  
required: No  
default value: 0  
description: Number of times to try restarting the waveform on startup.

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
description: Number of seconds to wait when stopping the waveform.

parameter: `stopsignal`  
required: No  
default value: TERM  
description: Signal to send when stopping the waveform.

parameter: `stopasgroup`  
required: No  
default value: False  
description: Send stop signal to the unix group when stopping.

parameter: `killasgroup`  
required: No  
default value: False  
description: Send SIGKILL to the unix group when forcibly killing the waveform.

parameter: `environment`  
required: No  
default value: None  
format: KEY='value',KEY2='value2'  
description: Additional environment variables to add before starting the waveform.

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
description: Controls core file generation from the REDHAWK Waveform Service.

parameter: `ulimit`  
required: No  
default value: User’s environment  
description: Provide any additional ulimit settings before running the process.

parameter: `environment`  
required: No  
default value: none  
note: These values are available as a replacement(eg. `%(ENV_SDRROOT)s`) for most parameters in the UPPERCASE  
description: Provide additional environment variables to set before executing service. Format is NAME=Value, for Values that contain spaces, it must be enclosed with double quotes.

parameter: `directory`  
required: No  
default value:`$SDRROOT`  
description: Change directory to `directory` before running this waveform.

parameter: `run_detached`  
required: No  
default value: True  
description: Run the Waveform as a daemon, not a child of the AdminService process.

parameter: `logfile_directory`  
required: No  
default value: `/var/log/redhawk/waveforms`  
description: Absolute path to the logging directory.

parameter: `stdout_logfile`  
required: No  
default value: `<domain name>.<waveform name>.stdout.log`  
description: Name of a file that captures the stdout from the process. If not specified, the default value list above is used.

parameter: `stderr_logfile`  
required: No  
default value: `<domain name>.<waveform name>.stderr.log`  
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
description: Path to file or directory to search for scripts to be run to determine if a waveform is started properly.

parameter: `status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to get the status for the waveform.
