# Ubuntu 22.04 Goss config

## Overview

### Based on STIG Ubuntu Linux 22.04 LTS Benchmark v2.1.0 [Release](https://dl.dod.cyber.mil/wp-content/uploads/stigs/zip/U_CAN_Ubuntu_22-04_LTS_V2R1_STIG.zip)

Set of configuration files and directories to run the first stages of stig of Ubuntu 20.04 servers

This is configured in a directory structure level.

This could do with further testing but sections 1.x should be complete

Goss is run based on the goss.yml file in the top level directory. This specifies the configuration.

## variables

file: vars/stig.yml

Please refer to the file for all options and their meanings

STIG listed variable for every control/benchmark can be turned on/off or section

- other controls
enable_selinux
run_heavy_tasks

- bespoke options
If a site has specific options e.g. password complexity these can also be set.

## Requirements

goss >= 0.4.4
root privileges

## Branches

If running as part of the ansible playbook, this will pull in the relevant branch for the version of benchmark you are remediating.

- e.g. v1.0.0 will pull in branch benchmark-v1.0.0

Devel is normally the latest benchmark version, so maybe different from the version of benchmark you wish to test.
Details will show in the README as to the benchmark for the version it is written for.

## Usage

You must have [goss](https://github.com/goss-org/goss/) available to your host you would like to test.

- Run as root not sudo due to sudo and shared memory access

Assuming you have already clone this repository you can run goss from where you wish.

- full check

```sh
# {{path to your goss binary}} --vars {{ path to the vars file }} -g {{path to your clone of this repo }}/goss.yml --validate

```

example:

```sh
# /usr/local/bin/goss --vars ../vars/stig.yml -g /home/bolly/ub22_stig_goss/goss.yml validate
......FF....FF................FF...F..FF.............F........................FSSSS.............FS.F.F.F.F.........FFFFF....

Failures/Skipped:

Title: 1.6.1 Ensure core dumps are restricted (Automated)_sysctl
Command: suid_dumpable_2: exit-status:
Expected
    <int>: 1
to equal
    <int>: 0
Command: suid_dumpable_2: stdout: patterns not found: [fs.suid_dumpable = 0]


Title: 1.4.2 Ensure filesystem integrity is regularly checked (Automated)
Service: aidecheck: enabled:
Expected
    <bool>: false
to equal
    <bool>: true
Service: aidecheck: running:
Expected
    <bool>: false
to equal
    <bool>: true

< ---------cut ------- >

Title: 1.1.22 Ensure sticky bit is set on all world-writable directories
Command: version: exit-status:
Expected
    <int>: 0
to equal
    <int>: 123

Total Duration: 5.102s
Count: 124, Failed: 21, Skipped: 5

```

- running a particular section of tests

```sh
# /usr/local/bin/goss -g /home/bolly/rh8_stig_goss/section_1/stig_1.1/stig_1.1.22.yml  validate
............

Total Duration: 0.033s
Count: 12, Failed: 0, Skipped: 0

```

- changing the output

```sh
# /usr/local/bin/goss -g /home/bolly/rh8_stig_goss/section_1/stig_1.1/stig_1.1.22.yml  validate -f documentation
Title: 1.1.20 Check for removeable media nodev
Command: floppy_nodev: exit-status: matches expectation: [0]
Command: floppy_nodev: stdout: matches expectation: [OK]
< -------cut ------- >
Title: 1.1.20 Check for removeable media noexec
Command: floppy_noexec: exit-status: matches expectation: [0]
Command: floppy_noexec: stdout: matches expectation: [OK]


Total Duration: 0.022s
Count: 12, Failed: 0, Skipped: 0
```

## Extra settings

Ability to add your own requirements is available in several sections

## further information

- [goss documentation](https://github.com/goss-org/goss/blob/master/README.md)
- [stig standards](https://public.cyber.mil/stigs/downloads/?_dl_facet_stigs=unix-linux)

## Feedback required
