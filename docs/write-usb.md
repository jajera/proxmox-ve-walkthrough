# Write USB

!!! danger "The stick is erased"
    Pick the removable USB disk. Never an internal NVMe or SATA disk.

UNetbootin does not work with this image. CD/DVD and PXE also work; USB is the
easy path.

## 1. Find the stick

<div class="run" markdown>

```bash
lsblk -o NAME,SIZE,TYPE,FSTYPE,MODEL,TRAN,RM
```

```text {.no-copy}
NAME          SIZE TYPE FSTYPE MODEL            TRAN   RM
sda          28.9G disk        USB DISK 2.0     usb     1
└─sda1       28.9G part vfat                    usb     1
nvme0n1     931.5G disk        Samsung SSD …    nvme    0
├─nvme0n1p1     1G part vfat                    nvme    0
└─nvme0n1p2 930.5G part ext4                    nvme    0
```

</div>

The stick is the **disk** row with `TRAN=usb` and `RM=1` (here `sda`).

## 2. Safety gate

Set the device once. Every later command uses it.

<div class="run" markdown>

```bash
DEV=/dev/sda   # change to your stick
eval "$(lsblk -dP -o NAME,SIZE,TYPE,MODEL,TRAN,RM "$DEV")"
test "$TRAN" = usb && test "$RM" = 1 && test "$TYPE" = disk \
  && echo "safe_to_flash: $NAME $SIZE $MODEL" || echo REFUSE
```

```text {.no-copy}
safe_to_flash: sda 28.9G USB DISK 2.0
```

</div>

`REFUSE` means stop and re-check `lsblk`.

## 3. Write

<div class="run" markdown>

```bash
ISO="$HOME/Downloads/proxmox-ve_9.2-1.iso"
sudo umount "$DEV"?* 2>/dev/null || true
sudo dd if="$ISO" of="$DEV" bs=1M conv=fdatasync status=progress
sync
```

```text {.no-copy}
1706178560 bytes (1.7 GB, 1.6 GiB) copied, 469 s, 3.6 MB/s
```

</div>

!!! note "Quiet at the end is normal"
    After the progress line reaches 100%, `dd` flushes to the stick for a few
    minutes. Wait for the prompt to return.

!!! tip "Windows"
    Use [balenaEtcher](https://etcher.balena.io/), or
    [Rufus](https://rufus.ie/) in **DD mode**.

## 4. Confirm and eject

<div class="run" markdown>

```bash
sudo cmp -n "$(stat -c %s "$ISO")" "$ISO" "$DEV" && echo MATCH || echo MISMATCH
sudo eject "$DEV" || true
```

```text {.no-copy}
MATCH
```

</div>

`MISMATCH` means write again.

## Next

[Install](install.md){ .md-button }
