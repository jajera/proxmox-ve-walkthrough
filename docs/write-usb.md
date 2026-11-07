# Write USB

The stick will be **wiped**. Confirm the device is a removable USB — never your
internal NVMe/SATA disk.

CD/DVD and PXE also work with the hybrid ISO; USB is the recommended path.

!!! warning "Do not use UNetbootin"
    UNetbootin does not work with the Proxmox VE installation image.

## 1. Detect the USB

Plug the stick in, then:

<div class="run" markdown>

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,LABEL,MODEL,TRAN,RM
```

```text {.no-copy}
NAME          SIZE TYPE FSTYPE LABEL       MODEL                   TRAN   RM
sda          28.9G disk                    USB DISK 2.0            usb     1
└─sda1       28.9G part vfat                                       usb     1
nvme0n1     931.5G disk                    Samsung SSD …           nvme    0
…
```

</div>

Read the row with `TRAN=usb` and `RM=1`. That whole disk is the target
(example **`/dev/sda`**). The internal OS disks stay on `nvme` / `sata` with
`RM=0` — **do not** use those for `of=`.

## 2. Safety gate (recommended)

<div class="run" markdown>

```bash
eval "$(lsblk -P -o NAME,SIZE,TYPE,MODEL,TRAN,RM /dev/sda | head -1)"
echo "NAME=$NAME SIZE=$SIZE TYPE=$TYPE MODEL=$MODEL TRAN=$TRAN RM=$RM"
test "$TRAN" = "usb" && test "$RM" = "1" && test "$TYPE" = "disk" \
  && echo "safe_to_flash" || echo "REFUSE"
```

```text {.no-copy}
NAME=sda SIZE=28.9G TYPE=disk MODEL=USB DISK 2.0 TRAN=usb RM=1
safe_to_flash
```

</div>

If you see `REFUSE`, stop and re-check `lsblk`.

## 3. Unmount if needed

<div class="run" markdown>

```bash
sudo umount /dev/sda?* 2>/dev/null || true
findmnt /dev/sda /dev/sda1 || echo "not mounted"
```

```text {.no-copy}
not mounted
```

</div>

## 4. Write the ISO with `dd`

Use the **whole disk** (`/dev/sda`), not a partition. Path assumes the ISO from
[Download ISO](download.md):

<div class="run" markdown>

```bash
ISO="$HOME/Downloads/proxmox-ve_9.2-1.iso"
sudo dd if="$ISO" of=/dev/sda bs=1M conv=fdatasync status=progress
```

```text {.no-copy}
… bytes (1.7 GB, 1.6 GiB) copied, … s, … MB/s
…
```

</div>

Then flush caches:

<div class="run" markdown>

```bash
sync
echo synced
```

```text {.no-copy}
synced
```

</div>

!!! tip "Password prompt"
    `dd` to a block device needs root. Run it in a normal terminal so `sudo` can
    prompt. Agent/CI shells without a TTY cannot enter that password for you.

!!! tip "Windows"
    Prefer [balenaEtcher](https://etcher.balena.io/), or
    [Rufus](https://rufus.ie/) in **DD mode**. Decline alternate GRUB downloads
    if Rufus asks. Still confirm you picked the stick, not the system drive.

## 5. Eject

<div class="run" markdown>

```bash
sudo eject /dev/sda || true
echo ready_to_boot
```

```text {.no-copy}
ready_to_boot
```

</div>

## Next

[Install](install.md) — boot the target machine from this USB.
