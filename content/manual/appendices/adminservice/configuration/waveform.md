---
title: "Waveform Configuration"
weight: 40
---

REDHAWK waveforms are controlled by files in the `/etc/redhawk/waveforms.d` directory. Default waveform configuration parameters are stored in `/etc/redhawk/init.d/waveform.defaults`. To define multiple waveform instances in one file, add multiple sections.

The waveform can be configured to start after the Device Manager has started up; it can also optionally wait a configurable amount of time for the domain to be available before attempting to start an instance of the waveform. If the waveform depends on devices or services, it is recommended that you add a custom script to verify that those devices and services have started and registered with the Domain Manager (see the `start_pre_script` parameter).

The [rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}) can generate an example waveform configuration file with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK waveform. To generate a generic waveform configuration, use the following command.
```sh
rhadmin config waveform > waveform.ini
```
To generate a waveform configuration from an existing waveform project, use the following command.
```sh
rhadmin config waveform <path/to/waveform>/<file>.sad.xml <DomainName> > waveform.ini
```

## Configuration Parameters

{{% notice note %}}
Parameter names are case sensitive.  
The following are the valid values for boolean configuration parameters. If no value is present the feature is disabled.  
True: 1, true, True  
False: 0, false, False
{{% /notice %}}


parameter: `DOMAIN_NAME`  
required: Yes  
default value: None  
format: A name with no spaces or periods (for example, `REDHAWK_DEV`).  
description: Domain name to associate with this waveform.

parameter: `WAVEFORM`  
required: Yes  
default value: None  
format: Valid directory under `$SDRROOT/dom/waveforms`, or a namespaced waveform identifier (for example, `rh.MyWaveform`).  
description: Name of the waveform to launch.

parameter: `INSTANCE_NAME`  
required: No  
format: A name with no spaces or periods (for example, `THE_WAVEFORM`)  
description: Used to give the waveform an instance-specific name.  

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Standard shell path environment variable.  
description: Path to use as the SDRROOT for this waveform.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Standard shell path environment variable.  
description: Absolute path to use as the OSSIEHOME for this waveform.

parameter: `PYTHONPATH`  
required: No  
default value: `$PYTHONPATH`  
format: Standard shell path environment variable.  
description: Path used by Python interpreter to load modules, overrides `PYTHONPATH` environment variable.

parameter: `start_delay`  
required: No  
default value: `30`  
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
default value: `True`  
format: `True`, `False`, or a string to be matched against `conditional_config`  
description: Specifies if waveform is able to be started.  `True` or `False` will enable or disable the waveform. See `conditional_config` below for how a string value gets evaluated.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of waveforms based on the `enable` parameter and the contents of this conditional config file’s `enable` field. If the value of both `enable` parameters are the same, the waveform will start; otherwise, the waveform is skipped. For example, `enable=primary` causes the `conditional_config` file to be examined for an `enable=primary` statement to match against when starting a waveform on the host. If no match is found, the waveform is skipped.

parameter: `priority`  
required: No  
default value: `900`  
description: Relative priority of the waveform in the group of processes to start for this domain. Lower values will be started earlier (for example priority 800 will be started before priority 900).

parameter: `autostart`  
required: No  
default value: `True`  
description: Automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: `5`  
description: Number of seconds the waveform has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: `45`  
description: Number of seconds to wait for the previous higher priority process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: `False`  
description: Abort starting this domain if `waitforprevious` has expired and the previous process hasn't been declared started yet.  This is useful to make sure the DeviceManager is started and all devices are ready before launching the waveform.

parameter: `startretries`  
required: No  
default value: `0`  
description: Number of times to try restarting the waveform on startup.

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
description: Number of seconds to wait when stopping the waveform.

parameter: `started_status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to determine if a waveform is started properly.

parameter: `status_script`  
required: No  
default value: None  
description: Path to file or directory to search for scripts to be run to get the status for the waveform.

parameter: `environment`  
required: No  
default value: None  
format: A list of key/value pairs in the form `key="value",key2="value2"`.  
description: Override existing environment variables or set new ones to be used when starting the Domain Manager.

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

parameter: `directory`  
required: No  
default value:`$SDRROOT`  
description: Change directory to `directory` before running this waveform.

parameter: `run_detached`  
required: No  
default value: `True`  
description: Run the waveform as a daemon, not a child of the AdminService process.

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
description: If `redirect_stderr` is False, name of a file that captures the stderr from the process. If not specified, the default value list above is used.

parameter: `redirect_stderr`  
required: No  
default value: `True`  
description: Write stdout and stderr to the same file.

parameter: `stopsignal`  
required: No  
default value: `TERM`  
description: Signal to send when stopping the waveform.

parameter: `stopasgroup`  
required: No  
default value: `False`  
description: Send stop signal to the process group when stopping.

parameter: `killasgroup`  
required: No  
default value: `False`  
description: Send SIGKILL to the process group when forcibly killing the waveform.

parameter: `start_pre_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run before the waveform is started.

parameter: `start_post_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run after the waveform is started.

parameter: `stop_pre_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run before the waveform is stopped.

parameter: `stop_post_script`  
required: No  
default value: None  
format: Absolute path of a file, or absolute path of a directory of files  
description: Script or directory of scripts to be run after the waveform is stopped.
