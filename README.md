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
| **0.3.3 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.3.3.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-Wallet-0.3.3.msi)** (97.8 MB) | `5256626703BC692777598A1E62CD0325FA82B5333A3E01B5033DAA46FF5CA8E1` |
| **0.3.3 beta** | Linux x64 (portable) | **[Handess-Wallet-0.3.3-x86_64.AppImage](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-Wallet-0.3.3-x86_64.AppImage)** (122.2 MB) | `32B1961951CD2ECB8C28E3AA6066FBC72B5015C8108EA6C9A48E959DA563087D` |
| **0.3.3 beta** | Ubuntu / Debian x64 | **[Handess-Wallet-0.3.3-amd64.deb](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-Wallet-0.3.3-amd64.deb)** (82.6 MB) | `468809B1096F7FDE886C33F2EFC2A27D23C5B906C16EAD9EAF64936B862432BB` |
| **0.3.3 beta** | Fedora / RHEL x64 | **[Handess-Wallet-0.3.3-x86_64.rpm](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-Wallet-0.3.3-x86_64.rpm)** (81.0 MB) | `DDF14B83B73982AB4E8E7E9CCEEF19650DCA3FB8A58AB6964C5904BA20E342EF` |
| **Optional runtime** | Windows x64 | **[Handess-HSD-Runtime-8.0.0-win-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-HSD-Runtime-8.0.0-win-x64.zip)** (79.5 MB) | `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA` |
| **Optional runtime** | Linux x64 | **[Handess-HSD-Runtime-8.0.0-linux-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.3/Handess-HSD-Runtime-8.0.0-linux-x64.zip)** (53.3 MB) | `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE` |

The app packages contain the wallet only. The optional runtime contains Node.js 24.15.0 and hsd 8.0.0 and is used by the setup wizard for **New to Handshake** users. Existing Node.js or HSD installations are detected and can be reused.

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.3.2 and earlier are superseded.** 0.3.3 fixes a DGB swap main-process crash caused by Electrum server notifications, and prevents a pending name transfer from being reported as cancelled in the marketplace.

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

### AppImage (portable)

```bash
chmod +x Handess-Wallet-0.3.3-x86_64.AppImage
./Handess-Wallet-0.3.3-x86_64.AppImage
```

If FUSE is unavailable, run:

```bash
./Handess-Wallet-0.3.3-x86_64.AppImage --appimage-extract-and-run
```

The setup wizard can be re-run later from **Settings → Node & HSD setup**.

### Upgrading from 0.3.2

There is **no auto-updater**. Download and install the 0.3.3 package for your platform. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

If your node is already configured, the wizard is skipped and your existing HSD settings remain in use.

---

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

The same checksums are attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.3.3

- **DGB swap crash fixed** — DGB Electrum header notifications no longer trigger a main-process JavaScript error. The client now routes notifications safely and contains malformed server messages.
- **Marketplace transfer detection fixed** — a normal `CLOSED` name is no longer treated as a cancelled transfer while the seller's transfer is still waiting to confirm.
- **Pay seller is gated on-chain** — My Purchases disables the payment action until the name transfer is detected on-chain. The table shows “Waiting for on-chain transfer…” instead of the misleading cancellation error.
- **Payment after finalize** — if the buyer finalized the name before paying, the seller can still be paid.
- **Everything from 0.3.2**, including Linux packages, the DGB WIF QR, Finalize All, per-name transaction-history rows for batched finalizes, correct fee handling for received names, and click-to-copy transaction hashes.

---

## Beta limitations

- **Unsigned and unaudited.** Windows SmartScreen will warn you. Linux packages are not GPG-signed.
- **Windows x64 and Linux x64 only** in this release. macOS is not published yet.
- **Linux RPM testing is limited.** The `.rpm` is built but has not been installed on every Fedora/RHEL/openSUSE release; the AppImage is the portable fallback.
- **Ubuntu 24.04 AppImage** may need `libfuse2t64`; use the `.deb` if FUSE is unavailable.
- **No bundled Node.js or hsd in the app package.** The setup wizard can download an optional private runtime; existing Node.js/HSD installations can be reused.
- **No auto-updater.** Check the Releases page for new versions and install them manually.
- **A fresh HSD chain is large and slow to sync.** Reuse existing HSD data where possible.
- **Swaps and escrow have not completed a funded end-to-end trade on both chains yet.** Use small amounts on testnet/regtest where possible.
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

The EULA does **not** permit modifying, editing, forking, creating derivative works from, or redistributing the app. The source is not published, and no source-code license is granted. The Handess name and logo may not be used for modified versions.

The EULA is available in this repository as [LICENSE](LICENSE), is included in the app package as `LICENSE`, is shown on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party components remain under their own licenses; their notices are included in this repository and the app package as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The optional runtimes include Node.js and hsd license files.

If you do not agree to the EULA, do not install or use the app.
