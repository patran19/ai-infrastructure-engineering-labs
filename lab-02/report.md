# Lab 02 – Linux Baseline Validation Report

## Objective

The objective of this lab was to collect and review the Linux system baseline, including system identity, operating-system information, service health, storage devices, mounted filesystems, network interfaces, routes, and listening ports.

## Evidence Collected

The following evidence files were created:

- `evidence/raw/system.txt`
- `evidence/raw/services.txt`
- `evidence/raw/devices.txt`
- `evidence/raw/network.txt`

## System Summary

- Hostname: `paultran-pxe`
- Operating-system, kernel, uptime, and time information were recorded in `evidence/raw/system.txt`.
- Time synchronization status was checked with `timedatectl`.

## Service Health

| Service | Observed State | Classification | Explanation |
|---|---|---|---|
| `dnsmasq.service` | Failed: This host is a PXE server, and `dnsmasq` may provide DHCP, DNS, TFTP, or PXE-related services. The failure must be investigated because it may affect PXE operation. |
| `openipmi.service` | Failed: The system does not expose a local IPMI device such as `/dev/ipmi0`. This is expected on systems without a local IPMI interface. Remote BMC access can use `ipmitool -I lanplus`. |
| `systemd-networkd-wait-online.service` | Failed:  One or more configured interfaces did not reach the online state before the boot timeout. Required network connectivity may still be working. 

  UNIT                                 LOAD   ACTIVE SUB    DESCRIPTION
● dnsmasq.service                      loaded failed failed dnsmasq - A lightweight DHCP and caching DNS server
● openipmi.service                     loaded failed failed LSB: OpenIPMI Driver init script
● systemd-networkd-wait-online.service loaded failed failed Wait for Network to be Configured


## Storage and Filesystems

The `findmnt` output showed the following important filesystems:

- Root filesystem: `/dev/mapper/ubuntu--vg-ubuntu--lv`
- Root filesystem type: `ext4`
- Boot partition: `/dev/nvme0n1p2`
- EFI partition: `/dev/nvme0n1p1`
- EFI filesystem type: `vfat`

Additional virtual filesystems included:

- `proc`
- `sysfs`
- `tmpfs`
- `devtmpfs`
- `cgroup2`
- Docker network namespaces
- Docker overlay filesystems

The full device and filesystem information was saved in `evidence/raw/devices.txt`.

## Network Baseline

- USB Ethernet interface: `enx9c69d33c3418`
- Interface state: `UP`
- Link state: `LOWER_UP`
- MTU: `1500`
- IPv4 configuration: DHCP
- Two dynamically assigned IPv4 addresses were observed on the same interface.
- The duplicate addresses may have been caused by two DHCP clients, two network managers, or two different DHCP client identifiers.
- Interface addresses, routes, and listening ports were recorded in `evidence/raw/network.txt`.

## Listening Ports

The following command was used:

```bash
ss -lntup
