# Handess Wallet 0.3.5 — Swap Synchronization Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes swap event synchronization between the two PCs: the DGB side now applies the HNS side’s terms and funding events and can continue the swap.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.5.msi` (97.8 MB)
- **MSI SHA-256:** `A9A7F4AB3EAC30502CCB1A8A1F13B2C0D3497B3C4DAC931CA6765D5C38381120`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.5-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `BCFE55EBB5BD648C1015165C08D11D6C5BCA18AB4166745D7653865A39524B28`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.5-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `4E520CEFDBF576139E59E147EB03C251E3786EE92B5CFB8E666F2C20E09AD847`
- **Fedora/RHEL:** `Handess-Wallet-0.3.5-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `F54CA9E953672CD825FE03D629CA979DA363AB978064A269EB27C5190E153905`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.5-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.5-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.5-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.5-x86_64.AppImage
./Handess-Wallet-0.3.5-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.5-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### Swap events now use the shared offer ID

- Swap `TERMS`, `FUNDING`, `SECRET`, and `STATUS` events now use `swap.offer.id` instead of the local swap ID.
- The acceptor’s local swap ID is generated and differs from the offerer’s local ID, so the DGB side previously failed to match the HNS side’s terms and funding events.
- Incoming events are now resolved by either the local swap ID or the embedded offer ID.

This fixes the DGB side being stuck at **Accepted** while the HNS side shows **Terms Exchanged → HNS Locked**.

### Existing swaps

- Existing **HNS → DGB** swaps should recover after updating the DGB-side PC. The monitor will apply the already-published terms and HNS funding events.
- Older **DGB → HNS** swaps accepted before 0.3.5 may still be stuck if the HNS side published terms under its old local ID. Re-accept those swaps with both PCs on 0.3.5.

### Retained from 0.3.4

- HNS HTLC and escrow address derivation now uses the bundled pure-JS SHA3-256 implementation instead of Node/OpenSSL, fixing `Digest method not supported` on the packaged Electron runtime.

### Retained from 0.3.3

- DGB Electrum header notifications no longer crash the main process.
- Marketplace transfer detection no longer reports a pending transfer as cancelled.
- Pay seller is enabled only after the name transfer is detected on-chain.
- Payment to the seller is still allowed if the buyer finalized the name first.

### Retained from 0.3.2 and earlier

- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, Finalize All, per-name History rows for batched finalizes, received-name fee handling, and clickable transaction hashes.
- Exact-amount HNS sends with the fee added on top, the dark-mode transfer-address fix, and the first-run setup wizard.

## Upgrading from 0.3.4

There is **no auto-updater**. Download and install 0.3.5 on both PCs. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

For swaps already accepted before 0.3.5, update the **DGB-side** PC first. It will pick up the existing HNS terms and funding events. The HNS side should also be updated before starting new swaps.

If your node is already configured, the setup wizard is skipped and your existing HSD settings remain in use.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.5.msi -Algorithm SHA256
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
