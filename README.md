# Handess Wallet - Public Beta

**Handess Wallet** is a self-custody desktop wallet for **Handshake (HNS)** with a domain CRM, a peer-to-peer name marketplace, optional 2-of-3 escrow, and atomic swaps between **HNS and DigiByte (DGB)**.

> **Beta software. Unsigned build. Use small amounts.**
>
> These packages are not code-signed and have not been independently audited. Windows SmartScreen will warn you. Linux packages are not GPG-signed. Do not use them with funds you cannot afford to lose.

This repository contains release downloads and metadata. The GPLv3 source code is published at <https://github.com/8t9-dev/handess-wallet>.

---

## Download

| Version | Platform | File | SHA-256 |
| --- | --- | --- | --- |
| **0.3.11 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.3.11.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-Wallet-0.3.11.msi)** (97.8 MB) | `2217E01B557C36500D5F9E675256484EC0B92FB3348FCB2D4E0EB75EC2640186` |
| **0.3.11 beta** | Linux x64 (portable) | **[Handess-Wallet-0.3.11-x86_64.AppImage](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-Wallet-0.3.11-x86_64.AppImage)** (122.2 MB) | `89A54B5E32E49D1F81A76548639B781CEDCF2BB7C4269080DFA88FF762CD0041` |
| **0.3.11 beta** | Ubuntu / Debian x64 | **[Handess-Wallet-0.3.11-amd64.deb](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-Wallet-0.3.11-amd64.deb)** (82.6 MB) | `57A4656D8B894B8EEF1BC582E5C495478430406E884258A600D13FAFFED3283C` |
| **0.3.11 beta** | Fedora / RHEL x64 | **[Handess-Wallet-0.3.11-x86_64.rpm](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-Wallet-0.3.11-x86_64.rpm)** (81.0 MB) | `56C275D2F4DA928FB13B4AE5372348FDB136073CEEA27A83C9E559DB28F1165D` |
| **Optional runtime** | Windows x64 | **[Handess-HSD-Runtime-8.0.0-win-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-HSD-Runtime-8.0.0-win-x64.zip)** (79.5 MB) | `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA` |
| **Optional runtime** | Linux x64 | **[Handess-HSD-Runtime-8.0.0-linux-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.11/Handess-HSD-Runtime-8.0.0-linux-x64.zip)** (53.3 MB) | `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE` |

The app packages contain the wallet only. The optional runtime contains Node.js 24.15.0 and hsd 8.0.0 and is used by the setup wizard for **New to Handshake** users. Existing Node.js or HSD installations are detected and can be reused.

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.3.10 and earlier are superseded.** 0.3.11 fixes marketplace purchases that could not be paid while a name transfer was in progress. 0.3.10 fixed DGB Electrum connections that failed with `use server.version to identify client` on ElectrumX 2.0 servers. 0.3.9 relicensed Handess Wallet under **GPLv3** and published the source code; the proprietary EULA acceptance gate is removed, and the license is available in **Settings → About & Legal**. 0.3.8 fixed the swap progress display, added opt-in auto-complete for accepted swaps, and added a DGB reconnect button. 0.3.7 added HNS claim diagnostics and recovery, including the HNS transaction format fix that made claims valid.

---

## Before you install

1. **Windows 10/11 x64, or Linux x64.**
2. **Back up your seed phrase.** Handess never sends it anywhere and cannot recover it for you.
3. **Close Bob Wallet or any other wallet** using ports `12037`/`12039` before starting Handess's node.
4. **Review the license.** Handess Wallet is free/open-source beta software distributed under the GNU GPL v3 (or later). The license text is available in the app under **Settings → About & Legal**.

New users can use the setup wizard to install a private Node.js + HSD runtime. Existing Handshake users can point the wizard at their existing HSD data directory and avoid a new chain sync.

### Linux requirements

- x64 only.
- Built on Ubuntu 22.04; intended baseline is Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases.
- Ubuntu 24.04 AppImage users may need `libfuse2t64`. The `.deb` is recommended on Ubuntu/Debian.
- The `.rpm` is built for Fedora/RHEL-family systems but has not been installed on every distribution release. Use the AppImage as a fallback.

---

## Install

### Windows

1. Download the MSI and verify its SHA-256 (below).
2. Run it. If Windows shows **"Windows protected your PC"**:
   - Click **More info**.
   - Click **Run anyway**.
3. Launch **Handess Wallet**.
4. On first launch, choose a setup path:
   - **New to Handshake** — download the private Node.js + HSD runtime and choose a new HSD data folder.
   - **I already have Node.js** — use your Node.js with a private HSD runtime.
   - **I already have HSD** — use your existing HSD data folder (recommended for existing Handshake users).
5. Create a wallet or import an existing HNS seed phrase, set a passphrase, and log in.
6. Wait for the node to finish syncing before sending funds.

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.11-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.11-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.11-x86_64.rpm
```

### AppImage (portable)

```bash
chmod +x Handess-Wallet-0.3.11-x86_64.AppImage
./Handess-Wallet-0.3.11-x86_64.AppImage
```

If FUSE is unavailable, run:

```bash
./Handess-Wallet-0.3.11-x86_64.AppImage --appimage-extract-and-run
```

The setup wizard can be re-run later from **Settings → Node & HSD setup**.

### Upgrading from 0.3.10

There is **no auto-updater**. Download and install 0.3.11 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

Purchases that are waiting on a transfer should update after upgrading: open **My Purchases** and let the status refresh. A transfer that is in progress on-chain should show **Transfer detected** and enable **Pay seller**.

The DGB Electrum handshake fix from 0.3.10, the GPLv3 license and public source from 0.3.9, and the auto-complete toggle, per-swap opt-in, locked-wallet pause/resume, and DGB reconnect button from 0.3.8 are retained.

---

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.11.msi -Algorithm SHA256
Get-FileHash .\Handess-HSD-Runtime-8.0.0-win-x64.zip -Algorithm SHA256
```

### Linux

```bash
sha256sum -c SHA256SUMS.txt
```

The same checksums are attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.3.11

- **Marketplace transfer detection fixed** — Handshake has no `TRANSFER` name state. A transferring name stays `CLOSED` with a non-zero `transfer` height, and hsd pays the transfer coin back to the seller while encoding the buyer in the `TRANSFER` covenant. 0.3.11 treats `transfer > 0` as an active transfer, decodes the covenant to verify the buyer, and scopes matching by the name hash so an unrelated transfer to the same address cannot be attributed to another name.
- **Pay seller enabled from the chain** — the button is enabled as soon as the on-chain transfer is verified, even if a Nostr relay update was missed, and the payment path re-checks the chain before sending.
- **Finalization detected** — the owner coin is read through hsd's `coin/:hash/:index` endpoint. The previous `getcoin` JSON-RPC call does not exist on hsd's HTTP API, so finalization was never detected.
- **Purchase polling** — pending, transferred, and paid purchases are refreshed from the chain instead of relying only on the seller's status event, with clearer states for transfer, cancellation, and finalization.
- **Everything from 0.3.10**, including the DGB Electrum `server.version` handshake fix for ElectrumX 2.0.
- **Everything from 0.3.9**, including the GPLv3 license, removal of the proprietary EULA gate, the published source code, and updated legal notices.
- **Everything from 0.3.8**, including the completed-progress fix, opt-in auto-complete swaps, per-swap auto opt-in, locked-wallet pause/resume, and the DGB reconnect button.
- **Everything from 0.3.7**, including the HNS transaction format fix, claim diagnostics, on-chain verification, and rebroadcast recovery.
- **Everything from 0.3.6**, including the DGB signing fix that signs the raw sighash instead of a double hash.
- **Everything from 0.3.5**, including the shared-offer-ID swap event fix that lets the DGB side apply terms and HNS funding.
- **Everything from 0.3.4**, including the pure-JS SHA3 fix for HNS HTLC/escrow address derivation.
- **Everything from 0.3.3 and earlier**, including the DGB Electrum notification crash fix, Linux packages, the DGB WIF QR, Finalize All, per-name transaction-history rows, received-name fee handling, and copyable transaction hashes.

> **Before rebroadcasting:** select the HNS wallet that should receive the swap first. If no earlier claim address was saved, the rebroadcast pays the currently selected wallet.

---

## Beta limitations

- **Unsigned and unaudited.** Windows SmartScreen will warn you. Linux packages are not GPG-signed.
- **Windows x64 and Linux x64 only** in this release. macOS is not published yet.
- **Linux RPM testing is limited.** The `.rpm` is built but has not been installed on every Fedora/RHEL/openSUSE release; the AppImage is the portable fallback.
- **Ubuntu 24.04 AppImage** may need `libfuse2t64`; use the `.deb` if FUSE is unavailable.
- **No bundled Node.js or hsd in the app package.** The setup wizard can download an optional private runtime; existing Node.js/HSD installations can be reused.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **A fresh HSD chain is large and slow to sync.** Reuse existing HSD data where possible.
- **Swaps are beta.** A funded HNS ↔ DGB swap has completed end-to-end, but payout delivery can still require the recovery workflow from the 0.3.7 release notes. Use small amounts where possible. Escrow has not completed a funded end-to-end trade on both chains yet.
- **Auto-complete is opt-in and local.** Both PCs must run 0.3.11 and enable the toggle for a hands-off swap. HNS funding pauses if the wallet is locked; the passphrase must be entered once. Refunds remain manual, and offers are never auto-accepted.
- **DGB-side payouts use the selected HNS wallet.** Auto-complete claims HNS to the currently selected HNS wallet. Select the intended wallet before enabling it.
- **Escrow is non-custodial but not risk-free.** The arbiter can collude with either party, lost keys can deadlock funds, and there is no automatic arbiter notification.
- **Marketplace and swaps depend on Nostr relays.** Purchase payment uses the Handshake chain as the source of truth for transfer state.
- **DGB support is for swaps only.** There is no general DGB send screen.
- **Electron 28 is end-of-life.** A dependency refresh is in progress.
- **No hardware-wallet support.**

---

## Reporting bugs

Open an issue: <https://github.com/8t9-dev/handess-wallet/issues>

Please include the app version, operating system and version, and the exact error text.

**Never post seed phrases, private keys, WIFs, passphrases, API keys, or signed PSBTs.** Redact addresses and transaction IDs if needed.

---

## License and source

Handess Wallet is **free/open-source software** distributed under the **GNU General Public License, version 3 or (at your option) any later version** (`GPL-3.0-or-later`).

Copyright (C) 2026 **8t9.dev**.

You may use, modify, and redistribute the software under the GPL. The corresponding source code is published at <https://github.com/8t9-dev/handess-wallet>. The license text is available in this repository as [LICENSE](LICENSE), is included in the app package as `LICENSE`, and can be reviewed in the app under **Settings → About & Legal**.

Third-party components remain under their own licenses; their notices are included in this repository and the app package as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The optional runtimes include Node.js and hsd license files.

The **Handess** name and logo are branding of 8t9.dev and are not licensed under the GPL. Modified versions should use different branding.
