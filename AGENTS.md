# Agent notes

Zensical walkthrough: **Proxmox VE from ISO to web UI ready host**. Generic on
purpose — consumers (Home Assistant labs, other workloads) set hostname, storage
beyond the install tabs, and guest details in their own docs.

## Commands

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/zensical serve
```

## Docs

Reading order matches `zensical.toml` nav. Use `<div class="run" markdown>` for
command + result pairs (see EventBridge / patina `.run` CSS). No “Looks like”
labels.

## Non-negotiables

1. Stay generic — no lab-only hostnames as the only path, no Home Assistant or
   cluster HA steps here.
2. Dictate the **Proxmox VE** ISO and the **graphical** installer (Terminal UI
   only as a fallback).
3. Warn clearly that every selected target disk is erased — both layouts
   (two-disk ZFS mirror and one-disk ext4).
4. Keep the layout as **tabs** only at the three branch points (What you need,
   Install Target Harddisk, First boot). Do not fork the pages.
5. Do not commit ISOs, passwords, API tokens, or private keys.
