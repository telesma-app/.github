<p align="center">
  <img src="assets/telesma.svg" alt="Telesma" width="128">
</p>

<h1 align="center">Telesma</h1>

<p align="center">
  Open-source tools for security keys.<br>
  A desktop workbench and Go libraries for FIDO2 and PIV.
</p>

## Desktop app

[**Telesma**](https://github.com/telesma-app/telesma) is a desktop workbench for
inspecting, testing, and managing local FIDO2 authenticators.

- Inspect connected devices, capabilities, and FIDO metadata.
- Manage discoverable passkeys, PINs, and authenticator settings.
- Run WebAuthn credential creation and assertion scenarios and inspect the results.

Available operations depend on the connected authenticator.

## Go libraries

Build on the shared application runtime or use the protocol and platform
libraries directly.

| Libraries | Purpose |
|---|---|
| [`kit`](https://github.com/telesma-app/kit) | Device discovery, sessions, and application workflows. |
| [`ctap`](https://github.com/telesma-app/ctap) · [`piv`](https://github.com/telesma-app/piv) · [`iso7816`](https://github.com/telesma-app/iso7816) | FIDO2/CTAP, PIV smart cards, and ISO 7816 APDUs. |
| [`token2`](https://github.com/telesma-app/token2) · [`yubico`](https://github.com/telesma-app/yubico) | Vendor device information and PIV extensions. |
| [`mds`](https://github.com/telesma-app/mds) | FIDO Metadata Service download and verification. |
| [`hid`](https://github.com/telesma-app/hid) · [`pcsc`](https://github.com/telesma-app/pcsc) · [`winhello`](https://github.com/telesma-app/winhello) · [`windows-proxy`](https://github.com/telesma-app/windows-proxy) | HID, smart cards, Windows WebAuthn, and a Windows HID proxy. |

See the [architecture overview](ARCHITECTURE.md) for how the projects fit together.

## Command-line tools

- [`cli`](https://github.com/telesma-app/cli) — a CTAP/FIDO2 command-line example built on the shared runtime.
- [`stress`](https://github.com/telesma-app/stress) — USB CTAP2 stress testing with latency, throughput, and error reports.
