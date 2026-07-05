# Chain Notes — Companion

**Hosted page: https://objsal.github.io/chain-notes-companion/**

The online half of *Chain Notes*, a Passport Prime (KeyOS) app that writes
personal notes onto the bitcoin blockchain from a device that has no
network on purpose. This page does the two things the offline device
can't:

1. **Build sync bundles** — scans a notes address via the
   [mempool.space](https://mempool.space) API (mainnet / testnet4 /
   signet): UTXOs, full transaction history (paginated), OP_RETURN
   payloads, fee tiers and BTC price — packaged as a `bundle.json`
   download or shown as a **QR (static, or animated UR for big
   bundles)** that the device's scanner reads directly.
2. **Broadcast** the transactions the device signs — drop the exported
   `.hex` files, paste hex, or **scan the tx QR off the device's screen
   with the camera**; reject reasons are surfaced verbatim.

Vendored libraries: [jsQR](https://github.com/cozmo/jsQR) (Apache-2.0),
[qrcode-generator](https://github.com/kazuhikoarase/qrcode-generator)
(MIT); `ur.js` is a minimal hand-rolled BC-UR encoder, byte-verified
against the `foundation-ur` crate the device runs.

No keys ever touch this page. Bundles contain only public chain data;
the transactions it broadcasts are already signed and final.

## Local use with regtest

```bash
python3 server.py 8091 --regtest
```

serves this same page at `http://localhost:8091` plus a throwaway Bitcoin
Core regtest node exposed through a mempool.space-shaped API — the page
then shows a "Regtest (local server)" network option and a faucet. Needs
`bitcoind`/`bitcoin-cli` on PATH (Core v30+ recommended).

## Provenance

This repo is a **deploy mirror**: the canonical source (with the app,
protocol docs and the playwright test suites, including the live testnet4
verification where mempool.space accepted a 224-byte single OP_RETURN)
lives in the private `prime-chain-notes` repository and is published here
by its `scripts/publish-companion.sh`. GPL-3.0-or-later.
