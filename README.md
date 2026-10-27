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
| **0.2.2 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.2.2.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.2.2/Handess-Wallet-0.2.2.msi)** | `87B92EEBFE01EF5A7372F03DB496BE800AB6B3A280C1447E0734C792279F27BC` |

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.2.1 and 0.2.0 are superseded.** 0.2.1 can show an empty **History** page even when the wallet has transactions. 0.2.0 can show a false “An hsd node is already running locally” warning. Use **0.2.2**.

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

### Upgrading from 0.2.1 or 0.2.0

There is **no auto-updater**. Download and run the 0.2.2 MSI; it upgrades the installed app in place. Wallet data lives outside the install directory and is preserved. Back up your seed phrase first.

After upgrading, open **History** and press **Refresh** if the page was already open. Transactions that were sent or received in 0.2.1 are still in the wallet database and will appear.

If you must keep using 0.2.0, its false node warning can be avoided by starting `hsd` manually before launching the app:

```powershell
hsd --prefix "$env:USERPROFILE\.hsd" --network main --index-tx --index-address
```

Use your own data directory if it is not `%USERPROFILE%\.hsd`. Installing 0.2.2 removes the need for this workaround.

---

## Verify the download

PowerShell:

```powershell
Get-FileHash .\Handess-Wallet-0.2.2.msi -Algorithm SHA256
```

Expected:

```text
87B92EEBFE01EF5A7372F03DB496BE800AB6B3A280C1447E0734C792279F27BC
```

The same checksum is attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.2.2

- **History page fixed** — the page now reads the selected wallet's transactions through hsd's wallet-scoped history API. This avoids an hsd JSON-RPC bug that returned “Assertion failed” and made wallets with transactions look empty. The page reloads after login and when you switch wallets, and sent transactions appear with their confirmation count.
- **Deep Clean fixed** — the maintenance action now calls hsd's real `POST /deepclean` endpoint with the required backup acknowledgement, instead of a nonexistent `deepclean` RPC method. **Deep Clean clears transaction history and balance state for every wallet managed by the node and requires a full rescan. Do not use it to fix a missing History page; install 0.2.2 instead.**
- **Sends use the selected wallet** — HNS sends target the wallet you logged into, rather than hsd's global wallet pointer.
- **Node-detection hotfix from 0.2.1** — the app checks that the local node and wallet ports are actually listening before showing “An hsd node is already running locally” or trying to attach, and rechecks every 5 seconds while disconnected.
- **Clearer connection errors from 0.2.1** — missing node or wallet-server ports now produce specific messages instead of a generic API-key/wallet-name error.
- **Everything from 0.2.0**, including the HNS wallet, domain CRM, auctions, marketplace, escrow, HNS ↔ DGB swaps, DGB swap wallet, explorer, and dark mode.

---

## Beta limitations

- **Unsigned and unaudited.** SmartScreen will warn you.
- **Windows x64 only** for this release. macOS and Linux targets are configured but not published.
- **No bundled Node.js or `hsd`.** A clean machine needs Node.js + `hsd`, or an external node.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **Deep Clean is destructive.** It clears history and balance state for all wallets on the node and requires a full blockchain rescan. Back up first.
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
