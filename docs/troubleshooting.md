# Troubleshooting

| Symptom | Likely cause | What to try |
| --- | --- | --- |
| No boot menu / USB ignored | Legacy vs UEFI, bad stick | Re-flash USB; force UEFI boot entry |
| Graphical installer blank or garbled | GPU / mode issues | Use **Terminal UI**; try `nomodeset` |
| No NIC in network step | Cable / driver / link down | Plug Ethernet; check link lights; try another NIC |
| Wrong disk wiped | Selected USB or wrong drive | Restore from backup; always confirm disk names |
| Mirror shows `DEGRADED` | Disk missing or failing | Reseat or replace; run `zpool status` |
| Mirror smaller or slower than expected | Size and speed follow the weaker disk | Expected for mixed NVMe + SATA |
| Installer failure mid-copy | Media / disk / hardware | Ctrl+Alt+F2 for logs; re-flash USB; check SMART |
| No `/dev/kvm` | VT-x / AMD-V off | Enable virtualization in firmware; reboot |
| Web UI timed out | Wrong IP / HTTP / VLAN | Use `https://IP:8006`; same LAN; check `ip -br a` and `systemctl status pveproxy` |
| Browser certificate warning | Self-signed cert | Expected; proceed for lab hosts |
| `apt update` 401 on enterprise | No subscription key | Disable enterprise; enable `pve-no-subscription` |
| Hostname regrets | FQDN hard to rename | Rebuild or follow Proxmox hostname-change docs carefully |
| Subscription dialog every login | No paid key | Click **OK**; cosmetic only |

## Next

[References](references.md)
