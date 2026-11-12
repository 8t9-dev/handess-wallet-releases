# Handess Wallet - Public Beta

**Handess Wallet** is a self-custody desktop wallet for **Handshake (HNS)** with a domain CRM, a peer-to-peer name marketplace, optional 2-of-3 escrow, and atomic swaps between **HNS and DigiByte (DGB)**.

> **Beta software. Unsigned build. Use small amounts.**
>
> These packages are not code-signed and have not been independently audited. Windows SmartScreen will warn you. Linux packages are not GPG-signed. Do not use them with funds you cannot afford to lose.

This repository contains release downloads only. The source is not published.

---

## Download

| Version | Platform | File | SHA-256 |
| --- | --- | --- | --- |
| **0.3.8 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.3.8.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-Wallet-0.3.8.msi)** (97.8 MB) | `E3B2E0DD7AF8281AF6CD96C23E5423007749024FC2B476E688EB1F66400072DF` |
| **0.3.8 beta** | Linux x64 (portable) | **[Handess-Wallet-0.3.8-x86_64.AppImage](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-Wallet-0.3.8-x86_64.AppImage)** (122.2 MB) | `143EFB0D109942C1B036601FFAD65E715E0CBCF272D784E60DA17F1B75B38255` |
| **0.3.8 beta** | Ubuntu / Debian x64 | **[Handess-Wallet-0.3.8-amd64.deb](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-Wallet-0.3.8-amd64.deb)** (82.6 MB) | `5A3D8F85E742A8BA0079AB1643A63A0B2840D61118901DE53AF9A2A2E57F867D` |
| **0.3.8 beta** | Fedora / RHEL x64 | **[Handess-Wallet-0.3.8-x86_64.rpm](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-Wallet-0.3.8-x86_64.rpm)** (81.0 MB) | `678490CA6258E16115BE2D1851DC0F19A95F9ED355D7E8B5A3A3418E75E76528` |
| **Optional runtime** | Windows x64 | **[Handess-HSD-Runtime-8.0.0-win-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-HSD-Runtime-8.0.0-win-x64.zip)** (79.5 MB) | `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA` |
| **Optional runtime** | Linux x64 | **[Handess-HSD-Runtime-8.0.0-linux-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.8/Handess-HSD-Runtime-8.0.0-linux-x64.zip)** (53.3 MB) | `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE` |

The app packages contain the wallet only. The optional runtime contains Node.js 24.15.0 and hsd 8.0.0 and is used by the setup wizard for **New to Handshake** users. Existing Node.js or HSD installations are detected and can be reused.

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.3.7 and earlier are superseded.** 0.3.8 fixes the swap progress display, adds an opt-in auto-complete mode for accepted swaps, and adds a DGB reconnect button on the Swap page. 0.3.7 added HNS claim diagnostics and recovery, including the HNS transaction format fix that made claims valid. 0.3.6 fixed DGB transaction signing, which was double-hashing the sighash and causing DigiByte nodes to reject DGB HTLC funding, claim, and refund transactions.

---

## Before you install

1. **Windows 10/11 x64, or Linux x64.**
2. **Back up your seed phrase.** Handess never sends it anywhere and cannot recover it for you.
3. **Close Bob Wallet or any other wallet** using ports `12037`/`12039` before starting Handess's node.
4. **Review the license.** Handess Wallet is proprietary beta software distributed under the Handess Wallet EULA. The app shows the EULA on first launch and requires acceptance before use.

New users can use the setup wizard to install a private Node.js + HSD runtime. Existing Handshake users can point the wizard at their existing HSD data directory and avoid a new chain sync.

### Linux requirements

- x64 only.
- Built on Ubuntu 22.04; intended baseline is Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases.
- Ubuntu 24.04 AppImage users may need `libfuse2t64`. The `.deb` is recommended on Ubuntu/Debian.
- The `.rpm` is built for Fedora/RHEL-family systems but has not been installed on every distribution release. Use the AppImage as a fallback.

---

## Install

### Windows

1. Download the MSI and verify its SHA-256 (below).
2. Run it. If Windows shows **"Windows protected your PC"**:
   - Click **More info**.
   - Click **Run anyway**.
3. Launch **Handess Wallet**.
4. Review and accept the **End User License Agreement**.
5. On first launch, choose a setup path:
   - **New to Handshake** — download the private Node.js + HSD runtime and choose a new HSD data folder.
   - **I already have Node.js** — use your Node.js with a private HSD runtime.
   - **I already have HSD** — use your existing HSD data folder (recommended for existing Handshake users).
6. Create a wallet or import an existing HNS seed phrase, set a passphrase, and log in.
7. Wait for the node to finish syncing before sending funds.

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

### AppImage (portable)

```bash
chmod +x Handess-Wallet-0.3.8-x86_64.AppImage
./Handess-Wallet-0.3.8-x86_64.AppImage
```

If FUSE is unavailable, run:

```bash
./Handess-Wallet-0.3.8-x86_64.AppImage --appimage-extract-and-run
```

The setup wizard can be re-run later from **Settings → Node & HSD setup**.

### Upgrading from 0.3.7

There is **no auto-updater**. Download and install 0.3.8 on both PCs. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

0.3.8 adds the **Auto-complete swaps** toggle on the Swap page. It applies to offers accepted while the toggle is on; existing accepted swaps can be enabled individually from **My Swaps**. Both PCs need 0.3.8 and the toggle enabled for a fully hands-off swap. HNS funding pauses if the HNS wallet is locked; enter the passphrase on the swap to continue.

The DGB connection refresh icon on the Swap page reconnects to the saved DGB server without requiring Disconnect/Connect.

---

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

The same checksums are attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.3.8

- **Completed progress fixed** — the final **Completed** step now shows a green check instead of an endless spinner after both HTLCs are spent.
- **Auto-complete swaps** — an opt-in toggle on the Swap page automatically locks and claims the local side of an accepted swap. Manual accept is still required. HNS funding pauses when the wallet is locked and resumes after the passphrase is entered; refunds stay manual.
- **Per-swap auto opt-in** — the global toggle applies to new accepts. Existing accepted swaps show **Enable auto-complete for this swap**.
- **DGB reconnect button** — the refresh icon on the Swap page reconnects to the saved DGB server and refreshes the address, balance, offers, and swaps.
- **HNS payout warning** — the UI states that DGB-side swaps claim HNS to the currently selected HNS wallet, so the correct wallet can be selected before enabling auto-complete.
- **Everything from 0.3.7**, including the HNS transaction format fix, claim diagnostics, on-chain verification, and rebroadcast recovery.
- **Everything from 0.3.6**, including the DGB signing fix that signs the raw sighash instead of a double hash.
- **Everything from 0.3.5**, including the shared-offer-ID swap event fix that lets the DGB side apply terms and HNS funding.
- **Everything from 0.3.4**, including the pure-JS SHA3 fix for HNS HTLC/escrow address derivation.
- **Everything from 0.3.3 and earlier**, including the DGB Electrum notification crash fix, marketplace transfer detection fixes, Linux packages, the DGB WIF QR, Finalize All, per-name transaction-history rows, received-name fee handling, and copyable transaction hashes.

> **Before rebroadcasting:** select the HNS wallet that should receive the swap first. If no earlier claim address was saved, the rebroadcast pays the currently selected wallet.

---

## Beta limitations

- **Unsigned and unaudited.** Windows SmartScreen will warn you. Linux packages are not GPG-signed.
- **Windows x64 and Linux x64 only** in this release. macOS is not published yet.
- **Linux RPM testing is limited.** The `.rpm` is built but has not been installed on every Fedora/RHEL/openSUSE release; the AppImage is the portable fallback.
- **Ubuntu 24.04 AppImage** may need `libfuse2t64`; use the `.deb` if FUSE is unavailable.
- **No bundled Node.js or hsd in the app package.** The setup wizard can download an optional private runtime; existing Node.js/HSD installations can be reused.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **A fresh HSD chain is large and slow to sync.** Reuse existing HSD data where possible.
- **Swaps are beta.** A funded HNS ↔ DGB swap has completed end-to-end, but payout delivery can still require the recovery workflow from the 0.3.7 release notes. Use small amounts where possible. Escrow has not completed a funded end-to-end trade on both chains yet.
- **Auto-complete is opt-in and local.** Both PCs must run 0.3.8 and enable the toggle for a hands-off swap. HNS funding pauses if the wallet is locked; the passphrase must be entered once. Refunds remain manual, and offers are never auto-accepted.
- **DGB-side payouts use the selected HNS wallet.** Auto-complete claims HNS to the currently selected HNS wallet. Select the intended wallet before enabling it.
- **Escrow is non-custodial but not risk-free.** The arbiter can collude with either party, lost keys can deadlock funds, and there is no automatic arbiter notification.
- **Marketplace and swaps depend on Nostr relays.**
- **DGB support is for swaps only.** There is no general DGB send screen.
- **Electron 28 is end-of-life.** A dependency refresh is in progress.
- **No hardware-wallet support.**

---

## Reporting bugs

Open an issue: <https://github.com/8t9-dev/handess-wallet-releases/issues>

Please include the app version, operating system and version, and the exact error text.

**Never post seed phrases, private keys, WIFs, passphrases, API keys, or signed PSBTs.** Redact addresses and transaction IDs if needed.

---

## License and source

Handess Wallet is **proprietary beta software** distributed under the **Handess Wallet End User License Agreement (EULA)**. It is free to download and use for personal or internal business purposes.

The EULA does **not** permit modifying, editing, forking, creating derivative works based on, or redistributing the app. The source is not published, and no source-code license is granted. The Handess name and logo may not be used for modified versions.

The EULA is available in this repository as [LICENSE](LICENSE), is included in the app package as `LICENSE`, is shown on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party components remain under their own licenses; their notices are included in this repository and the app package as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The optional runtimes include Node.js and hsd license files.

If you do not agree to the EULA, do not install or use the app.
