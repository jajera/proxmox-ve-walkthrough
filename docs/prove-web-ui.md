# Prove web UI

From the **workstation**. Use the IP from [First boot](first-boot.md).

<div class="run" markdown>

```bash
curl -k -sS -o /dev/null -w "%{http_code} %{server}\n" https://192.168.1.60:8006/
```

```text {.no-copy}
200 pve-api-daemon/3.0
```

</div>

Use a GET as above. `curl -I` can return 501 on this API.

## 1. Log in

Open `https://<ip>:8006/` and accept the self-signed certificate warning.
Username `root`, realm **Linux PAM standard authentication**.

![Login dialog: root, Linux PAM realm](assets/web-ui/01-login.jpg)

## 2. Dismiss the subscription notice

It appears at every login, is cosmetic, and locks nothing.

![No valid subscription dialog, OK](assets/web-ui/02-subscription.jpg)

## 3. Node online

Select **pve**. The tooltip reads **Status: online**, and `local` plus the
install storage are listed (`local-zfs` on the ZFS layout, `local-lvm` on
ext4).

![Node pve online with local and local-zfs storage](assets/web-ui/03-node-summary.jpg)

## 4. Repositories

**Node → Updates → Repositories**. Enterprise entries are disabled and the
no-subscription repo is enabled.

![APT Repositories: updates available, enterprise disabled](assets/web-ui/04-repositories.jpg)

The yellow **no-subscription is not recommended for production** warning is
expected on this path.

!!! note "Consuming labs"
    Keep `PVE_HOST` and credentials in that project's env file. Never commit
    passwords or API tokens.

## Done

You have an updated Proxmox VE host with a working web UI. Something wrong?
See [Troubleshooting](troubleshooting.md).
