# Handess Wallet 0.3.9 — GPLv3 License and Public Source

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release relicenses Handess Wallet under the **GNU General Public License, version 3 or (at your option) any later version** (`GPL-3.0-or-later`) and publishes the corresponding source code.

The proprietary EULA and first-run acceptance gate are removed. The license text is available in the app under **Settings → About & Legal**.

Source code: <https://github.com/8t9-dev/handess-wallet> (tag `v0.3.9`).

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.9.msi` (97.8 MB)
- **MSI SHA-256:** `DEA2B5705E33BA538FC32488797C99D575231DFEB01AAD120A7B48A527813F4D`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.9-x86_64.AppImage` (122.2 MB)
- **AppImage SHA-256:** `56C4BC66BFB79434C831D92FAD475FA269A9E4A09B5BBAADA3DD8BA28F7915CE`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.9-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `9A4649515439C08FD7170372B16437467DEC3CEB83D49235D3988A4EBBB079E8`
- **Fedora/RHEL:** `Handess-Wallet-0.3.9-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `22109D0F33357020E792FB3D9798256BFE989A10C949A8A8BCDA8B9DB669D6F1`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.9-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.9-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.9-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.9-x86_64.AppImage
./Handess-Wallet-0.3.9-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.9-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### GPLv3 license

- Handess Wallet is now free/open-source software under `GPL-3.0-or-later`.
- Copyright (C) 2026 **8t9.dev**.
- The proprietary EULA, first-run clickwrap gate, and “licensed, not sold” wording are removed.
- The bundled `LICENSE` file is the official GNU GPL v3 text.
- **Settings → About & Legal** shows the GPL license and third-party notices.
- The MSI/package metadata now identifies 8t9.dev as the author and GPL-3.0-or-later as the license.

### Public source code

- The corresponding source is published at <https://github.com/8t9-dev/handess-wallet>, tagged `v0.3.9`.
- The published source builds the released packages: `npm ci`, `npm run typecheck`, `npm run lint`, `npm test`, `npm run package-win`, `npm run package-linux`.
- Only necessary code and docs are published: source, tests, build scripts, configs, icon, bundled HNS search index seed, `README.md`, `LICENSE`, and `THIRD_PARTY_NOTICES.md`.
- Scratch notes, logs, local agent state, build output, and vendored runtime binaries are excluded.

### Third-party notices

- `THIRD_PARTY_NOTICES.md` is regenerated for 0.3.9 and updated to describe the GPL license.
- Third-party components remain under their own licenses. The optional runtime still contains Node.js and hsd under their own licenses.

### Branding

- The **Handess** name and logo are branding of 8t9.dev and are not licensed under the GPL.
- Modified versions should use different branding.

## Retained from 0.3.8

- **Completed progress fixed.** The final **Completed** step shows a green check once both HTLCs are spent.
- **Auto-complete swaps.** An opt-in toggle automatically locks and claims the local side of an accepted swap. Manual accept is still required.
- **Per-swap auto opt-in.** The global toggle applies to new accepts; existing accepted swaps can be enabled individually.
- **Locked-wallet pause/resume.** HNS funding pauses when the wallet is locked and resumes after the passphrase is entered. The passphrase is not stored.
- **DGB reconnect button.** The refresh icon reconnects to the saved DGB server and refreshes address, balance, offers, and swaps.
- **HNS payout warning.** DGB-side swaps claim HNS to the currently selected HNS wallet.
- Refunds remain manual, and offers are never auto-accepted.

## Retained from 0.3.7

- **HNS transaction format fixed.** HNS claim, refund, and escrow transactions use transaction version 0 and HNS outpoint byte order.
- **Claim details saved.** The DGB side records the HNS claim transaction, payout address, amount, broadcast time, and raw signed transaction.
- **On-chain claim verification.** Completed swaps are re-checked against the HNS node. A confirmed claim is never marked missing or offered for rebroadcast.
- **Rebroadcast recovery.** If a claim disappears before mining, the swap offers **Rebroadcast HNS Claim**.
- **HNS received-at address.** The swap shows the exact payout address.

## Retained from 0.3.6

- DGB HTLC funding, claim, and refund transactions sign the raw sighash.

## Retained from 0.3.5

- Swap events use the shared offer ID, so the DGB side applies terms and HNS funding and can progress past **Accepted**.

## Retained from 0.3.4

- HNS HTLC and escrow address derivation uses the bundled pure-JS SHA3-256 implementation.

## Retained from 0.3.3 and earlier

- DGB Electrum header notifications no longer crash the main process.
- Marketplace transfer detection no longer reports a pending transfer as cancelled.
- Pay seller is enabled only after the name transfer is detected on-chain.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, Finalize All, per-name History rows, received-name fee handling, clickable transaction hashes, exact-amount HNS sends, and the dark-mode transfer-address fix.

## Upgrading from 0.3.8

There is **no auto-updater**. Download and install 0.3.9 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

The EULA acceptance gate is gone. Existing users do not need to accept anything new; the GPL license is available in **Settings → About & Legal**.

For a fully hands-off swap, both PCs must run 0.3.9 and enable **Auto-complete swaps**. Select the intended HNS wallet on the DGB side before enabling it.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.9.msi -Algorithm SHA256
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
- Auto-complete is opt-in and local. Both PCs need 0.3.9 and the toggle enabled for a hands-off swap. Refunds remain manual, and offers are not auto-accepted.
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
