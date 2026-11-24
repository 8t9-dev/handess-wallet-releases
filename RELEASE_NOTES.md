# Handess Wallet 0.3.11 — Marketplace Transfer Fix

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes marketplace purchases that could not be paid while the seller's name transfer was in progress.

Source code: <https://github.com/8t9-dev/handess-wallet> (tag `v0.3.11`).

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.11.msi` (97.8 MB)
- **MSI SHA-256:** `2217E01B557C36500D5F9E675256484EC0B92FB3348FCB2D4E0EB75EC2640186`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.11-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `89A54B5E32E49D1F81A76548639B781CEDCF2BB7C4269080DFA88FF762CD0041`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.11-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `57A4656D8B894B8EEF1BC582E5C495478430406E884258A600D13FAFFED3283C`
- **Fedora/RHEL:** `Handess-Wallet-0.3.11-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `56C275D2F4DA928FB13B4AE5372348FDB136073CEEA27A83C9E559DB28F1165D`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.11-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.11-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.11-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.11-x86_64.AppImage
./Handess-Wallet-0.3.11-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.11-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### Marketplace transfer detection fixed

Handshake does not have a `TRANSFER` name state. A transferring name remains `CLOSED` with a non-zero `transfer` block height, and hsd pays the transfer coin back to the seller while encoding the buyer in the `TRANSFER` covenant. 0.3.10 and earlier looked for a `TRANSFER` state and checked the output address instead of the covenant.

That caused the buyer to see **Waiting for transfer** while the name was actually transferring. Clicking **Pay seller** returned **The name transfer has not been detected on-chain yet**, and the buy page could report the seller cancelled.

0.3.11 now:

- Treats `transfer > 0` as an active transfer and computes the remaining 288-block lockup.
- Reads the transfer transaction from the name's owner outpoint.
- Decodes the `TRANSFER` covenant to verify the intended recipient is the buyer.
- Scopes matching by the name hash so an unrelated transfer to the same address cannot be attributed to another name.
- Detects finalization through the owner coin with hsd's `coin/:hash/:index` endpoint. The previous `getcoin` JSON-RPC call does not exist on hsd's HTTP API, so finalization was never detected.
- Polls the chain for pending, transferred, and paid purchases instead of relying only on the seller's Nostr status event.
- Enables **Pay seller** as soon as the on-chain transfer is verified, even if a relay update was missed.
- Shows clearer purchase states for on-chain transfer, cancellation, and finalization.

### Retained from 0.3.10

- DGB Electrum connections negotiate `server.version` before any other request, fixing ElectrumX 2.0 `use server.version to identify client` failures.
- DGB balance refresh, offers, and swaps connect normally.

### Retained from 0.3.9 and earlier

- GPL-3.0-or-later license, **8t9.dev** copyright, and published source.
- No EULA or first-run acceptance gate.
- Completed-step progress fix, opt-in auto-complete swaps, per-swap opt-in, locked-wallet pause/resume, and the DGB reconnect button.
- HNS claim, refund, and escrow transactions use transaction version 0 and HNS outpoint byte order.
- HNS claims are re-checked on-chain and can be rebroadcast before mining.
- DGB HTLC funding, claim, and refund transactions sign the raw sighash.
- Swap events use the shared offer ID.
- HNS HTLC and escrow addresses use the bundled pure-JS SHA3-256 implementation.
- DGB Electrum header notifications no longer crash the main process.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.

## Upgrading from 0.3.10

There is **no auto-updater**. Download and install 0.3.11 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

Existing purchases remain in **My Purchases**. Open that tab and let the status refresh; an in-progress transfer should change to **Transfer detected** and enable **Pay seller**.

For a fully hands-off swap, both PCs must run 0.3.11 and enable **Auto-complete swaps**. Select the intended HNS wallet on the DGB side before enabling it.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.11.msi -Algorithm SHA256
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
- Auto-complete is opt-in and local. Both PCs need 0.3.11 and the toggle enabled for a hands-off swap. Refunds remain manual, and offers are not auto-accepted.
- On the DGB side, HNS is claimed to the currently selected HNS wallet. Select the intended wallet before enabling auto-complete.
- Escrow arbiter trust is real; lost keys can deadlock funds. The arbiter does not receive automatic notifications; dispute PSBT delivery is manual.
- Marketplace and swaps depend on Nostr relays, but purchase payment now uses the Handshake chain as the source of truth for transfer state.
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
