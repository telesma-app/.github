# Architecture

[← Telesma overview](README.md)

The diagram shows the desktop runtime and the layered CTAP, PIV and OpenPGP stacks.
Arrows point from a consumer to a dependency; standalone tools and secondary
package imports are omitted.

`kit` uses vendor device information for FIDO2 discovery and identity. PIV
workflows use `piv` directly; `token2/piv` and `yubico/piv` extend its card
sessions with slot metadata, attestation, key import and generation with
PIN/touch policies, and administration commands. `openpgp` provides an independent
card/client stack over the same ISO 7816 and PC/SC layers. `bertlv` supplies shared
encoding primitives for the card protocols and secure channels.

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

    subgraph pivStack["piv · layered protocol stack"]
        direction TB

        pivCard["`card
stateful card workflows`"]
        pivClient["`client
individual PIV commands`"]
        pivBackends["`backend/pcsc
reader discovery and opening`"]
        pivTransports["`transport/*
ISO 7816 · NIST secure messaging`"]
        pivTypes["`protocol
PIV objects · algorithms · encodings`"]

        pivCard --> pivClient
        pivClient --> pivTransports
        pivBackends --> pivTransports
        pivCard --> pivTypes
        pivClient --> pivTypes
        pivTransports --> pivTypes
    end

    subgraph openpgpStack["openpgp · layered protocol stack"]
        direction TB

        openpgpCard["`card
stateful card workflows`"]
        openpgpClient["`client
individual OpenPGP commands`"]
        openpgpBackends["`backend/pcsc
reader discovery and opening`"]
        openpgpTransports["`transport/*
ISO 7816 · AES secure messaging`"]
        openpgpTypes["`protocol
OpenPGP objects · algorithms · encodings`"]

        openpgpCard --> openpgpClient
        openpgpClient --> openpgpTransports
        openpgpBackends --> openpgpTransports
        openpgpCard --> openpgpTypes
        openpgpClient --> openpgpTypes
        openpgpTransports --> openpgpTypes
    end

    bertlv["`bertlv
BER-TLV encoding and parsing`"]
    integrations["`vendor device information
token2 · yubico`"]
    pivExtensions["`PIV vendor extensions
token2/piv · yubico/piv`"]
    hostIO["`host I/O and APDU support
hid · ble · windows-proxy · pcsc · iso7816`"]

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
    pivExtensions --> pivCard
    pivBackends -->|PC/SC| hostIO
    pivTransports -->|ISO 7816| hostIO
    openpgpBackends -->|PC/SC| hostIO
    openpgpTransports -->|ISO 7816| hostIO
    openpgpTypes --> bertlv
    pivTypes --> bertlv
    hostIO -->|SCP11b| bertlv
    backends --> hostIO
    transports --> hostIO
```
