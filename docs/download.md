# Download ISO

Work on the **workstation** (not the target machine). This page uses the
**Proxmox VE 9.2** x86_64 installer from the official ISO listing — a newer
point release is fine if you update the filename and hash.

Official landing page:
[Proxmox VE ISO downloads](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso).

Mirror directory:
[download.proxmox.com/iso](http://download.proxmox.com/iso/).

## 1. Working directory

<div class="run" markdown>

```bash
mkdir -p "$HOME/Downloads"
cd "$HOME/Downloads"
pwd
```

```text {.no-copy}
/home/…/Downloads
```

</div>

## 2. Fetch the ISO

<div class="run" markdown>

```bash
ISO=proxmox-ve_9.2-1.iso
BASE=http://download.proxmox.com/iso
curl -fL --continue-at - --progress-bar -o "$ISO" "$BASE/$ISO"
ls -lh "$ISO"
```

```text {.no-copy}
-rw-rw-r-- 1 you you 1.6G … proxmox-ve_9.2-1.iso
```

</div>

`--continue-at -` lets you resume if the transfer drops. Alternate host:
`https://enterprise.proxmox.com/iso/` (same file).

## 3. Verify SHA256

Pin the hash published for this ISO (Proxmox download page, May 2026):

<div class="run" markdown>

```bash
ISO=proxmox-ve_9.2-1.iso
echo "4e88fe416df9b527624a175f24c9aa07c714d3332afb1ee3dbf3879573ef2c6c  $ISO" \
  | sha256sum -c
```

```text {.no-copy}
proxmox-ve_9.2-1.iso: OK
```

</div>

!!! danger "Do not continue on FAILED"
    If you see `FAILED` or a mismatched hash, delete the ISO and download again.
    Do not write a bad image to USB.

## 4. Optional: OpenPGP signature

Each ISO ships with a detached `.asc` signature. Release keys live under
[enterprise.proxmox.com/debian](https://enterprise.proxmox.com/debian/) (not on
public keyservers). For Trixie-based Proxmox VE 9:

<div class="run" markdown>

```bash
ISO=proxmox-ve_9.2-1.iso
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

Confirm the fingerprint is `24B30F06ECC1836A4E5EFECBA7BCD1420BFE778E`, then:

<div class="run" markdown>

```bash
gpgv --keyring ./proxmox-release-trixie.gpg \
  "$ISO.asc" "$ISO"
```

```text {.no-copy}
gpgv: Good signature from …
```

</div>

## Next

[Write USB](write-usb.md)
