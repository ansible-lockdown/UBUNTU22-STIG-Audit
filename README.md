# Ubuntu 22.04 Syver config

## Overview

### Based on STIG Ubuntu Linux 22.04 LTS Benchmark v2.10.0 [Release](https://dl.dod.cyber.mil/wp-content/uploads/stigs/zip/U_CAN_Ubuntu_22-04_LTS_V2R10_STIG.zip)

Ability to audit a system using a lightweight binary to check the current state.

This is:

- very small < 14MB
- lightweight
- self contained

It works using a set of configuration files and directories to audit STIG of Ubuntu22 servers. These files/directories correlate to the STIG Level and STIG_ID

feedback on any differences between OSs please raise an issue

## Requirements

You must have [syver](https://github.com/krameff/syver) >= 0.13.0 available to the host you would like to test. The audit content uses the goss file format, which syver reads unchanged.

You must have sudo/root access to the system as some commands require privilege information.

Assuming you have already clone this repository you can run syver from where you wish.

Please refer to the audit documentation for usage.

- [readthedocs](https://ansible-lockdown.readthedocs.io/en/latest/)

This also works alongside the [Ansible Lockdown Ubuntu22-STIG role](https://github.com/ansible-lockdown/UBUNTU22-STIG)

Which will:

- install
- audit
- remediate
- audit

## Join us

On our [Discord Server](https://www.lockdownenterprise.com/discord) to ask questions, discuss features, or just chat with other Ansible-Lockdown users

Set of configuration files and directories to run the first stages of STIG Ubuntu 22.04 based servers

This is configured in a directory structure level.

Syver is run based on the goss.yml file in the top level directory. This specifies the configuration.

## further information

- [Syver documentation](https://github.com/krameff/syver/blob/main/docs/index.md)
- [goss vs Syver](https://github.com/krameff/syver/blob/main/docs/goss-vs-syver.md)
- [STIG standards](https://public.cyber.mil/stigs/)
