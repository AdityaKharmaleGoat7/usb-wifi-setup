# USB Wi-Fi driver setup

Local snapshot of the RTL8812AU/RTL8821AU/RTL8814AU driver setup inspected on 2026-10-20.
The exact connected adapter model and connection status have not been verified.

## Installed state

- OS: Ubuntu 24.04.4 LTS, x86_64.
- Running kernel: `7.0.0-31-generic`.
- DKMS: `8812au/5.6.4.2_35491.20191025`, installed for kernels
  `7.0.0-30-generic` and `7.0.0-31-generic`.
- Kernel module: `88XXau`, observed loaded with `lsmod`.
- Installed module: `/lib/modules/7.0.0-31-generic/updates/dkms/88XXau.ko.zst`.
- Installed source: `/usr/src/8812au-5.6.4.2_35491.20191025`.
- Original local checkout: `/home/melco/rtl8812au`.
- Upstream: https://github.com/aircrack-ng/rtl8812au.git
- Base commit: `734485506a30d6237c2deaad666a19f8ca5379f2`.

## Preserved local changes

This repository retains the original Git history and copies these five changed
files from the installed source, rather than assuming the original checkout
matches the installed driver:

- `Makefile`
- `hal/phydm/phydm.mk`
- `hal/phydm/sd4_phydm_2_kernel.mk`
- `include/osdep_service_linux.h`
- `os_dep/linux/ioctl_cfg80211.c`

These changes are left uncommitted for review using `git diff`.
The upstream README marks this driver deprecated; it is preserved here because
it is the driver already installed on this machine.

## Build and inspect

The project defines a kernel-module build, not a unit-test or lint suite:

```bash
make -j2
dkms status -m 8812au
modinfo 88XXau
lsmod | grep 88XXau
```

Building needs development tools and headers for the running kernel. Building
alone does not install or reload the driver. See `VERIFICATION.txt` for the
result of the local build check.

## Installation reference

The existing Makefile provides `sudo make dkms_install`. This was not run as
part of creating this repository; the driver is already installed. Note that
`dkms.conf` names the package `realtek-rtl88xxau` with version
`5.6.4.2~20230501`, while the Makefile and installed DKMS registration use
`8812au/5.6.4.2_35491.20191025`. That existing mismatch is preserved and needs
review before attempting a fresh installation.

No Wi-Fi passwords, network profiles, or signing private keys are included.
