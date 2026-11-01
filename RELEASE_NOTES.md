# Handess Wallet 0.3.0 — Windows Beta

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release adds a first-run setup wizard, configurable Search Engine refresh depth, full DNS resource records in Explore, and auction accuracy fixes.

## Download

- **Installer:** `Handess-Wallet-0.3.0.msi` (Windows 10/11 x64, 97.8 MB)
- **MSI SHA-256:** `D268D9D18A43B1E853173E92BFC47CAB312ECB9DA1FA085E14FEA7228AE1DB29`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

The MSI remains wallet-only. The runtime is a separate, optional download used by the setup wizard for new users; the MSI itself stays small.

## What changed

### First-run setup wizard

- **New to Handshake** — download a checksum-verified private Node.js + HSD runtime, choose a new HSD data folder, then create/import the wallet.
- **I already have Node.js** — use the detected Node.js with a private HSD runtime.
- **I already have HSD** — use an existing HSD installation and data folder. No new chain sync.
- The wizard detects existing HSD data folders, validates HSD binaries, and warns before creating a fresh mainnet data folder.
- The HSD data folder can be placed on any writable drive. The runtime is app-managed under `%LOCALAPPDATA%\Handess\runtime`.
- The wizard can be re-run from **Settings → Node & HSD setup**.
- Existing users with a configured node are not interrupted after upgrading; the wizard only appears when no setup is detected.

### Search Engine refresh controls

- Added a **Refresh settings** section with scan depths: **1,000**, **10,000**, **50,000**, and **105,120** blocks.
- 105,120 blocks is the two-year mainnet renewal window (144 blocks/day).
- Refresh now scans raw blocks locally and reads resource data directly from REGISTER/UPDATE covenants, instead of stopping after a fixed number of names.
- Renewals advance expiry without RPC; empty resource updates and revokes remove stale entries; new names are added only when they have tags or custom DNS.
- Long scans show progress and can be cancelled. The completed refresh height survives restarts.
- Removed the manual **Add tag** input from the Search Engine; existing tag chips and all other filters remain.

### Explore resource records

- Name details now show every resource record with its full fields:
  - Every **TXT** record and every chunk
  - Every **DS** record with key tag, algorithm, digest type, and digest
  - **NS**, **GLUE4/6**, and **SYNTH4/6** values
- Explorer timeline and event dates use actual historical block timestamps; future dates are marked estimated.

### Auctions

- Corrected phase schedules to match hsd:
  - Mainnet: OPENING 37, BIDDING 720, REVEAL 1,440
  - Testnet: OPENING 37, BIDDING 144, REVEAL 288
  - Regtest: OPENING 6, BIDDING 5, REVEAL 10
- **Global Auctions Overview** now includes only names currently in **BIDDING**.
- The left half shows bidding ending soon; the right half shows bid activity (most/fewest bids, highest/lowest bid), 50 names per page.
- Phase countdowns and dates match hsd.

### My Domains tags

- Tags are read from on-chain TXT records and shown comma-separated.
- CSV export/import supports bulk tag edits.
- Each tag is written as its own `tag: name` TXT record; non-tag TXT records are preserved.
- Unchanged names are skipped during import.

### Marketplace and escrow

- No marketplace behavior changes in this release.
- The seller still selects the arbiter; the arbiter key and payout address are embedded in the signed listing.
- The arbiter is **not notified automatically**. During a dispute, a buyer or seller must copy the dispute PSBT and send it to the arbiter out-of-band. The arbiter signs it with the offline `arbiter:sign` tool and returns it.

## Upgrading from 0.2.2

There is **no auto-updater**. Download and run the 0.3.0 MSI; it upgrades the installed app in place. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the setup wizard is skipped and your existing HSD path, binary, and data folder remain in use. To change them, open **Settings → Node & HSD setup → Run setup wizard**.

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

`SHA256SUMS.txt` is attached to the release and stored in this repository.

## Before installing

1. Windows 10/11 x64.
2. Back up your seed phrase.
3. Close Bob Wallet or any other wallet using ports 12037/12039.
4. Review the EULA shown on first launch.

New users do **not** need to install Node.js or hsd manually. The setup wizard can install the optional private runtime. Existing Node.js and HSD installations are detected and can be reused.

A fresh HSD mainnet data folder can take hours or days to sync and a significant amount of disk space. Existing HSD data avoids that.

The installer is **unsigned**. If SmartScreen appears, choose **More info → Run anyway**.

## Beta caveats

- Unsigned and not independently audited.
- Windows x64 only in this release.
- No auto-updater; install new versions manually.
- The MSI does not bundle Node.js or hsd. The optional runtime is downloaded separately and requires internet access, or can be selected as a local zip.
- A fresh HSD chain can be large and slow to sync.
- No funded end-to-end swap or escrow trade has completed on both chains yet; test with small amounts.
- Escrow arbiter trust is real; lost keys can deadlock funds.
- The arbiter does not receive automatic notifications; dispute PSBT delivery is manual.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only; there is no general DGB send screen.
- Electron 28 is end-of-life; a dependency refresh is in progress.
- No hardware-wallet support.

## License

This build is distributed under the **Handess Wallet End User License Agreement (EULA)**, a proprietary license. It is free to download and use, but modification, editing, forking, derivative works, and redistribution are not permitted. The source is not published, and no source-code license is granted.

The EULA is included in the installer as `LICENSE`, is presented on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`. The optional runtime contains Node.js and hsd under their own licenses; their license files are included in the runtime zip.

## Feedback

Open an issue: https://github.com/8t9-dev/handess-wallet-releases/issues

Please include the app version, Windows version, and the exact error text.

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
