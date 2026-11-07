# Overview

Install **Proxmox VE** on spare x86_64 hardware and prove you can open the web
UI on the LAN. That is the whole guide.

Other walkthroughs link here for host prep, then layer their own hostname,
storage choices beyond the install tabs, guests, and workloads.

## What you will do

1. Download the Proxmox VE installer ISO and verify SHA256
2. Detect the USB stick (`TRAN=usb`), then write the ISO with `dd` (or Etcher)
3. Run the graphical installer with the choices this guide dictates
4. Confirm services on first boot and take the LAN IP
5. Prove the web UI at `https://<ip>:8006`

## What this does not do

- Clustering or high availability
- Ceph or shared storage
- Creating VMs, containers, or workloads (Home Assistant and friends stay in
  consuming labs)
- Buying a subscription (the free no-subscription repo is enough here)

## Next

[What you need](prerequisites.md)
