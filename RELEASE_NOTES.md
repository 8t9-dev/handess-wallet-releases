# Handess Wallet 0.3.8 — Swap Auto-Complete and Progress Fix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes the final swap progress step, adds an opt-in auto-complete mode for accepted swaps, and adds a DGB reconnect button on the Swap page.

0.3.8 retains the 0.3.7 HNS payout recovery work, including the transaction-format fix that made HNS claims valid.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.8.msi` (97.8 MB)
- **MSI SHA-256:** `E3B2E0DD7AF8281AF6CD96C23E5423007749024FC2B476E688EB1F66400072DF`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.8-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `143EFB0D109942C1B036601FFAD65E715E0CBCF272D784E60DA17F1B75B38255`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.8-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `5A3D8F85E742A8BA0079AB1643A63A0B2840D61118901DE53AF9A2A2E57F867D`
- **Fedora/RHEL:** `Handess-Wallet-0.3.8-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `678490CA6258E16115BE2D1851DC0F19A95F9ED355D7E8B5A3A3418E75E76528`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.8-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.8-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.8-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.8-x86_64.AppImage
./Handess-Wallet-0.3.8-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.8-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### Completed progress is fixed

- The final **Completed** row now shows a green check once both HTLCs are spent.
- Previously, the completed state pointed at the final progress row as the current step, so it kept showing an in-progress spinner even though the swap badge said **Completed**.
- Older swaps with both claims recorded are reconciled to **Completed** during the monitor pass.

### Auto-complete swaps

A new **Auto-complete swaps** toggle on the Swap page automatically advances the local side of a swap after the user manually accepts an offer.

- **HNS side:** creates terms, locks the HNS HTLC, waits for 6 DGB confirmations, claims DGB, and reveals the secret.
- **DGB side:** waits for 2 HNS confirmations, funds the DGB HTLC, waits for the secret, and claims HNS.
- When both claims are recorded, the swap is marked **Completed**.
- The existing terms, amount, key, script, and confirmation checks still apply. Auto-complete only removes the button presses.
- The toggle is off by default and persists across restarts.
- The global toggle applies to offers accepted while it is on. Existing accepted swaps show **Enable auto-complete for this swap** so old trades are not started unexpectedly.
- A per-swap action lock prevents duplicate funding and claim transactions.

### Locked-wallet pause and resume

- HNS funding needs an unlocked wallet when the wallet is encrypted.
- If the wallet is locked, auto-complete pauses at the HNS funding step and shows the passphrase prompt on the swap.
- Entering the passphrase locks the HNS HTLC once; the rest of the process then continues automatically.
- The passphrase is not stored.
- DGB funding and both claims do not require the HNS passphrase.

### DGB reconnect button

- A refresh icon on the Swap page reconnects to the saved DGB server.
- It refreshes the DGB address, balance, active offers, and swap list.
- This replaces the old Disconnect → Connect workflow for a stale DGB connection.

### HNS payout destination warning

- On the DGB side, auto-complete claims HNS to the **currently selected HNS wallet**.
- The Swap page shows the selected wallet name in the auto-complete warning so the intended wallet can be selected before enabling the toggle.

### Refunds remain manual

- Auto-complete never performs a refund.
- Refund actions remain manual and time-locked.
- Offers are never auto-accepted.

## Retained from 0.3.7

- **HNS transaction format fixed.** HNS claim, refund, and escrow transactions now use transaction version 0, and HNS outpoints serialize the txid in display order. The previous build used version 2 and reversed the outpoint txid, so claims were rejected as nonstandard or treated as orphans.
- **Claim details saved.** The DGB side records the HNS claim transaction, payout address, amount, broadcast time, and raw signed transaction.
- **On-chain claim verification.** Completed swaps are re-checked against the HNS node. A confirmed claim is never marked missing or offered for rebroadcast.
- **Rebroadcast recovery.** If a claim disappears before mining, the swap offers **Rebroadcast HNS Claim**.
- **HNS received-at address.** The swap shows the exact payout address so a payout sent to another local HNS wallet can be identified.

## Retained from 0.3.6

- DGB HTLC funding, claim, and refund transactions sign the raw sighash. The previous build signed `SHA256(sighash)` because Noble v2 prehashes by default, and DigiByte nodes rejected the invalid signature.

## Retained from 0.3.5

- Swap events use the shared offer ID, so the DGB side applies terms and HNS funding and can progress past **Accepted**.

## Retained from 0.3.4

- HNS HTLC and escrow address derivation uses the bundled pure-JS SHA3-256 implementation, fixing `Digest method not supported` on the packaged Electron runtime.

## Retained from 0.3.3 and earlier

- DGB Electrum header notifications no longer crash the main process.
- Marketplace transfer detection no longer reports a pending transfer as cancelled.
- Pay seller is enabled only after the name transfer is detected on-chain.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, Finalize All, per-name History rows, received-name fee handling, clickable transaction hashes, exact-amount HNS sends, and the dark-mode transfer-address fix.

## Upgrading from 0.3.7

There is **no auto-updater**. Download and install 0.3.8 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

For a fully hands-off swap, both PCs must run 0.3.8 and enable **Auto-complete swaps**. Select the intended HNS wallet on the DGB side before enabling it. If the HNS wallet is locked when funding is due, enter the passphrase on the swap and the rest continues automatically.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.8.msi -Algorithm SHA256
Get-FileHash .\Handess-HSD-Runtime-8.0.0-win-x64.zip -Algorithm SHA256
```

### Linux

```bash
sha256sum -c SHA256SUMS.txt
```

`SHA256SUMS.txt` is attached to the release and stored in the repository.

## Before installing

1. Windows 10/11 x64 or Linux x64.
2. Back up your seed phrase.
3. Close Bob Wallet or any other wallet using ports `12037`/`12039`.
4. Review the EULA shown on first launch.

New users can use the setup wizard to install the optional private Node.js + HSD runtime, or reuse an existing Node.js/HSD installation. A fresh HSD mainnet data folder can take hours or days to sync and a significant amount of disk space.

The Windows installer is **unsigned**. If SmartScreen appears, choose **More info → Run anyway**. Linux packages are not GPG-signed; verify the SHA-256 hashes.

## Beta caveats

- Unsigned and not independently audited.
- Windows x64 and Linux x64 only in this release. macOS is not published yet.
- Linux RPM testing is limited; the AppImage is the portable fallback.
- Ubuntu 24.04 AppImage may need `libfuse2t64`.
- No auto-updater; install new versions manually.
- The app package does not bundle Node.js or hsd; the optional runtime is downloaded separately or selected as a local zip.
- A funded HNS ↔ DGB swap has completed end-to-end, but payout delivery can still require the 0.3.7 recovery workflow. Use small amounts where possible. Escrow has not completed a funded end-to-end trade on both chains yet.
- Auto-complete is opt-in and local. Both PCs need 0.3.8 and the toggle enabled for a hands-off swap. Refunds remain manual, and offers are not auto-accepted.
- On the DGB side, HNS is claimed to the currently selected HNS wallet. Select the intended wallet before enabling auto-complete.
- Escrow arbiter trust is real; lost keys can deadlock funds. The arbiter does not receive automatic notifications; dispute PSBT delivery is manual.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only.
- Electron 28 is end-of-life; a dependency refresh is in progress.
- No hardware-wallet support.

## License

This build is distributed under the **Handess Wallet End User License Agreement (EULA)**, a proprietary license. It is free to download and use, but modification, editing, forking, derivative works, and redistribution are not permitted. The source is not published, and no source-code license is granted.

The EULA is included in the app package as `LICENSE`, is presented on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`. The optional runtimes contain Node.js and hsd under their own licenses; their license files are included in the runtime zips.

## Feedback

Open an issue: <https://github.com/8t9-dev/handess-wallet-releases/issues>

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
