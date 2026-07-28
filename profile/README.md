# go-ctap

Go libraries and runtime components for working with FIDO2, CTAP, CTAPHID and platform WebAuthn APIs.

## Repositories

| Repository                                                    | Summary                                                                                                       |
|---------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| [`app`](https://github.com/go-ctap/app)                       | Telesma, a Wails desktop workbench for inspecting and managing local FIDO2/CTAP authenticators.               |
| [`kit`](https://github.com/go-ctap/kit)                       | Shared application runtime for discovery, sessions, operations, interaction prompts, and safety checks.      |
| [`mds`](https://github.com/go-ctap/mds)                       | Client for downloading and verifying FIDO Metadata Service (MDS3) data.                                      |
| [`ctap`](https://github.com/go-ctap/ctap)                     | Core CTAP 2.0–2.3 implementation and stateful authenticator workflows.                                       |
| [`token2`](https://github.com/go-ctap/token2)                 | Token2 device support over PC/SC, USB HID feature reports, and CTAPHID.                                      |
| [`yubico`](https://github.com/go-ctap/yubico)                 | Yubico-specific device information and identity support.                                                     |
| [`iso7816`](https://github.com/go-ctap/iso7816)               | Transport-independent ISO/IEC 7816-4 command and response APDUs.                                             |
| [`hid`](https://github.com/go-ctap/hid)                       | Cross-platform, cgo-free access to HID devices.                                                               |
| [`pcsc`](https://github.com/go-ctap/pcsc)                     | Minimal, cgo-free PC/SC access for smart cards and security tokens.                                          |
| [`windows-proxy`](https://github.com/go-ctap/windows-proxy)   | Windows service and named-pipe bridge for accessing FIDO2 HID devices without elevated application rights.   |

## Project map

Arrows point from each consumer to its direct in-organization dependencies.

```mermaid
flowchart TB
    subgraph application[Application]
        app["app<br/>Telesma desktop workbench"]
    end

    subgraph services[Application services]
        kit["kit<br/>shared workflows and sessions"]
        mds["mds<br/>FIDO metadata"]
    end

    subgraph integrations[Device integrations]
        token2["token2<br/>Token2 support"]
        yubico["yubico<br/>Yubico support"]
    end

    subgraph protocols[Protocols]
        ctap["ctap<br/>CTAP/FIDO2 core"]
        iso7816["iso7816<br/>command and response APDUs"]
    end

    subgraph access[Device access]
        hid["hid<br/>USB HID"]
        pcsc["pcsc<br/>PC/SC"]
        windowsProxy["windows-proxy<br/>Windows named-pipe bridge"]
    end

    app --> kit
    app --> mds
    app --> ctap
    kit --> ctap
    kit --> token2
    kit --> yubico
    kit --> iso7816
    kit --> hid
    kit --> pcsc
    mds --> ctap
    token2 --> ctap
    token2 --> hid
    token2 --> pcsc
    yubico --> ctap
    ctap --> hid
    ctap --> iso7816
    ctap --> windowsProxy
    windowsProxy --> hid
```
