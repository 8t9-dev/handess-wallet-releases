# Handess Wallet 0.3.2 — Windows + Linux Beta

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release adds Linux packages and the Linux private HSD runtime, plus the DGB WIF QR, Finalize All, per-name transaction history for batched finalizes, correct fee handling for received names, and click-to-copy transaction hashes.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.2.msi` (97.8 MB)
- **MSI SHA-256:** `FE17A9A8D49BCA085BB52958D52B1769005D2AE8144F00CCE6C6D9E7EAE47CCC`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.2-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `7225F562BDFD444A5D52530AF1828D5C8D678450AC5C54B13F0BED3FA49E3DE6`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.2-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `B6A925AC79F66BFFE92940EE7CC9025FFD73944E07A9D39CC7A95C579F851F3D`
- **Fedora/RHEL:** `Handess-Wallet-0.3.2-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `7583475973116C3CE50F89E5BC7CB1AD33FAE133C82D6A1412DC51344232B507`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.2-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.2-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.2-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.2-x86_64.AppImage
./Handess-Wallet-0.3.2-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.2-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### Linux support

- Added AppImage, `.deb`, and `.rpm` packages for Linux x64.
- Added a Linux private HSD runtime containing Node.js 24.15.0 and hsd 8.0.0.
- The setup wizard now detects Linux system Node.js/HSD installations, installs the private runtime under the app's user-data folder, and restores the executable permission after extraction.
- The Windows runtime remains unchanged.

### DGB WIF QR

- The DGB WIF QR is gated behind the existing WIF reveal and wallet passphrase.
- A separate **Show QR** action displays the QR with a warning.
- The QR clears on hide, logout, and network change.

### Finalize All

- My Domains shows a green **Finalize All** button only when more than two transfers are ready to finalize.
- One passphrase entry unlocks the wallet and submits the batch through the batch FINALIZE path.
- Progress, completion, and failure states are shown on the page; the wallet is unlocked once, and unencrypted wallets are handled correctly.

### Transaction history fixes

- A batched FINALIZE now produces one History row per name instead of a single row for the whole batch.
- Expanded rows share the on-chain transaction hash and have unique row IDs.
- The batch transaction fee is shown once, not multiplied across the name rows.
- Receiving a name no longer charges or displays the sender's transaction fee. Incoming transfers show the received amount with fee zero.

### Copyable transaction hashes

- Click a recent transaction hash on the Dashboard or History page to copy the full hash.
- Copying uses the Electron clipboard bridge, with Web Clipboard and `execCommand` fallbacks.

### Retained from 0.3.1

- HNS sends deliver the exact entered amount; the network fee is added on top and paid by the sender.
- The dark-mode name-transfer recipient input is readable.

## Upgrading

There is **no auto-updater**. Download and install the 0.3.2 package for your platform. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the setup wizard is skipped and your existing HSD settings remain in use.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.2.msi -Algorithm SHA256
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
- No funded end-to-end swap or escrow trade has completed on both chains yet; test with small amounts.
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
