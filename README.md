# Infra Inventory

Portable infrastructure master inventory for a data-center team: **one Excel workbook is the database, one self-contained HTML page is the dashboard.** No server, no install, no modules. It runs offline in Edge or Chrome, also on locked-down Windows Server 2012 R2 hosts.

## What it does

- **Inventory:** ServersMI (virtual and physical), ESXi hosts, storage arrays and volumes, datastores, backup, other devices, lifecycle, patching, relations between everything.
- **Data center:** DCMI, DC checks, open issues, rack layout and designer, ports and cabling, IP & VLANs (Subnets).
- **Failover clusters:** nodes, roles, resources, cluster disks (VMDK / RDM / direct), network interfaces. Per server: disks and interfaces cards; `Disk GB` is always the total of all its disks.
- **Network interface purposes:** detected by rules (Subnets list, ServersMI `IP Primary`, `<host>p` backup name, cluster network, not-routed heartbeat). The reason is stored in `Detected by`; hand edits are kept as `Manual`.
- **Collectors (PowerShell, read-only):** WSFC (WinRM 5985 or DCOM), vCenter, 3PAR/Primera, iLO/BMC, OneView. Schedule daily, then *Review and import* in the page. Plus smart import for RVTools and your own CSV/Excel files (cluster-aware shared-disk handling).
- **Work tools:** Stuff (files, links, daily check list), **RDS Generator** (Remote Desktop Connection Manager `.rdg` from ServersMI, grouped by Env/Role, account per group, no certificate warning), To do & notes (personal and shared), knowledge base, activity log linked to operators.
- **Security:** optional **login portal with an encrypted workbook** (AES-256-GCM, PBKDF2-SHA256 250,000 rounds, one global admin plus a login per operator).
- Light, dark and AMOLED themes, saved views, edit-in-place lists, merge and backup restore, change review before saving.

## Quick start

1. Copy the `InfraInventory/` folder anywhere (a network share works).
2. Rename `Inventory/inventory.xlsx.template` to `Inventory/inventory.xlsx` (empty starter workbook).
3. Open `index.html` in Edge or Chrome, click **Connect project folder**, pick that folder.
4. Settings: add operators, environments and rack device types. Then add data or run a collector.

*Save to Excel* writes the workbook in place and refreshes `index.html` itself. Details: `InfraInventory/README.txt` and `InfraInventory/DataImport/README.txt`.

## Repository layout

| Path | Content |
|---|---|
| `InfraInventory/` | Ready-to-use package (what you deploy) |
| `InfraInventory/DataImport/` | Collector scripts, targets, credential helpers |
| `src/` | Page source in parts (`p*.js`, `p1.html`, CSS) |
| `tools/build.py` | Builds `index.html` and `dashboard-template.html` from `src/` (`python3 tools/build.py`) |
| `tests/` | Playwright tests (`NODE_PATH=$(npm root -g) node tests/pw133.js`) |
| `docs/CHANGELOG.txt` | Full change history |

Change a collector script, then run `python3 tools/build.py`: the scripts are embedded in the page so the "View / copy script" buttons stay current.

## Security notes

- The workbook is encrypted only when you turn on *Settings → Security*. Attachments, Stuff files and collector output stay unencrypted: protect the folder with NTFS rights.
- Roles are a display rule of the page. A user who has a login can decrypt the file. Removing a login does not close copies made earlier.
- **A lost admin password with no other working login means the data cannot be recovered.**
- Never commit `inventory.xlsx`, backups, `Credentials/` or collector output (see `.gitignore`).
- Collectors were tested against simulated systems. Run each by hand against one real target first.

## Known limits

- Edge or Chrome required for writing into the project folder; other browsers only download the file.
- RDCMan stores passwords encrypted with the Windows login, so the RDS Generator writes user names and domains only.
- The `.rdg` certificate option (`authentication` = 0) was written from knowledge of the file format; verify it once in RDCMan.
- `Collect-WSFC.ps1` DNS/gateway reading was reviewed statically; test it on one cluster first.

## Third-party

SheetJS Community Edition (`InfraInventory/vendor/`, Apache-2.0, license included). Geist and Geist Mono fonts are embedded (SIL OFL). Add your own `LICENSE` for the rest before publishing.
