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
| **0.3.5 beta** | Windows 10/11 x64 | **[Handess-Wallet-0.3.5.msi](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-Wallet-0.3.5.msi)** (97.8 MB) | `A9A7F4AB3EAC30502CCB1A8A1F13B2C0D3497B3C4DAC931CA6765D5C38381120` |
| **0.3.5 beta** | Linux x64 (portable) | **[Handess-Wallet-0.3.5-x86_64.AppImage](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-Wallet-0.3.5-x86_64.AppImage)** (122.2 MB) | `BCFE55EBB5BD648C1015165C08D11D6C5BCA18AB4166745D7653865A39524B28` |
| **0.3.5 beta** | Ubuntu / Debian x64 | **[Handess-Wallet-0.3.5-amd64.deb](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-Wallet-0.3.5-amd64.deb)** (82.6 MB) | `4E520CEFDBF576139E59E147EB03C251E3786EE92B5CFB8E666F2C20E09AD847` |
| **0.3.5 beta** | Fedora / RHEL x64 | **[Handess-Wallet-0.3.5-x86_64.rpm](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-Wallet-0.3.5-x86_64.rpm)** (81.0 MB) | `F54CA9E953672CD825FE03D629CA979DA363AB978064A269EB27C5190E153905` |
| **Optional runtime** | Windows x64 | **[Handess-HSD-Runtime-8.0.0-win-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-HSD-Runtime-8.0.0-win-x64.zip)** (79.5 MB) | `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA` |
| **Optional runtime** | Linux x64 | **[Handess-HSD-Runtime-8.0.0-linux-x64.zip](https://github.com/8t9-dev/handess-wallet-releases/releases/download/v0.3.5/Handess-HSD-Runtime-8.0.0-linux-x64.zip)** (53.3 MB) | `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE` |

The app packages contain the wallet only. The optional runtime contains Node.js 24.15.0 and hsd 8.0.0 and is used by the setup wizard for **New to Handshake** users. Existing Node.js or HSD installations are detected and can be reused.

All files are also on the [Releases](https://github.com/8t9-dev/handess-wallet-releases/releases) page.

> **0.3.4 and earlier are superseded.** 0.3.5 fixes swap event synchronization between the two PCs: the DGB side now applies the HNS side’s terms and funding events and can continue the swap.

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

### AppImage (portable)

```bash
chmod +x Handess-Wallet-0.3.5-x86_64.AppImage
./Handess-Wallet-0.3.5-x86_64.AppImage
```

If FUSE is unavailable, run:

```bash
./Handess-Wallet-0.3.5-x86_64.AppImage --appimage-extract-and-run
```

The setup wizard can be re-run later from **Settings → Node & HSD setup**.

### Upgrading from 0.3.4

There is **no auto-updater**. Download and install 0.3.5 on both PCs. Wallet data and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

For swaps already accepted before 0.3.5, update the **DGB-side** PC first. It will pick up the existing HNS terms and funding events. The HNS side should also be updated before starting new swaps.

If your node is already configured, the wizard is skipped and your existing HSD settings remain in use.

---

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

The same checksums are attached to the release as `SHA256SUMS.txt`.

---

## What is in 0.3.5

- **Swap event synchronization fixed** — swap terms, funding, secret, and status events now use the shared offer ID instead of the initiator’s local swap ID. The DGB side can resolve the events and progress past **Accepted**.
- **Existing HNS → DGB swaps recover** — after updating the DGB-side PC, the monitor can apply the already-published terms and HNS funding events.
- **Everything from 0.3.4**, including the pure-JS SHA3 fix for HNS HTLC/escrow derivation and the `Digest method not supported` fix.
- **Everything from 0.3.3**, including the DGB Electrum notification crash fix, marketplace transfer detection fixes, Linux packages, the DGB WIF QR, Finalize All, per-name transaction-history rows, received-name fee handling, and copyable transaction hashes.

> Older **DGB → HNS** swaps accepted before 0.3.5 may still be stuck if the HNS side already published terms under its old local ID. Re-accept those swaps with both PCs on 0.3.5.

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

The EULA does **not** permit modifying, editing, forking, creating derivative works based on, or redistributing the app. The source is not published, and no source-code license is granted. The Handess name and logo may not be used for modified versions.

The EULA is available in this repository as [LICENSE](LICENSE), is included in the app package as `LICENSE`, is shown on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party components remain under their own licenses; their notices are included in this repository and the app package as [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). The optional runtimes include Node.js and hsd license files.

If you do not agree to the EULA, do not install or use the app.
