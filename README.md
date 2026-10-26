# Handess Wallet — Public Beta

**Handess Wallet** is a self-custody desktop wallet for **Handshake (HNS)** with a domain CRM, a peer-to-peer name marketplace, optional 2-of-3 escrow, and atomic swaps between **HNS and DigiByte (DGB)**.

> **Beta software. Unsigned build. Use small amounts.**
>
> This installer is not code-signed and has not been independently audited. Windows SmartScreen will warn you. Do not use it with funds you cannot afford to lose.

This repository contains release downloads only. The source is not published.

---

## Download

| Version | Platform | Installer | SHA-256 |
| --- | --- | --- | --- |
| **0.2.1 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.2.1.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.2.1/Handess-Wallet-0.2.1.msi)** | `24FBD7018C5C717B00648FD5668C6068F6A67810AAF80A9982225D4A6FF0CBFE` |

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.2.0 is superseded.** It can show a false “An hsd node is already running locally” warning after the app quits its managed node, which blocks reconnecting on the next launch. Use **0.2.1**. If you must run 0.2.0, start `hsd` manually first (see [Upgrading from 0.2.0](#upgrading-from-020)).

---

## Before you install

1. **Windows 10 or 11, 64-bit.**
2. **A Handshake node.** The installer does **not** bundle Node.js or `hsd`. Choose one:
   - Install [Node.js](https://nodejs.org/) (20 LTS or newer), then run:

     ```powershell
     npm install -g hsd
     ```

     Handess Wallet can then start and manage `hsd` for you.
   - Or connect to an existing `hsd` node in **Settings → HSD Full Node** using its host, node port (`12037`), wallet port (`12039`), and API key.
3. **Only one node per machine.** Close Bob Wallet or any other wallet that uses ports `12037`/`12039` before starting Handess's node. Bob Wallet's embedded node does not expose its API key, so auto-detection usually will not work.
4. **Back up your seed phrase.** Handess never sends it anywhere and cannot recover it for you.
5. **Review the license.** Handess Wallet is proprietary beta software distributed under the Handess Wallet EULA. The app shows the EULA on first launch and requires acceptance before use.

---

## Install

1. Download the MSI and verify its SHA-256 (below).
2. Run it. If Windows shows **"Windows protected your PC"**:
   - Click **More info**.
   - Click **Run anyway**.
3. Launch **Handess Wallet**.
4. Review and accept the **End User License Agreement**.
5. Create a wallet or import an existing HNS seed phrase, set a passphrase, and log in.
6. Wait for the node to finish syncing before sending funds.

Uninstall from **Settings → Apps → Installed apps → Handess Wallet**, or run the MSI again.

### Upgrading from 0.2.0

0.2.0 has **no auto-updater**. Download and run the 0.2.1 MSI; it upgrades the installed app. Wallet data lives outside the install directory and is preserved by an in-place upgrade. Back up your seed phrase first.

If 0.2.0 is stuck on the false “node already running” warning, start `hsd` manually before launching 0.2.0:

```powershell
hsd --prefix "$env:USERPROFILE\.hsd" --network main --index-tx --index-address
```

Use your own data directory if it is not `%USERPROFILE%\.hsd`. Once the node is listening on `12037` and `12039`, launch 0.2.0 and connect. Installing 0.2.1 removes the need for this workaround.

---

## Verify the download

PowerShell:

```powershell
Get-FileHash .\Handess-Wallet-0.2.1.msi -Algorithm SHA256
```

Expected:

```text
24FBD7018C5C717B00648FD5668C6068F6A67810AAF80A9982225D4A6FF0CBFE
```

The same checksum is attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.2.1

- **Node-detection hotfix** — the app now checks whether the local node and wallet ports are actually listening before showing “An hsd node is already running locally” or trying to attach. This fixes the false warning that appeared after quitting a managed node and relaunching, and after an external node stopped. The check repeats every 5 seconds while disconnected.
- **Clearer connection errors** — missing node or wallet-server ports now produce specific messages instead of a generic API-key/wallet-name error.
- **HNS wallet** — create or import multiple wallets, encrypted passphrase, send/receive with QR codes, fee selection, transaction history, message signing and verification.
- **Domain CRM** — My Domains table, custom tags, expiry timeline, bulk renew / transfer / DNS updates, watchlist, and bulk availability search (paste, CSV, or JSON).
- **Auctions** — open names, place bids, reveal, redeem losing bids, and register won names.
- **Marketplace** — peer-to-peer Handshake name trading over Nostr relays, transfer-first trades, purchase requests, and optional 2-of-3 HNS escrow with an independent arbiter.
- **Atomic swaps** — HNS → DGB and DGB → HNS through HTLCs, with background monitoring, retry, and on-chain secret recovery.
- **DGB swap wallet** — Electrum-DGB connection, address and balance for swaps, and passphrase-gated WIF export.
- **Node management** — start and stop a local `hsd` node, connect to an external node, choose mainnet/testnet/regtest, and adjust advanced node settings.
- **Explorer and history** — search names, blocks, and transactions; browse latest blocks; review wallet history.
- **Dark mode.**

---

## Beta limitations

- **Unsigned and unaudited.** SmartScreen will warn you.
- **Windows x64 only** for this release. macOS and Linux targets are configured but not published.
- **No bundled Node.js or `hsd`.** A clean machine needs Node.js + `hsd`, or an external node.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **Swaps and escrow have not completed a funded end-to-end trade on both chains yet.** The cryptography and scripts are tested, but treat this as experimental. Start with small amounts on testnet/regtest where possible.
- **Escrow is non-custodial but not risk-free.** The arbiter can collude with either party, and lost keys can deadlock funds.
- **Marketplace and swaps depend on Nostr relays.** On-chain recovery exists, but the counterparty still needs the signed terms event.
- **DGB support is for swaps only.** There is no general DGB send screen in this release.
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

The EULA is available in this repository as [LICENSE](LICENSE), is included in the installer as `LICENSE`, is shown on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party components remain under their own licenses; their notices are included in this repository and the installer as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

If you do not agree to the EULA, do not install or use the app.
