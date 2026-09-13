# CachyOS Limine Boot Repair Notes

## Problem

Boot error:

``` text
Failed to open module boot():/initramfs-linux-cachyos-lts.img
```

Cause: - `initramfs-linux-cachyos.img` missing -
`initramfs-linux-cachyos-lts.img` missing

## Partition Layout

``` text
EFI    : /dev/nvme0n1p1
Swap   : /dev/nvme0n1p2
Root   : /dev/nvme0n1p3 (Btrfs)
Home   : /dev/nvme0n1p4 (Btrfs)
```

## Verified

-   `limine.conf` is correct.
-   Root UUID matches `nvme0n1p3`.
-   Kernel images exist.
-   Initramfs images are missing.

## Enter Chroot

``` bash
sudo cachy-chroot
```

Select: - Root → `nvme0n1p3` - EFI → `nvme0n1p1`

## Diagnostics

``` bash
ls /boot
ls /etc/mkinitcpio.d
pacman -Q linux-cachyos linux-cachyos-lts mkinitcpio
```

## Generate Initramfs

``` bash
mkinitcpio -P
```

If that fails:

``` bash
mkinitcpio -p linux-cachyos
mkinitcpio -p linux-cachyos-lts
```

## Verify

``` bash
ls /boot
```

Expected files:

``` text
initramfs-linux-cachyos.img
initramfs-linux-cachyos-fallback.img
initramfs-linux-cachyos-lts.img
initramfs-linux-cachyos-lts-fallback.img
```

## If Still Broken

``` bash
ls /etc/mkinitcpio.d
```

If preset files are missing:

``` bash
pacman -S linux-cachyos linux-cachyos-lts
```

## Next Step

If `mkinitcpio -P` still shows:

``` text
option -p requires an argument
```

send the output of:

``` bash
ls /etc/mkinitcpio.d
```
