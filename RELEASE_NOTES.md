# Handess Wallet 0.2.2 — Windows Beta Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes an empty **History** page and a broken **Deep Clean** action.

## Download

- **Installer:** `Handess-Wallet-0.2.2.msi` (Windows 10/11 x64, 95.6 MB)
- **SHA-256:** `87B92EEBFE01EF5A7372F03DB496BE800AB6B3A280C1447E0734C792279F27BC`

## What changed

- **History now shows your transactions.** The page reads the selected wallet's history through hsd's wallet-scoped API. The old path used a JSON-RPC method that can fail with `Assertion failed` on wallets with unresolved or pruned history records, which made the page look empty even when the wallet had transactions. The page also reloads after login and when you switch wallets.
- **Deep Clean works correctly.** It now calls hsd's real `POST /deepclean` endpoint with the required backup acknowledgement, instead of a nonexistent `deepclean` RPC method.
- **Sends use the wallet you selected.** HNS transfers no longer depend on hsd's global wallet pointer.
- **Node detection from 0.2.1 is included.** The app checks that the local node and wallet ports are listening before showing “An hsd node is already running locally,” and rechecks every 5 seconds while disconnected.

## Important: do not use Deep Clean to fix History

**Deep Clean clears transaction history and balance state for every wallet managed by the node and requires a full blockchain rescan.** It is not needed for the 0.2.1 History bug. If History is empty on 0.2.1, install 0.2.2 and press **Refresh** on the History page instead.

If you already ran Deep Clean, start a **Rescan Blockchain** from Settings and let it finish before relying on balances or history.

## Upgrading from 0.2.1 or 0.2.0

There is **no auto-updater**. Download the 0.2.2 MSI and run it. The installer upgrades the app in place; wallet data is stored outside the install directory and is preserved. Back up your seed phrase first.

Transactions sent or received on 0.2.1 remain in the wallet database. Open **History** after upgrading and press **Refresh** if the page was already open.

0.2.0's false “node already running” warning can still be avoided by starting `hsd` manually before launching it:

```powershell
hsd --prefix "$env:USERPROFILE\.hsd" --network main --index-tx --index-address
```

Use your own data directory if it is not `%USERPROFILE%\.hsd`. Installing 0.2.2 removes the need for this workaround.

## Before installing

1. Windows 10/11 x64.
2. Install [Node.js](https://nodejs.org/) 20 LTS or newer and run `npm install -g hsd`, **or** connect Handess to an existing hsd node in Settings.
3. Close Bob Wallet or any other wallet using ports 12037/12039.
4. Back up your seed phrase.
5. Review the EULA shown on first launch.

The installer is **unsigned**. If SmartScreen appears, choose **More info → Run anyway**.

## License

This build is distributed under the **Handess Wallet End User License Agreement (EULA)**, a proprietary license. It is free to download and use, but modification, editing, forking, derivative works, and redistribution are not permitted. The source is not published, and no source-code license is granted.

The EULA is included in the installer as `LICENSE`, is presented on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`.

## Beta caveats

- Unsigned and not independently audited.
- Windows x64 only in this release.
- No bundled Node.js/hsd.
- No auto-updater; install new versions manually.
- Deep Clean is destructive: it clears history and balance state for all wallets on the node and requires a full rescan.
- No funded end-to-end swap or escrow trade has been completed on both chains yet; test with small amounts.
- Escrow arbiter trust is real; lost keys can deadlock funds.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only; there is no general DGB send screen.
- Electron 28 is end-of-life; dependency refresh is in progress.

## Feedback

Open an issue: https://github.com/8t9-dev/handess-wallet-releases/issues

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
