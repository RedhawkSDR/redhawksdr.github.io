---
title: "Waveform Configuration File"
weight: 50
---

REDHAWK waveforms are controlled by files in the `/etc/redhawk/waveforms.d` directory. Default waveform configuration parameters are stored in `/etc/redhawk/init.d/waveform.defaults`. To define multiple waveform instances in one file, add multiple sections.

The waveform can be configured to start after the Device Manager has started up; it can also optionally wait a configurable amount of time for the domain to be available before attempting to start an instance of the waveform. If the waveform depends on devices or services, it is recommended that you add a custom script to verify that those devices and services have started and registered with the Domain Manager (see the `start_pre_script` parameter).

[rhadmin]({{< relref "manual/appendices/adminservice/rhadmin.md" >}}) can generate an example waveform configuration file with the complete set of parameters that can be used to the control the setup and execution of a REDHAWK waveform. To generate a generic waveform configuration, enter the following command.
```sh
rhadmin config waveform > waveform.ini
```
To generate a waveform configuration from an existing waveform project, enter the following command.
```sh
rhadmin config waveform <path/to/waveform>/<file>.sad.xml <DomainName> > waveform.ini
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
format: A name with no spaces or periods (for example, `REDHAWK_DEV`)  
description: The domain name to associate with this waveform.

parameter: `WAVEFORM`  
required: Yes  
default value: None  
format: Valid directory under `$SDRROOT/dom/waveforms`, or a namespaced waveform identifier (for example, `rh.MyWaveform`)  
description: The name of the waveform to launch.

parameter: `INSTANCE_NAME`  
required: No  
format: A name with no spaces or periods (for example, `THE_WAVEFORM`)  
description: Specifies an instance-specific name for the waveform.

parameter: `SDRROOT`  
required: No  
default value: `$SDRROOT`  
format: Standard shell path environment variable  
description: The absolute path to use as the `SDRROOT` for this waveform.

parameter: `OSSIEHOME`  
required: No  
default: `$OSSIEHOME`  
format: Standard shell path environment variable  
description: The absolute path to use as the `OSSIEHOME` for this waveform.

parameter: `PYTHONPATH`  
required: No  
default value: `$PYTHONPATH`  
format: Standard shell path environment variable  
description: The path used by Python interpreter to load modules; overridesthe  `PYTHONPATH` environment variable.

parameter: `start_delay`  
required: No  
default value: `30`  
description: The amount of time for the waveform control script to wait for the Domainmanager to be available before failing startup.

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
description: Specifies if waveform may be started. `True` or `False` will enable or disable the waveform. Refer to `conditional_config` for information about how a string value gets evaluated.

parameter: `conditional_config`  
required: No  
default value: `/etc/redhawk/rh.cond.cfg`  
description: Allows conditional startup of processes based on the `enable` parameter and the contents of this conditional config. If the value `enable` is a string, the process will start only if there is a line in the `conditional_config` file that has that exact content; otherwise, the process is skipped. For example, `enable="type=primary"` causes the `conditional_config` file to be examined for a line equal to `type=primary` when starting a process on the host. If there is no `type=primary` line in the file, the process is skipped.

parameter: `priority`  
required: No  
default value: `900`  
description: The priority of this waveform relative to the group of processes to start for this domain. Lower values will be started earlier. For example, priority 800 will be started before priority 900.

parameter: `autostart`  
required: No  
default value: `True`  
description: Specifies whether to automatically start this process when the AdminService starts, if `enable` is True.

parameter: `startsecs`  
required: No  
default value: `5`  
description: The number of seconds the waveform has to be running to consider it started.

parameter: `waitforprevious`  
required: No  
default value: `45`  
description: The number of seconds to wait for the previous higher priority process to start before trying to start this process.

parameter: `failafterwait`  
required: No  
default value: `False`  
description: Specifies whether to abort starting this domain if `waitforprevious` has expired and the previous process has not been declared started yet. This is useful to make sure the DeviceManager is started and all devices are ready before launching the waveform.

parameter: `startretries`  
required: No  
default value: `0`  
description: The number of times to try restarting the waveform on startup.

parameter: `autorestart`  
required: No  
default value: `True`  
description: Specifies whether to automatically restart this process when an abnormal termination is detected. Abnormal is any exit code not in `exitcodes`.

parameter: `exitcodes`  
required: No  
default value: `0,2`  
description: The expected exit codes from the process.

parameter: `stopwaitsecs`  
required: No  
default value: `10`  
description: The number of seconds to wait when stopping the waveform.

parameter: `started_status_script`  
required: No  
default value: None  
format: Absolute path of a file  
description: Specifies the script used to determine if the waveform started properly.

parameter: `status_script`  
required: No  
default value: None  
format: Absolute path of a file  
description: Specifies the script to check the status for the waveform.

parameter: `environment`  
required: No  
default value: None  
format: A list of key/value pairs in the form `key="value",key2="value2"`  
description: Specifies whether to override existing environment variables or set new ones to be used when starting the Domain Manager.

parameter: `user`  
required: No  
default value: `redhawk`  
description: Executes the process with User ID.

parameter: `group`  
required: No  
default value: `redhawk`  
description: Executes the process with Group ID.

parameter: `umask`  
required: No  
default value: None  
description: The umask for the process.

parameter: `directory`  
required: No  
default value:`$SDRROOT`  
description: Specifies to change the directory to `directory` before running this waveform.

parameter: `run_detached`  
required: No  
default value: `True`  
description: Specifies to run the the waveform as a daemon, not a child of the AdminService process.

parameter: `logfile_directory`  
required: No  
default value: `/var/log/redhawk/waveforms`  
description: The absolute path to the logging directory.

parameter: `stdout_logfile`  
required: No  
default value: `<domain name>.<waveform name>.stdout.log`  
description: The name of a file that captures the stdout from the process. If not specified, the default value list above is used.

parameter: `stderr_logfile`  
required: No  
default value: `<domain name>.<waveform name>.stderr.log`  
description: If `redirect_stderr` is False, the name of a file that captures the stderr from the process. If not specified, the default value list above is used.

parameter: `redirect_stderr`  
required: No  
default value: `True`  
description: Specifies to write stdout and stderr to the same file.

parameter: `stopsignal`  
required: No  
default value: `TERM`  
description: The signal to send when stopping the waveform.

parameter: `stopasgroup`  
required: No  
default value: `False`  
description: Specifies to send the stop signal to the process group when stopping.

parameter: `killasgroup`  
required: No  
default value: `False`  
description: Specifies to send SIGKILL to the process group when forcibly killing the waveform.

parameter: `start_pre_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: The script or directory of scripts to run before the waveform is started.

parameter: `start_post_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: The script or directory of scripts to run after the waveform is started.

parameter: `stop_pre_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: The script or directory of scripts to run before the waveform is stopped.

parameter: `stop_post_script`  
required: No  
default value: None  
format: Absolute path of a file or absolute path of a directory of files  
description: The script or directory of scripts to run after the waveform is stopped.
