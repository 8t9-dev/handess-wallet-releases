# Handess Wallet 0.3.10 — DGB Connection Fix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes DigiByte (DGB) Electrum connections that failed with `use server.version to identify client` after DGB servers upgraded to ElectrumX 2.0.

Source code: <https://github.com/8t9-dev/handess-wallet> (tag `v0.3.10`).

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.10.msi` (97.8 MB)
- **MSI SHA-256:** `7968F53D8BD164D1B5DF23CF20C0940095C2F226F05D5066F7C17B98A4E77C83`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.10-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `97573367606FBC22376F5462BC20BC5CBC9413B51E2395F657D906236A8B9555`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.10-amd64.deb` (82.5 MB)
- **Deb SHA-256:** `763A91469FCA979530B6611C2A15602098C84FCC25ADC2B47B0D85DBD0E719A4`
- **Fedora/RHEL:** `Handess-Wallet-0.3.10-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `8D74979ECC291CA8449C742772CE8254C2AB5F16E8949B76EB975679939E89AB`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.10-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.10-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.10-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.10-x86_64.AppImage
./Handess-Wallet-0.3.10-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.10-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### DGB Electrum connection fixed

- ElectrumX 2.0 servers now require `server.version` to be the first request on a connection.
- The bundled Electrum client requested `server.banner` before identifying the client, so 0.3.9 could fail with **`use server.version to identify client`**.
- 0.3.10 suppresses the banner request during connect, negotiates protocol version 1.4 first, then verifies the DGB genesis hash before use.
- Tested against `electrum1.cipig.net:20059` (ElectrumX 2.0.0, DGB mainnet).
- DGB balance refresh, offers, and swaps can connect normally again.

No wallet data, swap history, or HNS handling changed in this release.

### Retained from 0.3.9

- Handess Wallet is licensed under **GPL-3.0-or-later**, with **8t9.dev** as the copyright holder.
- The proprietary EULA and first-run acceptance gate remain removed. The license is shown in **Settings → About & Legal**.
- The corresponding source is published at <https://github.com/8t9-dev/handess-wallet>, tagged `v0.3.10`.
- Third-party notices remain bundled and published.

### Retained from 0.3.8

- **Completed progress fixed.** The final **Completed** step shows a green check once both HTLCs are spent.
- **Auto-complete swaps.** An opt-in toggle automatically locks and claims the local side of an accepted swap. Manual accept is still required.
- **Per-swap auto opt-in.** The global toggle applies to new accepts; existing accepted swaps can be enabled individually.
- **Locked-wallet pause/resume.** HNS funding pauses when the wallet is locked and resumes after the passphrase is entered. The passphrase is not stored.
- **DGB reconnect button.** The refresh icon reconnects to the saved DGB server and refreshes address, balance, offers, and swaps.
- **HNS payout warning.** DGB-side swaps claim HNS to the currently selected HNS wallet.
- Refunds remain manual, and offers are never auto-accepted.

### Retained from 0.3.7 and earlier

- HNS claim, refund, and escrow transactions use transaction version 0 and HNS outpoint byte order.
- Claim details are saved, re-checked on-chain, and can be rebroadcast before mining if needed.
- DGB HTLC funding, claim, and refund transactions sign the raw sighash.
- Swap events use the shared offer ID, so the DGB side can apply terms and progress past **Accepted**.
- HNS HTLC and escrow address derivation uses the bundled pure-JS SHA3-256 implementation.
- DGB Electrum header notifications no longer crash the main process.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.

## Upgrading from 0.3.9

There is **no auto-updater**. Download and install 0.3.10 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

The only functional change is the DGB Electrum handshake. Existing DGB server settings are reused; if a connection still fails, open **Settings** and use the default DGB server, or click the DGB refresh button on the Swap page.

For a fully hands-off swap, both PCs must run 0.3.10 and enable **Auto-complete swaps**. Select the intended HNS wallet on the DGB side before enabling it.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.10.msi -Algorithm SHA256
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
4. Review the GPL license in **Settings → About & Legal**.

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
- Auto-complete is opt-in and local. Both PCs need 0.3.10 and the toggle enabled for a hands-off swap. Refunds remain manual, and offers are not auto-accepted.
- On the DGB side, HNS is claimed to the currently selected HNS wallet. Select the intended wallet before enabling auto-complete.
- Escrow arbiter trust is real; lost keys can deadlock funds. The arbiter does not receive automatic notifications; dispute PSBT delivery is manual.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only.
- Electron 28 is end-of-life; a dependency refresh is in progress.
- No hardware-wallet support.

## License

This build is distributed under the **GNU General Public License, version 3 or (at your option) any later version** (`GPL-3.0-or-later`).

Copyright (C) 2026 **8t9.dev**.

You may use, modify, and redistribute the software under the GPL. The corresponding source code is published at <https://github.com/8t9-dev/handess-wallet>. The license text is included in the app package as `LICENSE` and can be reviewed in **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`. The optional runtimes contain Node.js and hsd under their own licenses; their license files are included in the runtime zips.

The **Handess** name and logo are branding of 8t9.dev and are not licensed under the GPL. Modified versions should use different branding.

## Feedback

Open an issue: <https://github.com/8t9-dev/handess-wallet/issues>

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
