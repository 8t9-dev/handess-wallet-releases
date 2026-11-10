# Handess Wallet 0.3.3 — Swap and Marketplace Hotfix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes a DGB swap main-process crash and a false “seller cancelled” marketplace error while a name transfer is still confirming.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.3.msi` (97.8 MB)
- **MSI SHA-256:** `5256626703BC692777598A1E62CD0325FA82B5333A3E01B5033DAA46FF5CA8E1`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.3-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `32B1961951CD2ECB8C28E3AA6066FBC72B5015C8108EA6C9A48E959DA563087D`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.3-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `468809B1096F7FDE886C33F2EFC2A27D23C5B906C16EAD9EAF64936B862432BB`
- **Fedora/RHEL:** `Handess-Wallet-0.3.3-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `DDF14B83B73982AB4E8E7E9CCEEF19650DCA3FB8A58AB6964C5904BA20E342EF`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.3-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.3-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.3-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.3-x86_64.AppImage
./Handess-Wallet-0.3.3-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.3-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### DGB swap crash fixed

- DGB Electrum header notifications no longer trigger a main-process JavaScript error.
- The Electrum client now routes server notifications through its initialized event emitter instead of the undefined `subscribe` property.
- Malformed or unexpected server notifications are caught and logged instead of terminating the main process.

This fixes the “A JavaScript error occurred in the main process — Cannot read properties of undefined (reading 'emit')” dialog seen on the Swap page.

### Marketplace transfer detection fixed

- A normal `CLOSED` name is no longer treated as a cancelled transfer while the seller's `TRANSFER` is still waiting to be mined or indexed.
- A cancellation is reported only when a transfer to the buyer was actually confirmed and the name is no longer in `TRANSFER` state.
- If the transfer transaction cannot be fetched, the buyer's wallet history is checked before giving up.
- **Pay seller** is disabled until the name transfer is detected on-chain. My Purchases shows “Waiting for on-chain transfer…” instead of claiming the seller cancelled.
- If the buyer already finalized the name before paying, payment to the seller is still allowed.

### Retained from 0.3.2

- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, gated behind the wallet passphrase.
- Finalize All for more than two ready transfers.
- One History row per name for batched FINALIZE transactions, with the transaction fee shown once.
- Received name transfers no longer charge or display the sender's fee.
- Click-to-copy transaction hashes on the Dashboard and History page.
- Exact-amount HNS sends with the fee added on top, and the dark-mode transfer-address fix.

## Upgrading from 0.3.2

There is **no auto-updater**. Download and install the 0.3.3 package for your platform. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the setup wizard is skipped and your existing HSD settings remain in use.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.3.msi -Algorithm SHA256
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
