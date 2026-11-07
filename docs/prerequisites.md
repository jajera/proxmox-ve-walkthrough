# What you need

| Item | Detail |
| --- | --- |
| Target machine | x86_64 with UEFI; enable Intel VT-x or AMD-V in firmware for KVM |
| RAM | Evaluation minimum 1 GB; recommended ≥ 2 GB for the OS plus guest memory. With ZFS, ≥ 8 GB is comfortable |
| Disks | SSDs preferred. Two matching disks for the ZFS mirror tab; one disk for the ext4 tab |
| Display + keyboard | HDMI (or similar) and USB keyboard for install |
| Network | Wired Ethernet preferred (installer configures a management NIC) |
| USB stick | ≥ **4 GB** and larger than the ISO; contents erased |
| Workstation | Downloads the ISO, verifies SHA256, writes the stick, later opens the web UI |
| Disk space on workstation | ~**2 GB** for `proxmox-ve_9.2-1.iso` under `$HOME/Downloads` |

## Pick your layout

=== "Two disks: ZFS mirror"

    Use when you have **two** internal SSDs of the same size (or close — the
    mirror uses the smaller capacity and the slower disk’s speed).

    - Usable space ≈ one disk
    - One disk can fail without taking the host down
    - A mirror is **not** a backup

=== "One disk: ext4"

    Use when you have a **single** internal disk. This is the installer default
    (ext4 with LVM).

    - Full disk capacity available
    - No redundancy if that disk fails

### Confirm disks before you wipe them

From a Linux live USB on the target (or any view of its disks):

<div class="run" markdown>

```bash
lsblk -d -o NAME,SIZE,MODEL,TRAN,ROTA
```

```text {.no-copy}
NAME    SIZE MODEL                 TRAN   ROTA
nvme0n1 931.5G Samsung SSD 970…    nvme      0
sda     931.5G Samsung SSD 860…    sata      0
```

</div>

`ROTA=0` means SSD; `ROTA=1` means spinning disk. Prefer SSDs for the mirror.
Back up anything on either disk first — install erases every selected target.

## Defaults this guide uses

Consumers may rename these — record what you choose. The hostname is awkward to
change later.

| Setting | This guide |
| --- | --- |
| ISO | `proxmox-ve_9.2-1.iso` (Proxmox VE 9.2) |
| Installer | Install Proxmox VE (Graphical) |
| Layout | ZFS RAID1 on two disks, or ext4 on one disk (tabs) |
| FQDN | `pve.home.arpa` |
| Management IP | `192.168.1.60/24` (example) |
| Gateway / DNS | `192.168.1.1` |
| User | `root` (realm PAM) |
| Email | lab admin address for update notices |

Pick a hostname and a static IPv4 (or IPv6 — the installer allows one family,
not both) before you start.

## Next

[Download ISO](download.md)
