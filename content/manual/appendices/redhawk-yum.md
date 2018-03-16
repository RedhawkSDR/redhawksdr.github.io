---
title: "REDHAWK Yum Repository and Packages"
weight: 10
---

The following sections describe the REDHAWK packages and provided dependencies.

{{% notice note %}}
[External dependencies]({{< relref "manual/appendices/dependencies.md" >}}) are also necessary for development with REDHAWK and for building REDHAWK from source.
{{% /notice %}}

## REDHAWK Yum Repository

All of the REDHAWK packages are included in a yum repository, which can be configured as described in [Setting Up the REDHAWK Repository]({{< relref "manual/installation/_index.md#setting-up-the-redhawk-repository" >}}). The repository contains three yum groups (REDHAWK Runtime, REDHAWK Development, and REDHAWK Enterprise Integration) and additional OS dependencies. The yum groups and additional OS dependencies are described in the following sections:

### REDHAWK Yum Groups

#### REDHAWK Runtime

  - `bulkioInterfaces`
  - `burstioInterfaces`
  - `frontendInterfaces`
  - `GPP`
  - `GPP-profile`
  - `redhawk-sdrroot-dev-mgr`
  - `redhawk-sdrroot-dom-mgr`
  - `redhawk-sdrroot-dom-profile`
  - `redhawk`

#### REDHAWK Development

  - `redhawk-basic-components`
  - `redhawk-basic-devices`
  - `redhawk-basic-waveforms`
  - `redhawk-codegen`
  - `redhawk-devel`
  - `redhawk-ide`
  - `redhawk-qt-tools`

{{% notice note %}}
The Development group will install the Runtime group packages as dependencies.
{{% /notice %}}

#### REDHAWK Enterprise Integration

  - `redhawk-enterprise-integration-demo-dist`
  - `redhawk-enterprise-integration-dist`
  - `redhawk-enterprise-integration-docs`

### Dependencies Packaged with REDHAWK

#### Dependencies for RHEL/CentOS 6

  - `libomniEvents2`
  - `libomniEvents2-devel`
  - `log4cxx`
  - `log4cxx-devel`
  - `omniEvents-bootscripts`
  - `omniEvents-debuginfo`
  - `omniEvents-doc`
  - `omniEvents-server`
  - `omniEvents-utils`
  - `omniORBpy-debuginfo`
  - `omniORBpy-devel`
  - `omniORBpy-libs`
  - `python-omniORB`
  - `uhd`
  - `uhd-debuginfo`
  - `uhd-devel`
  - `uhd-doc`
  - `uhd-firmware`

To install the dependencies for RHEL/CentOS 6, enter the following commands:

```bash
sudo yum install libomniEvents2 libomniEvents2-devel log4cxx \
    log4cxx-devel omniEvents-bootscripts omniEvents-debuginfo omniEvents-doc \
    omniEvents-server omniEvents-utils omniORBpy-debuginfo omniORBpy-devel \
    omniORBpy-libs python-omniORB uhd uhd-debuginfo uhd-devel uhd-doc uhd-firmware
```

#### Dependencies for RHEL/CentOS 7

  - `libomniEvents2`
  - `libomniEvents2-devel`
  - `omniEvents-bootscripts`
  - `omniEvents-debuginfo`
  - `omniEvents-doc`
  - `omniEvents-server`
  - `omniEvents-utils`
  - `omniORBpy-debuginfo`
  - `omniORBpy-devel`
  - `omniORBpy-libs`
  - `python-omniORB`

To install the dependencies for RHEL/CentOS 7, enter the following commands:

```bash
sudo yum install libomniEvents2 libomniEvents2-devel omniEvents-bootscripts \
    omniEvents-debuginfo omniEvents-doc omniEvents-server omniEvents-utils \
    omniORBpy-debuginfo omniORBpy-devel omniORBpy-libs python-omniORB \
    uhd uhd-debuginfo uhd-devel uhd-doc uhd-firmware
```

### Selective Installation

After you set up the REDHAWK yum repository as described in [Setting Up the REDHAWK Repository](/manual/installation/#setting-up-the-redhawk-repository), you can also install individual packages via yum for selective installations.

For example, to perform a selective installation that includes the GPP, enter the following command:

```bash
sudo yum install GPP
```

To perform a selective development installation that includes the REDHAWK IDE, enter the following command:

```bash
sudo yum install redhawk-ide
```
