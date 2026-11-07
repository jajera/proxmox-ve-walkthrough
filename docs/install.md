# Install

Screenshots are added after the authoring pass (see the repo issues for the
capture checklist). Until then, follow the table and the steps below.

## Boot from USB

1. Plug the USB into the target machine; connect display, keyboard, and Ethernet.
2. Power on. Open the boot menu (often **F10**, **F12**, or **Esc**; BIOS setup
   often **F2** or **Del** — varies by vendor).
3. Boot the USB in **UEFI** mode when both UEFI and legacy appear.
4. At the Proxmox boot menu, choose **Install Proxmox VE (Graphical)**.

If the graphical installer is blank or garbled, reboot and choose
**Install Proxmox VE (Terminal UI)**. You can also add the `nomodeset` kernel
parameter from the advanced boot options.

## Installer choices

| Screen | Choice |
| --- | --- |
| EULA | Agree / I agree |
| Target Harddisk | See layout tabs below |
| Location / time zone / keyboard | Your country; auto-detect is usually fine |
| Password + email | Strong root password (≥ 12 chars recommended); admin email |
| Management network | Management NIC, FQDN, static IPv4 CIDR, gateway, DNS |
| Summary | Review; enable automatic reboot; Install |

### EULA

Read and accept the End User License Agreement.

### Target Harddisk

!!! danger "Disks are erased"
    Every selected target disk is wiped. Confirm you selected the machine’s
    internal storage, **not** the installer USB.

=== "Two disks: ZFS mirror"

    1. On **Target Harddisk**, open **Options**.
    2. Set the filesystem to **ZFS (RAID1)**.
    3. Select both internal disks. Set any other slots to **-- do not use --**.
    4. Confirm **OK**, then **Next**.

    Usable space is about one disk. Speed follows the slower disk. A mirror is
    not a backup.

=== "One disk: ext4"

    1. Leave the filesystem as **ext4** (installer default with LVM).
    2. Select the single internal disk.
    3. **Next**.

### Location, time zone, keyboard

Accept auto-detected values unless they are wrong for your site.

### Administration password and email

Set the `root` password and an email address used for update and cron notices.
The password must be at least 8 characters; 12 or more with mixed character
classes is recommended.

### Management network

| Field | Example |
| --- | --- |
| Management interface | first wired NIC that is UP |
| Hostname (FQDN) | `pve.home.arpa` |
| IP address (CIDR) | `192.168.1.60/24` |
| Gateway | `192.168.1.1` |
| DNS server | `192.168.1.1` |

The installer configures either IPv4 or IPv6 here, not both. Add the other
family after install if you need dual-stack.

### Summary

Re-check every value. Enable automatic reboot if offered, then **Install**.
Wait for package copy to finish, remove the USB when prompted, and let the
machine reboot from disk.

If install fails, press **Ctrl+Alt+F2** for the second console and read the
log, then confirm the hardware meets the minimum requirements.

## Next

[First boot](first-boot.md)
