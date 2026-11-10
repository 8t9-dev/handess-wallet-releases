# Handess Wallet 0.3.4 — HNS Swap and Escrow Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes HNS HTLC and escrow address derivation in the packaged Electron runtime, which failed with “Digest method not supported” when starting an HNS → DGB swap.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.4.msi` (97.8 MB)
- **MSI SHA-256:** `9AC5BB986E6417969EF246540E5E360218ECB1C01E079009FFA2E79F6D5DC74C`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.4-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `5DE7C8CBEC19B97C61F4A03623B2C06377D8B6DB9E482C36ABB8143C80BADA47`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.4-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `0E7BE58837C78F2DC2170DB0A331E6D37D08A8BADDDA60993F85AFB60D0169A0`
- **Fedora/RHEL:** `Handess-Wallet-0.3.4-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `B12811F173B4F7B9DE2CB50B9A1BFC16FE95B69B97D104CB7E996CB4C496205A`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.4-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.4-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.4-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.4-x86_64.AppImage
./Handess-Wallet-0.3.4-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.4-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### HNS swap and escrow SHA3 fix

- HNS P2WSH HTLC and escrow addresses now use the bundled pure-JS SHA3-256 implementation instead of Node/OpenSSL.
- This fixes the packaged Electron error `Digest method not supported` when starting an HNS → DGB swap or funding an HNS escrow.
- The resulting addresses and scripts are unchanged; only the hash implementation used to derive them changed.
- The same fix applies to the HNS escrow paths that share these helpers.

### Retained from 0.3.3

- DGB Electrum header notifications no longer crash the main process.
- Marketplace transfer detection no longer reports a pending transfer as cancelled.
- Pay seller is enabled only after the name transfer is detected on-chain.
- Payment to the seller is still allowed if the buyer finalized the name first.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, Finalize All, per-name History rows for batched finalizes, received-name fee handling, and clickable transaction hashes.

### Retained from 0.3.2 and earlier

- Exact-amount HNS sends with the fee added on top.
- Dark-mode transfer-address fix.
- First-run setup wizard, Search Engine refresh scopes, Explore resource records, auction timing fixes, on-chain tags with CSV import/export, and the Global Auctions Overview.

## Upgrading from 0.3.3

There is **no auto-updater**. Download and install the 0.3.4 package for your platform. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the setup wizard is skipped and your existing HSD settings remain in use.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.4.msi -Algorithm SHA256
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
