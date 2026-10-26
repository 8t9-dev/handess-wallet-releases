# Handess Wallet 0.2.1 — Windows Beta Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes a false “hsd node is already running” warning that could block reconnecting after the app quit its managed node.

## Download

- **Installer:** `Handess-Wallet-0.2.1.msi` (Windows 10/11 x64, 95.7 MB)
- **SHA-256:** `24FBD7018C5C717B00648FD5668C6068F6A67810AAF80A9982225D4A6FF0CBFE`

## What changed

- **Accurate node detection.** Handess now probes the local node port (`12037`) and wallet port (`12039`) before showing “An hsd node is already running locally” or attempting to attach. A saved API key alone is no longer treated as proof that a node is running.
- **No stale banner.** While disconnected, the app rechecks both ports every 5 seconds. The banner appears when a node is started outside the app and clears when it stops.
- **Specific connection errors.** If the node port is closed, the app says no node is listening on that address. If the node is running but the wallet server is down, it says so instead of blaming the API key or wallet name.

Everything available in 0.2.0 is included.

## Upgrading from 0.2.0

0.2.0 has **no auto-updater**. Download the 0.2.1 MSI and run it. The installer upgrades the app in place; wallet data is stored outside the install directory and is preserved. Back up your seed phrase first.

If 0.2.0 shows the false “node already running” warning, start `hsd` manually before launching it:

```powershell
hsd --prefix "$env:USERPROFILE\.hsd" --network main --index-tx --index-address
```

Use your own data directory if it is not `%USERPROFILE%\.hsd`. Once `hsd` listens on `12037` and `12039`, launch 0.2.0 and connect — or install 0.2.1, which fixes the detection.

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
- No funded end-to-end swap or escrow trade has been completed on both chains yet; test with small amounts.
- Escrow arbiter trust is real; lost keys can deadlock funds.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only; there is no general DGB send screen.
- Electron 28 is end-of-life; dependency refresh is in progress.

## Feedback

Open an issue: https://github.com/8t9-dev/handess-wallet-releases/issues

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
