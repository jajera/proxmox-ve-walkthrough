# Download ISO

On the **workstation**, not the target machine. This guide uses the
**Proxmox VE 9.2** installer. A newer point release works if you change the
filename and hash.

Sources: [ISO downloads](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso)
· [mirror](http://download.proxmox.com/iso/)

## 1. Fetch

<div class="run" markdown>

```bash
mkdir -p "$HOME/Downloads" && cd "$HOME/Downloads"
ISO=proxmox-ve_9.2-1.iso
curl -fL --continue-at - --progress-bar -o "$ISO" \
  "http://download.proxmox.com/iso/$ISO"
ls -lh "$ISO"
```

```text {.no-copy}
-rw-rw-r-- 1 you you 1.6G … proxmox-ve_9.2-1.iso
```

</div>

## 2. Verify SHA256

<div class="run" markdown>

```bash
echo "4e88fe416df9b527624a175f24c9aa07c714d3332afb1ee3dbf3879573ef2c6c  $ISO" \
  | sha256sum -c
```

```text {.no-copy}
proxmox-ve_9.2-1.iso: OK
```

</div>

!!! danger "FAILED means stop"
    Delete the ISO and download again. Never write a bad image to USB.

## 3. Optional: check the signature

Release keys are published at
[enterprise.proxmox.com/debian](https://enterprise.proxmox.com/debian/).

<div class="run" markdown>

```bash
curl -fL -o proxmox-release-trixie.gpg \
  https://enterprise.proxmox.com/debian/proxmox-release-trixie.gpg
curl -fL -o "$ISO.asc" "http://download.proxmox.com/iso/$ISO.asc"
gpg --show-keys ./proxmox-release-trixie.gpg
```

```text {.no-copy}
pub   …
      24B30F06ECC1836A4E5EFECBA7BCD1420BFE778E
uid   …
```

</div>

The fingerprint must match the one above. Then:

<div class="run" markdown>

```bash
gpgv --keyring ./proxmox-release-trixie.gpg "$ISO.asc" "$ISO"
```

```text {.no-copy}
gpgv: Good signature from …
```

</div>

## Next

[Write USB](write-usb.md){ .md-button }
