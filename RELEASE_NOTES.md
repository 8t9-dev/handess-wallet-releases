# Handess Wallet 0.3.7 — HNS Swap Payout Recovery

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps. This release fixes HNS claim transaction encoding and makes HNS swap payouts auditable and recoverable: the DGB side now saves the HNS claim transaction and payout address, verifies the claim against the local HNS node after the swap completes, and can rebroadcast a dropped claim.

The HNS payout from the first completed HNS ↔ DGB swap did not arrive because its claim transaction was rejected by hsd. The claim used transaction version 2, but hsd policy only accepts version 0, and it reversed the outpoint txid even though HNS serializes outpoints in display order. 0.3.7 corrects both. A corrected claim for that swap was broadcast and confirmed; the recovery controls below cover any swap still missing its payout.

## Download

### Windows x64

- **Installer:** `Handess-Wallet-0.3.7.msi` (97.8 MB)
- **MSI SHA-256:** `9608CAB91F24D304C0B20521D8BF2888D38DC9A12A5C88F405C5B715A8E581F4`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-win-x64.zip` (79.5 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Runtime SHA-256:** `332930EC7D336DD98F82408413FA539ED240D4B7976BE0D05B008C5C2BDBB8DA`

### Linux x64

- **Portable AppImage:** `Handess-Wallet-0.3.7-x86_64.AppImage` (122.3 MB)
- **AppImage SHA-256:** `B22080AD80227870B4D35BE6C174096E48DFE1B4752BA46179030A945495E89C`
- **Ubuntu/Debian:** `Handess-Wallet-0.3.7-amd64.deb` (82.6 MB)
- **Deb SHA-256:** `8E037DF5D98F24C590F6B77C93AEBE995E0CA160BF13B8649C05CB0E3B429995`
- **Fedora/RHEL:** `Handess-Wallet-0.3.7-x86_64.rpm` (81.0 MB)
- **RPM SHA-256:** `AFD4AD0FDDD9F0F81DFFC1005399BFE72BC54ADC2C990171903E1AB1BABDA68A`
- **Optional HSD runtime:** `Handess-HSD-Runtime-8.0.0-linux-x64.zip` (53.3 MB, Node.js 24.15.0 + hsd 8.0.0)
- **Linux runtime SHA-256:** `45BBB5B5BB8B1A9DC7AB9F5F1DBDAE9DB2F301EEE684D4EAC38A6BB1FD15DADE`

The app packages contain the wallet only. The optional runtime is downloaded separately by the setup wizard for **New to Handshake** users, or can be selected as a local zip. Existing Node.js or HSD installations are detected and can be reused.

## Linux install

### Ubuntu / Debian

```bash
sudo apt install ./Handess-Wallet-0.3.7-amd64.deb
handess-wallet
```

### Fedora / RHEL

```bash
sudo dnf install ./Handess-Wallet-0.3.7-x86_64.rpm
handess-wallet
```

openSUSE may require allowing the unsigned package:

```bash
sudo zypper install --allow-unsigned-rpm ./Handess-Wallet-0.3.7-x86_64.rpm
```

### AppImage

```bash
chmod +x Handess-Wallet-0.3.7-x86_64.AppImage
./Handess-Wallet-0.3.7-x86_64.AppImage
```

If FUSE is unavailable:

```bash
./Handess-Wallet-0.3.7-x86_64.AppImage --appimage-extract-and-run
```

Ubuntu 24.04 may need `libfuse2t64` for the AppImage. The `.deb` is recommended on Ubuntu/Debian.

Linux packages are built on Ubuntu 22.04. Intended baseline: Ubuntu 22.04+, Debian 12+, and recent Fedora/RHEL releases. The RPM has not been installed on every distribution release; use the AppImage as a fallback.

## What changed

### HNS claim transactions are now valid

- **Transaction version:** hsd sets `MAX_TX_VERSION = 0` in its mempool policy. HNS claim, refund, and escrow transactions now use version 0. The previous build used version 2, so nodes rejected claims as `nonstandard` with `reason=version`.
- **Outpoint byte order:** HNS outpoints serialize the txid in display order. The previous builder reversed the txid the way Bitcoin does, so the input pointed at a non-existent outpoint and the node treated the claim as an orphan even after the version was fixed.
- Both issues are fixed for HTLC claims, HTLC refunds, and 2-of-3 escrow spends.
- Regression tests assert version 0, standardness, and correct outpoint encoding.

These two bugs are why the completed DGB → HNS swap's claim never reached the HNS chain, even though the swap was marked completed. The HNS HTLC remained funded and was recovered with a corrected claim.

### HNS claim details are saved

When the DGB side claims an HNS HTLC, the app now records:

- the claim transaction ID
- the HNS payout address
- the claim amount
- the broadcast time
- the raw signed claim transaction

**My Swaps** shows the claim transaction, its status, the receiving address, and the amount for the completed swap.

### Claims are verified against the HNS node

- The swap monitor re-checks the HNS claim during status refreshes, including after the swap is marked completed.
- The claim is shown as **broadcast**, **confirmed**, or **not found**.
- A confirmed claim is never marked missing or offered for rebroadcast, even if the node connection is temporarily unavailable.
- If it is confirmed, **HNS received at** shows the exact address that was paid and **HNS claim amount** shows the value.
- If that address belongs to another HNS wallet on the same PC, switch to that wallet. The payout is not lost; it was sent to the address recorded by the claim.

Previously, completed swaps were skipped by the monitor, so a claim that was dropped before mining was never noticed.

### Dropped claims can be rebroadcast

- If the HNS node cannot find the claim transaction for about two minutes, the swap marks it **not found** and enables **Rebroadcast HNS Claim**.
- Rebroadcast rebuilds the claim with the same HTLC secret. It pays the destination recorded by the swap when one is known. For older swaps with no recorded destination, it pays the **currently selected HNS wallet**.
- Rebroadcast is only allowed while the HNS HTLC is still funded and has enough confirmations.

### Recovering the recent missing HNS payout

1. Install 0.3.7 on the **DGB-side PC**. The HNS-side PC does not need this update to recover the payout.
2. Open **My Swaps** and expand the completed swap.
3. Read the **HNS claim** transaction and the **HNS received at** address.
4. If the claim is **confirmed**, the HNS is at that address. If it is not the wallet currently selected, switch to the matching HNS wallet.
5. If the claim is **not found**, select the HNS wallet that should receive the swap, then click **Rebroadcast HNS Claim**. 0.3.7 rebuilds the claim with the corrected version and outpoint encoding.
6. If the payout still does not appear, open an issue with the claim transaction ID and destination address.

**Never post seed phrases, private keys, WIFs, or passphrases.**

### Retained from 0.3.6

- DGB HTLC funding, claim, and refund transactions sign the raw sighash. The previous build signed `SHA256(sighash)` because Noble v2 prehashes by default, and DigiByte nodes rejected the invalid signature.

### Retained from 0.3.5

- Swap events use the shared offer ID, so the DGB side applies terms and HNS funding and can progress past **Accepted**.

### Retained from 0.3.4

- HNS HTLC and escrow address derivation uses the bundled pure-JS SHA3-256 implementation, fixing `Digest method not supported` on the packaged Electron runtime.

### Retained from 0.3.3 and earlier

- DGB Electrum header notifications no longer crash the main process.
- Marketplace transfer detection no longer reports a pending transfer as cancelled.
- Pay seller is enabled only after the name transfer is detected on-chain.
- Linux AppImage, `.deb`, and `.rpm` packages plus the Linux private HSD runtime.
- DGB WIF QR, Finalize All, per-name History rows, received-name fee handling, clickable transaction hashes, exact-amount HNS sends, and the dark-mode transfer-address fix.

## Upgrading from 0.3.6

There is **no auto-updater**. Download and install 0.3.7. Wallet data, swap history, and HSD data live outside the install directory and are preserved. Back up your seed phrase first.

For a completed swap whose HNS payout is missing, update the **DGB-side PC** and use the claim details and **Rebroadcast HNS Claim** action described above.

## Verify the downloads

### Windows (PowerShell)

```powershell
Get-FileHash .\Handess-Wallet-0.3.7.msi -Algorithm SHA256
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
- A funded HNS ↔ DGB swap has completed end-to-end, but payout delivery can still require the recovery workflow above. Use small amounts where possible. Escrow has not completed a funded end-to-end trade on both chains yet.
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
