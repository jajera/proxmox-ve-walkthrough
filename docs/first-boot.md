# First boot

The console prints the management URL. Note it.

![Console banner with the management URL](assets/first-boot/01-console-banner.jpg)

Log in as `root` with the install password. Your IP will differ; this guide's
examples use `192.168.1.60`.

## Check the host

```bash
hostnamectl; ip -br a; pveversion
systemctl is-active pveproxy pvedaemon
test -e /dev/kvm && echo kvm_ok
```

![Host checks: vmbr0 has the IP, services active, kvm_ok](assets/first-boot/03-host-checks.jpg)

You want `vmbr0` with your IP, two `active`, and `kvm_ok`. No `kvm_ok` means
VT-x / AMD-V is off in firmware.

## Check storage

=== "Two disks: ZFS mirror"

    ```bash
    zpool status
    ```

    ![zpool status, mirror-0 and both disks ONLINE](assets/first-boot/02-zpool-status.jpg)

    Both disks `ONLINE`. `DEGRADED` means a disk is missing or failing.

=== "One disk: ext4"

    <div class="run" markdown>

    ```bash
    lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT
    pvs
    ```

    ```text {.no-copy}
    NAME               SIZE FSTYPE …
    …
    pve                …
    ```

    </div>

    The `pve` volume group sits on the install disk.

## Next

[Update host](update-host.md){ .md-button }
