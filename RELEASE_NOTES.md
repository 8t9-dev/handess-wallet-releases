# Handess Wallet 0.3.1 — Windows Beta Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This hotfix corrects HNS sending and the dark-mode name-transfer input.

## Download

- **Installer:** `Handess-Wallet-0.3.1.msi` (Windows 10/11 x64, 97.8 MB)
- **MSI SHA-256:** `47002FA467C72A7324FEDF3B2B8F9E7E57C3FE3BDA242E15C7EA5AE5F9CA80B8`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

The MSI remains wallet-only. The runtime is the same optional setup runtime used by 0.3.0.

## What changed

### HNS sends now deliver the exact entered amount

- The recipient receives exactly the amount entered in **Send HNS**.
- The network fee is added on top and paid by the sender's wallet.
- The confirmation screen and **MAX** calculation already assumed this behavior; the backend now matches them.
- Example: sending **50 HNS** gives the recipient **50 HNS**; the sender pays 50 HNS plus the network fee.

### Dark-mode transfer address fixed

- The recipient-address input in **My Domains → Transfer** now uses a dark background, light text, and a readable placeholder in dark mode.
- No more white text on a white input field.

All 0.3.0 features remain, including the first-run setup wizard, Search Engine refresh scopes, full Explore resource records, auction timing fixes, on-chain tags with CSV import/export, and the bidding-only Global Auctions Overview.

## Upgrading from 0.3.0

There is **no auto-updater**. Download and run the 0.3.1 MSI; it upgrades the installed app in place. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the setup wizard is skipped and your existing HSD settings remain in use.

## Verify the downloads

PowerShell:

```powershell
Get-FileHash .\Handess-Wallet-0.3.1.msi -Algorithm SHA256
Get-FileHash .\Handess-HSD-Runtime-8.0.0-win-x64.zip -Algorithm SHA256
```

Expected:

```text
MSI:     47002FA467C72A7324FEDF3B2B8F9E7E57C3FE3BDA242E15C7EA5AE5F9CA80B8
Runtime: 332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA
```

`SHA256SUMS.txt` is attached to the release and stored in this repository.

## Before installing

1. Windows 10/11 x64.
2. Back up your seed phrase.
3. Close Bob Wallet or any other wallet using ports 12037/12039.
4. Review the EULA shown on first launch.

New users can use the setup wizard to install the optional private Node.js + HSD runtime, or reuse an existing Node.js/HSD installation. A fresh HSD mainnet data folder can take hours or days to sync and a significant amount of disk space.

The installer is **unsigned**. If SmartScreen appears, choose **More info → Run anyway**.

## Beta caveats

- Unsigned and not independently audited.
- Windows x64 only in this release.
- No auto-updater; install new versions manually.
- The MSI does not bundle Node.js or hsd; the optional runtime is downloaded separately or selected as a local zip.
- No funded end-to-end swap or escrow trade has completed on both chains yet; test with small amounts.
- Escrow arbiter trust is real; lost keys can deadlock funds.
- The arbiter does not receive automatic notifications; dispute PSBT delivery is manual.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only.
- Electron 28 is end-of-life; a dependency refresh is in progress.
- No hardware-wallet support.

## License

This build is distributed under the **Handess Wallet End User License Agreement (EULA)**, a proprietary license. It is free to download and use, but modification, editing, forking, derivative works, and redistribution are not permitted. The source is not published, and no source-code license is granted.

The EULA is included in the installer as `LICENSE`, is presented on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`. The optional runtime contains Node.js and hsd under their own licenses; their license files are included in the runtime zip.

## Feedback

Open an issue: https://github.com/8t9-dev/handess-wallet-releases/issues

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
