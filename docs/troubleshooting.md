# Troubleshooting

| Symptom | Likely cause | What to try |
| --- | --- | --- |
| No boot menu / USB ignored | Legacy vs UEFI, bad stick | Re-flash USB; force **UEFI** boot entry; confirm `cmp` → `MATCH` |
| `dd` looks stuck after progress | `conv=fdatasync` flushing | Wait; quiet period can be several minutes on slow USB |
| `cmp` → `MISMATCH` | Incomplete write / wrong device | Check `$DEV` is the stick, re-run `dd`, then `cmp` again |
| Graphical installer blank or garbled | GPU / mode issues | Use **Terminal UI**; try `nomodeset` |
| No NIC in network step | Cable / driver / link down | Plug Ethernet; check link lights; pick a NIC marked **UP** |
| Wrong disk wiped | Selected USB or wrong drive | Restore from backup; never choose the installer stick |
| Mirror shows `DEGRADED` | Disk missing or failing | Reseat or replace; run `zpool status` |
| Mirror smaller or slower than expected | Size and speed follow the weaker disk | Expected for mixed NVMe + SATA |
| Installer failure mid-copy | Media / disk / hardware | **Ctrl+Alt+F2** for logs; re-flash USB; check SMART |
| No `/dev/kvm` | VT-x / AMD-V off | Enable virtualization in firmware; reboot |
| Web UI timed out | Wrong IP / HTTP / VLAN | Use `https://IP:8006`; same LAN; check `ip -br a` and `systemctl status pveproxy` |
| Browser certificate warning | Self-signed cert | Expected on a fresh host; proceed for lab use |
| `curl -kI` returns **501** | API rejects HEAD | Use GET: `curl -k -sS -o /dev/null -w "%{http_code}\n" https://IP:8006/` |
| `apt update` **401** on enterprise | No subscription key | Disable enterprise; enable `pve-no-subscription` ([Update host](update-host.md)) |
| `Malformed stanza` in `.sources` | Blank line before `Enabled:` | Keep `Enabled: false` inside the same deb822 stanza |
| Hostname regrets | FQDN hard to rename | Rebuild, or see [Renaming a PVE node](https://pve.proxmox.com/wiki/Renaming_a_PVE_node) |
| Router has no DNS domain | DHCP **Domain** empty | Pick a local suffix (e.g. `home.arpa`) for the installer FQDN |
| Subscription dialog every login | No paid key | Click **OK**; cosmetic only |
| “no-subscription not for production” badge | Free repo warning | Expected on the default path; keep enterprise if you have a key |

## Next

[References](references.md)
