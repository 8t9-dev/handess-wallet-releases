# Handess Wallet - Public Beta

**Handess Wallet** is a self-custody desktop wallet for **Handshake (HNS)** with a domain CRM, a peer-to-peer name marketplace, optional 2-of-3 escrow, and atomic swaps between **HNS and DigiByte (DGB)**.

> **Beta software. Unsigned build. Use small amounts.**
>
> This installer is not code-signed and has not been independently audited. Windows SmartScreen will warn you. Do not use it with funds you cannot afford to lose.

This repository contains release downloads only. The source is not published.

---

## Download

| Version | Platform | File | SHA-256 |
| --- | --- | --- | --- |
| **0.3.0 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.3.0.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.0/Handess-Wallet-0.3.0.msi)** (97.8 MB) | `D268D9D18A43B1E853173E92BFC47CAB312ECB9DA1FA085E14FEA7228AE1DB29` |
| **Optional runtime** | Windows 10/11 x64 | **[Handess-HSD-Runtime-8.0.0-win-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.0/Handess-HSD-Runtime-8.0.0-win-x64.zip)** (79.5 MB) | `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA` |

The MSI is the wallet only. The optional runtime contains Node.js 24.15.0 and hsd 8.0.0 and is downloaded by the setup wizard for **New to Handshake** users. Existing Node.js or HSD installations are detected and can be reused.

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.2.2 and earlier are superseded.** Use 0.3.0.

---

## Before you install

1. **Windows 10 or 11, 64-bit.**
2. **Back up your seed phrase.** Handess never sends it anywhere and cannot recover it for you.
3. **Close Bob Wallet or any other wallet** using ports `12037`/`12039` before starting Handess's node.
4. **Review the license.** Handess Wallet is proprietary beta software distributed under the Handess Wallet EULA. The app shows the EULA on first launch and requires acceptance before use.

New users do **not** need to install Node.js or hsd manually. The setup wizard can install a private runtime. Existing Handshake users can point the wizard at their existing HSD data directory and avoid a new chain sync.

---

## Install

1. Download the MSI and verify its SHA-256 (below).
2. Run it. If Windows shows **"Windows protected your PC"**:
   - Click **More info**.
   - Click **Run anyway**.
3. Launch **Handess Wallet**.
4. Review and accept the **End User License Agreement**.
5. On first launch, choose a setup path:
   - **New to Handshake** — download the private Node.js + HSD runtime and choose a new HSD data folder.
   - **I already have Node.js** — use your Node.js with a private HSD runtime.
   - **I already have HSD** — use your existing HSD data folder (recommended for existing Handshake users).
6. Create a wallet or import an existing HNS seed phrase, set a passphrase, and log in.
7. Wait for the node to finish syncing before sending funds.

The setup wizard can be re-run later from **Settings → Node & HSD setup**.

### Upgrading from 0.2.2

There is **no auto-updater**. Download and run the 0.3.0 MSI; it upgrades the installed app in place. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the wizard is skipped and your existing HSD settings remain in use.

---

## Verify the downloads

PowerShell:

```powershell
Get-FileHash .\Handess-Wallet-0.3.0.msi -Algorithm SHA256
Get-FileHash .\Handess-HSD-Runtime-8.0.0-win-x64.zip -Algorithm SHA256
```

Expected:

```text
MSI:     D268D9D18A43B1E853173E92BFC47CAB312ECB9DA1FA085E14FEA7228AE1DB29
Runtime: 332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA
```

The same checksums are attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.3.0

- **First-run setup wizard** — private Node.js + HSD runtime download, existing Node.js detection, existing HSD detection, and an HSD data-folder picker with existing-chain detection.
- **Search Engine refresh settings** — scan the last 1,000, 10,000, 50,000, or 105,120 blocks (105,120 = two years). Refreshes are resource-aware, show progress, can be cancelled, and persist the completed height. The manual Add-tag input was removed; tag chips and filters remain.
- **Explore resource records** — every TXT record and chunk, every DS record with full fields, and NS/GLUE/SYNTH values. Explorer dates use actual block timestamps.
- **Auctions** — corrected mainnet/testnet/regtest phase schedules; Global Auctions Overview is bidding-only, split into ending-soon and bid-activity halves with 50 names per page.
- **My Domains tags** — on-chain TXT tags shown comma-separated, CSV export/import for bulk tag edits, and non-tag TXT preservation.
- **Marketplace and escrow unchanged** — the seller selects the arbiter, and the arbiter is not automatically notified. Dispute PSBTs are delivered manually and signed with the offline `arbiter:sign` tool.
- **Everything from 0.2.2**, including the HNS wallet, domain CRM, auctions, marketplace, escrow, HNS ↔ DGB swaps, DGB swap wallet, explorer, History fixes, and dark mode.

---

## Beta limitations

- **Unsigned and unaudited.** SmartScreen will warn you.
- **Windows x64 only** for this release. macOS and Linux targets are configured but not published.
- **No bundled Node.js or hsd in the MSI.** The setup wizard can download an optional private runtime; existing Node.js/HSD installations can be reused.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **A fresh HSD chain is large and slow to sync.** Reuse existing HSD data where possible.
- **Swaps and escrow have not completed a funded end-to-end trade on both chains yet.** Use small amounts on testnet/regtest where possible.
- **Escrow is non-custodial but not risk-free.** The arbiter can collude with either party, lost keys can deadlock funds, and there is no automatic arbiter notification.
- **Marketplace and swaps depend on Nostr relays.**
- **DGB support is for swaps only.** There is no general DGB send screen.
- **Electron 28 is end-of-life.** A dependency refresh is in progress.
- **No hardware-wallet support.**

---

## Reporting bugs

Open an issue: <https://github.com/8t9-dev/handess-wallet-releases/issues>

Please include the app version, Windows version, and the exact error text.

**Never post seed phrases, private keys, WIFs, passphrases, API keys, or signed PSBTs that contain sensitive data.** Redact addresses and transaction IDs if needed.

---

## License and source

Handess Wallet is **proprietary beta software** distributed under the **Handess Wallet End User License Agreement (EULA)**. It is free to download and use for personal or internal business purposes.

The EULA does **not** permit modifying, editing, forking, creating derivative works from, or redistributing the app. The source is not published, and no source-code license is granted. The Handess name and logo may not be used for modified versions.

The EULA is available in this repository as [LICENSE](LICENSE), is included in the installer as `LICENSE`, is shown on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party components remain under their own licenses; their notices are included in this repository and the installer as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The optional runtime includes Node.js and hsd license files.

If you do not agree to the EULA, do not install or use the app.
