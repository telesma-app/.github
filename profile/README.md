<p align="center">
  <img src="assets/telesma.svg" alt="Telesma" width="128">
</p>

# Telesma

Telesma is an open-source workbench and Go toolkit for inspecting, managing,
testing, and integrating local FIDO2 authenticators. The project covers CTAP
2.0–2.3, CTAPHID, ISO/IEC 7816-4, PC/SC, the Windows WebAuthn API, and the FIDO
Metadata Service.

## Projects

| Repository | What it provides |
|---|---|
| [`telesma`](https://github.com/telesma-app/telesma) | Wails 3 desktop workbench for inspecting and managing local FIDO2/CTAP authenticators. |
| [`cli`](https://github.com/telesma-app/cli) | Command-line example for building CTAP/FIDO2 tooling on the shared runtime. |
| [`stress`](https://github.com/telesma-app/stress) | Read-only USB CTAP2 stress testing with latency, throughput, and error reports. |
| [`kit`](https://github.com/telesma-app/kit) | Reusable discovery, session, operation, interaction, and safety workflows for authenticator applications. |
| [`mds`](https://github.com/telesma-app/mds) | Client for downloading and verifying FIDO Metadata Service (MDS3) data. |
| [`ctap`](https://github.com/telesma-app/ctap) | Layered CTAP 2.0–2.3 stack with stateful workflows, a command client, backend adapters, transports, and wire types. |
| [`iso7816`](https://github.com/telesma-app/iso7816) | Transport-independent ISO/IEC 7816-4 command and response APDUs. |
| [`token2`](https://github.com/telesma-app/token2) | Token2 device support over PC/SC, USB HID feature reports, and CTAPHID. |
| [`yubico`](https://github.com/telesma-app/yubico) | Yubico-specific device information and identity support. |
| [`winhello`](https://github.com/telesma-app/winhello) | cgo-free Go bindings for the Windows WebAuthn API and Windows Hello. |
| [`hid`](https://github.com/telesma-app/hid) | Cross-platform, cgo-free access to HID devices. |
| [`pcsc`](https://github.com/telesma-app/pcsc) | Minimal, cgo-free PC/SC access for smart cards and security tokens. |
| [`windows-proxy`](https://github.com/telesma-app/windows-proxy) | Windows service and named-pipe bridge for FIDO2 HID access without elevated application rights. |

## Architecture

The diagram follows the primary runtime path. Arrows point from a consumer to
the layer it uses; independent projects and secondary package imports are
omitted.

```mermaid
flowchart TB
    telesma["`telesma
desktop UI and Wails integration`"]
    kit["`kit
device lifecycle and application workflows`"]
    mds["`mds
metadata download and verification`"]

    subgraph ctapStack["ctap · layered protocol stack"]
        direction TB

        authenticator["`authenticator
stateful device workflows`"]
        client["`client
individual CTAP commands`"]
        backends["`backend/*
enumeration and endpoint opening`"]
        transports["`transport/*
CTAPHID and APDU bindings`"]
        types["`protocol and supporting packages
wire types · WebAuthn · extensions · crypto`"]

        authenticator --> client
        authenticator --> backends
        client --> transports
        backends --> transports
        authenticator --> types
        client --> types
        transports --> types
    end

    integrations["`device integrations
token2 · yubico`"]
    hostIO["`host I/O
hid · windows-proxy · pcsc · iso7816`"]

    telesma --> kit
    telesma --> mds
    kit -->|operations| authenticator
    kit -->|conformance| client
    kit -->|discovery and opening| backends
    kit -->|device identity| integrations
    kit -->|device watching| hostIO
    mds --> types
    integrations --> transports
    integrations --> hostIO
    backends --> hostIO
    transports --> hostIO
```
