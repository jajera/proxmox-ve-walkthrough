# Prove web UI

From the **workstation** (replace the IP):

<div class="run" markdown>

```bash
curl -kI https://192.168.1.60:8006/
```

```text {.no-copy}
HTTP/1.1 200 OK
…
```

</div>

## Browser login

1. Open `https://192.168.1.60:8006/` in a browser.
2. Accept the self-signed certificate warning (expected on a fresh install).
3. Log in as **root** with realm **Linux PAM standard authentication**.
4. When **No valid subscription** appears, click **OK**. It shows once per
   login, is cosmetic, and does not lock features.

## Repositories without a subscription

Enterprise repos return authentication errors without a key. Switch to the
no-subscription repo (Proxmox VE 9 / Debian Trixie uses the modern
`deb822` `.sources` format).

### UI path

1. Select the node → **Updates** → **Repositories**.
2. Disable the **Enterprise** repository (and any other enterprise Ceph
   entries you are not using).
3. Add / enable **No-Subscription** (`pve-no-subscription`).
4. Back on **Updates**, **Refresh**, then **Upgrade**.

### CLI path

Disable enterprise by editing the enterprise sources file (exact filename may
be `pve-enterprise.sources` on a fresh 9.x install):

<div class="run" markdown>

```bash
ls /etc/apt/sources.list.d/
```

```text {.no-copy}
debian.sources
pve-enterprise.sources
…
```

</div>

Create or edit `/etc/apt/sources.list.d/proxmox.sources`:

```text
Types: deb
URIs: http://download.proxmox.com/debian/pve
Suites: trixie
Components: pve-no-subscription
Signed-By: /usr/share/keyrings/proxmox-archive-keyring.gpg
```

Disable the enterprise entry (UI toggle, or add `Enabled: false` to that
stanza). Then:

<div class="run" markdown>

```bash
apt-get update
apt-get -y dist-upgrade
pveversion
```

```text {.no-copy}
…
pve-manager/9.2-…/…
```

</div>

Confirm the node summary in the UI shows a current `pve-manager` version and
no repository error badges.

!!! note "Consuming labs"
    Record `PVE_HOST` (and credentials) in that project’s env file. Never commit
    passwords or API tokens. Guest installs stay in the consuming walkthrough.

## Done

You have a web-UI-reachable Proxmox VE host. See
[Troubleshooting](troubleshooting.md) if something failed.
