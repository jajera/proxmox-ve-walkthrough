# proxmox-ve-walkthrough

Proxmox VE from ISO to web UI ready host.

Docs: [proxmox-ve-walkthrough.johna.kiwi](https://proxmox-ve-walkthrough.johna.kiwi/)

## Docs locally

```bash
python3 -m venv .venv
.venv/bin/pip install -r requirements.txt
.venv/bin/zensical serve
```

Open the URL printed by the server (usually `http://127.0.0.1:8000`).

## Scope

Generic host prep for other labs. Hostname, disk layout choices beyond the
documented tabs, and project-specific follow-ups belong in the consuming
walkthrough — not here.

See [AGENTS.md](AGENTS.md).
