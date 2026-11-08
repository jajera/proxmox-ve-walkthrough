# What you need

| Item | Detail |
| --- | --- |
| Target machine | x86_64, UEFI. Enable Intel VT-x / AMD-V in firmware |
| RAM | 2 GB+ (8 GB+ comfortable with ZFS) |
| Disks | SSDs preferred. Two for the ZFS mirror, one for ext4 |
| Display + keyboard | For the installer |
| Network | Wired Ethernet |
| USB stick | 4 GB+, erased |
| Workstation | Linux shell for the commands (Windows: Etcher or Rufus) |
| Workstation disk | About 2 GB for the ISO |

## Pick your layout

=== "Two disks: ZFS mirror"

    Two internal SSDs of similar size. Usable space is one disk, speed follows
    the slower one, and either disk can fail without taking the host down.
    A mirror is **not** a backup.

=== "One disk: ext4"

    A single internal disk with the installer default (ext4 on LVM). Full
    capacity, no redundancy.

!!! danger "The install erases every selected disk"
    Back up first. This applies to both layouts.

## Check your disks

From a Linux live USB on the target:

<div class="run" markdown>

```bash
lsblk -d -o NAME,SIZE,MODEL,TRAN,ROTA
```

```text {.no-copy}
NAME      SIZE MODEL              TRAN   ROTA
nvme0n1 931.5G Samsung SSD 970…   nvme      0
sda     931.5G Samsung SSD 860…   sata      0
```

</div>

`ROTA=0` is an SSD, `ROTA=1` a spinning disk.

## Values this guide uses

Change them to suit. Pick a hostname and static IP **before** you start, since
the hostname is awkward to change later.

| Setting | Example |
| --- | --- |
| Installer | Install Proxmox VE (Graphical) |
| FQDN | `pve.home.arpa` |
| IP / gateway / DNS | `192.168.1.60/24`, `192.168.1.1` |
| Login | `root`, realm PAM |

## Next

[Download ISO](download.md){ .md-button }
