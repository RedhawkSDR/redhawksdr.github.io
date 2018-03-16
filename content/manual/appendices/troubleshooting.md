---
title: "Troubleshooting"
weight: 80
---

This appendix explains how to troubleshoot and resolve `omniNames/omniEvents` failures.

## Performing a Hard Reset Using the `cleanomni` Script

The `cleanomni` script is used to perform a hard reset of `omniNames` and `omniEvents` and delete the associated log files. To run this script, enter the following command:

```bash
sudo $OSSIEHOME/bin/cleanomni
```

The `cleanomni` script performs the following commands:

```bash
sudo /etc/init.d/omniEvents stop
sudo /etc/init.d/omniNames stop
sudo rm -rf /var/log/omniORB/* /var/lib/omniEvents/*
sudo /etc/init.d/omniNames start
sudo /etc/init.d/omniEvents start
```

## Diagnosing Problems Using the `rh_net_diag` Script

The `rh_net_diag` script is a Python script used to diagnose various omniORB-related problems and to perform other system checks to diagnose potential problems that may impact a REDHAWK installation. To run this script, enter the following command:

```bash
rh_net_diag
```

By default, the `--ns` (naming service), `--dom` (Domain Manager), and `--dev` (Device Manager) options are enabled when the script is executed. These options assume that `omniNames`, the Domain Manager, and the Device Manager are all running on the host executing the script. For help with `rh_net_diag`, enable the `-h` option. For more detailed output, enable the `-v` option. The four test categories include:

  - Standard tests performed every time `rh_net_diag` is executed.
      - Check if `OSSIEHOME`, `SDRROOT`, and `/etc/omniORB.cfg` are readable.
      - Check that omniORB is installed.
      - If `/etc/omniORB.cfg` is not readable or if omniORB is not installed, the script terminates.

  - Tests that diagnose potential problems with the naming service. They are performed if `--ns` is enabled.
      - Check if `omniNames` is running.
      - Check if `omniEvents` is running and if it is running locally. If it is not running at all, refer to [Performing a Hard Reset Using the cleanonmi Script](#performing-a-hard-reset-using-the-cleanomni-script). If it is not running locally, assume it is running on another host.
      - Retrieve entries currently stored in the naming service. Refer to [Common Causes for omniNames Failure](#common-causes-for-omninames-failure) for further assistance.
      - Check `/etc/omniORB.cfg` to ensure that all defined endPoints are correct.
      - If all the above checks pass but the Domain Manager and Device Manager still cannot communicate with the naming service, manually check the firewall settings on the host running `omniNames`.

  - Tests that diagnose potential problems with the Domain Manager. They are performed if `--dom` is enabled.
      - Attempt to retrieve entries currently stored in the naming service.
      - Check if `omniEvents` is running and if it is running locally. If it is not running at all, refer to [Performing a Hard Reset Using the cleanonmi Script](#performing-a-hard-reset-using-the-cleanomni-script). If it is not running locally, assume it is running on another host.
      - Check `/etc/omniORB.cfg` to ensure all defined endPoints are correct.

  - Tests that diagnose potential problems with the Device Manager. They are performed if `--dev` is enabled.
      - Check if `InitRef` was overwritten with the `rh_net_diag` script `--ORBInitRef` option or if we are using the `InitRef` specified in `/etc/omniORB.cfg`.
      - If `InitRef` is valid, attempt to retrieve entries currently stored in the naming service. Refer to [Common Causes for omniNames Failure](#common-causes-for-omninames-failure) for further assistance.
      - Check if `omniEvents` is running and if it is running locally. If it is not running at all, refer to [Performing a Hard Reset Using the cleanonmi Script](#performing-a-hard-reset-using-the-cleanomni-script). If it is not running locally, assume it is running on another host.
      - Try to connect to the Domain Manager if one exists in the naming service.
      - Check that the IP address for the host running this script is listed in `ifconfig`. If there is no matching entry with the Device Manager, then the Java components fail on initialization.

## Performing a Soft Reset of omniNames and omniEvents

If the runtime-error indicates a naming service failure, enter the following command to attempt a soft reset on `omniNames`:

```bash
sudo /sbin/service omniNames restart
```

This process first performs a stop and then performs a start. If the stop process fails, `omniNames` was never started, stopped due to an error condition, or is in a non-recoverable state. If the start process fails, `omniNames` is either misconfigured or already running (i.e., `omniNames` was not stopped).

{{% notice note %}}
 `omniNames` can potentially report a successful start and then fail soon after. If the `omniNames` service appears to fail after reporting a successful start, a reconfiguration and hard reset of `omniNames` may be necessary. For more information, refer to [Common Failures](#common-causes-for-omninames-failure) and [Using `cleanomni`](#performing-a-hard-reset-using-the-cleanomni-script).
 {{% /notice %}}

A restart of `omniEvents` may be necessary when restarting `omniNames`. To perform a soft reset of `omniEvents`, enter the following command:

```bash
sudo /sbin/service omniEvents restart
```

## Setting Omni Log Levels

When diagnosing `omniNames/omniEvents` problems, it is often useful to set the omni logging levels. Use the following procedure to set the omni logging levels (requires root permissions):

1.  Open the `/etc/omniORB.cfg` file.
2.  Set a traceLevel value. For example:
    ```bash
    traceLevel = 10
    ```

Details on the available trace levels can be found in Chapter 4 of the omniORB User’s Guide (<http://omniorb.sourceforge.net/omni41/omniORB/omniORB004.html> or on your local system at <file:///usr/share/doc/omniORB-devel-4.1.6/doc/omniORB/omniORB004.html>).

For the changes to take effect, restart `omniNames/omniEvents`.

Log messages are displayed in the terminal and in the files contained in `/var/log/omniORB` and `/var/lib/omniEvents`.

## Common Causes for omniNames Failure

### IP Version 6 Conflicts

Certain combinations of IP Version 6 (IPv6) configurations and `/etc/omniORB.cfg` configurations can cause `omniNames` failures.

Specifically, if the `InitRef` section of `/etc/omniORB.cfg` is set to point to `localhost` rather than pointing explicitly to `127.0.0.1`, the operating system may resolve `localhost` to `::1` (the IPv6 localhost) and not to `127.0.0.1` (the IPv4 localhost). If this occurs, `omniNames` fails. There are three options for preventing this failure condition:

  - Explicitly set `127.0.0.1` in the `InitRef` section instead of using `localhost`.
  - Disable IPv6 in the operating system (refer to operating system documentation).
  - Modify the `/etc/hosts` file to prevent `localhost` from being resolved as `::1`.

#### Preventing IPv6 `localhost` Resolution

Below is an example `/etc/hosts` file from an older CentOS distribution:

```/etc/hosts
127.0.0.1   localhost.localdomain     localhost
::1         localhost6.localdomain6   localhost6
```

Below is an example `/etc/hosts` file from a newer CentOS distribution:

```/etc/hosts
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       localhost localhost.localdomain localhost6 localhost6.localdomain6
```

In the older `/etc/hosts file`, `localhost` resolves unambiguously to `127.0.0.1`. In the newer `/etc/hosts` file, `localhost` can resolve to either `127.0.0.1` or `::1` (where resolving to `::1` causes an `omniNames` failure).

The newer `/etc/hosts` file can be modified to read:

```/etc/hosts
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       localhost6 localhost6.localdomain6
```

Alternatively, `localhost4` can be used in the `InitRef` section of `/etc/omniORB.cfg`.

The line pertaining to IPv6 can also be completely removed from the file; however, some operating systems, depending on IPv6 configurations, may automatically repopulate IPv6 localhost settings on reboot.

### Invalid IP Addresses in `/etc/hosts`

Invalid entries in the `/etc/hosts` file may induce an `omniNames` failure. Invalid entries may be in the form of an IP address that cannot be reached or in the form of an entry that is not valid according to the `/etc/hosts` grammar.
{{% notice note %}}
Firewall IP and port settings on both the server and client side may cause the target `omniNames` service to be unreachable.
{{% /notice %}}
