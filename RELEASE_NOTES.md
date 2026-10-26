# Handess Wallet 0.2.0 — Windows Beta

Self-custody Handshake wallet, domain CRM, peer-to-peer marketplace, and HNS ↔ DGB atomic swaps.

## Download

- **Installer:** `Handess-Wallet-0.2.0.msi` (Windows 10/11 x64, 95.7 MB)
- **SHA-256:** `CF112D5CEB97F726A6D5F80A30C99161A9692365DAB6305999FD82182BC11614`

## Before installing

1. Windows 10/11 x64.
2. Install [Node.js](https://nodejs.org/) 20 LTS or newer and run `npm install -g hsd`, **or** connect Handess to an existing hsd node in Settings.
3. Close Bob Wallet or any other wallet using ports 12037/12039.
4. Back up your seed phrase.
5. Review the EULA shown on first launch.

The installer is **unsigned**. If SmartScreen appears, choose **More info → Run anyway**.

## License

This build is distributed under the **Handess Wallet End User License Agreement (EULA)**, a proprietary license. It is free to download and use, but modification, editing, forking, derivative works, and redistribution are not permitted. The source is not published, and no source-code license is granted.

The EULA is included in the installer as `LICENSE`, is presented on first launch, and can be reviewed later from **Settings → About & Legal**. Third-party notices are included as `THIRD_PARTY_NOTICES.md`.

## Highlights

- HNS wallet with multiple wallets, passphrase encryption, send/receive, history, and message signing.
- Domain CRM with tags, expiry timeline, bulk renew/transfer/DNS, watchlist, and bulk search.
- Auction management: open, bid, reveal, redeem, register.
- P2P name marketplace over Nostr with transfer-first trades and optional 2-of-3 escrow.
- HNS → DGB and DGB → HNS atomic swaps with background monitoring and recovery.
- Local hsd node management plus external node support.
- Block/name explorer and dark mode.

## Beta caveats

- Unsigned and not independently audited.
- Windows x64 only in this release.
- No bundled Node.js/hsd.
- No funded end-to-end swap or escrow trade has been completed on both chains yet; test with small amounts.
- Escrow arbiter trust is real; lost keys can deadlock funds.
- Marketplace and swaps depend on Nostr relays.
- DGB support is for swaps only; there is no general DGB send screen.
- Electron 28 is end-of-life; dependency refresh is in progress.

## Feedback

Open an issue: https://github.com/8t9-dev/handess-wallet-releases/issues

**Never share seed phrases, private keys, WIFs, passphrases, API keys, or sensitive signed PSBTs.**
