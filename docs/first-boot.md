# First boot

After reboot the console shows a banner with the management URL, for example:

```text
https://192.168.1.60:8006/
```

Log in as `root` with the password you set at install.

## Check the host

<div class="run" markdown>

```bash
hostnamectl
ip -br a
pveversion
systemctl is-active pveproxy pvedaemon
test -e /dev/kvm && echo kvm_ok
```

```text {.no-copy}
Static hostname: pve
…
vmbr0            UP             192.168.1.60/24 …
pve-manager/9.2-…/… (running version: …)
active
active
kvm_ok
```

</div>

Note the LAN IPv4 on `vmbr0` for the next page. If `kvm_ok` is missing, enable
Intel VT-x or AMD-V in firmware and reboot.

## Confirm storage layout

=== "Two disks: ZFS mirror"

    <div class="run" markdown>

    ```bash
    zpool status
    ```

    ```text {.no-copy}
      pool: rpool
     state: ONLINE
    …
    	NAME        STATE
    	rpool       ONLINE
    	  mirror-0  ONLINE
    	    …       ONLINE
    	    …       ONLINE
    ```

    </div>

    Both legs of `mirror-0` should be `ONLINE`. `DEGRADED` means a disk is
    missing or failing — reseat or replace it, then re-check.

=== "One disk: ext4"

    <div class="run" markdown>

    ```bash
    lsblk -o NAME,SIZE,FSTYPE,MOUNTPOINT
    pvs
    ```

    ```text {.no-copy}
    NAME               SIZE FSTYPE …
    …                  …
    pve                …
    ```

    </div>

    You should see the `pve` volume group on the install disk.

## Next

[Prove web UI](prove-web-ui.md)
