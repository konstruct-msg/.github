## Konstruct

An end-to-end-encrypted messenger with no phone number, no email and no required
username. An account is a keypair generated on your device; the server only ever
learns the public half.

Cryptography is the Signal Protocol design (X3DH + Double Ratchet) written from
scratch in Rust, extended with post-quantum key agreement: at session setup the
sender encapsulates an **ML-KEM-768** secret to the recipient's Kyber prekey
(PQXDH). The first message of a conversation is classical-only by design;
everything after it is not. All cryptography runs client-side in a shared Rust
core exposed to Swift and Kotlin through UniFFI.

Sealed sender is unconditional for ordinary traffic — the sender field is not in
the envelope. To route a message the server holds an opaque account id, and about
the message itself only its size and the time it arrived. Messages live in Redis
Streams and are trimmed once the client acknowledges delivery by cursor; an
hourly task removes unacknowledged messages after 30 days. The database stores
public keys and routing metadata, never plaintext.

### Status: alpha

This is worth stating plainly, because the rest of this page is a description of
a design and not a claim about maturity.

- **iOS only**, via TestFlight. No App Store release.
- **One server.** Federation is not implemented — the signing and S2S machinery
  exists but is disabled, and clients have no way to point at another server.
  A decentralised relay mesh is a roadmap item, not a shipped property.
- **No external security audit yet.**
- One-to-one chats and calls (audio) work. Group chats, channels, video calling
  and multi-device sync do not exist yet.
- Desktop (macOS), Android and a terminal client are in development with no
  public builds.

### Repository map

**Core**

- **[construct-core](https://github.com/konstruct-msg/construct-core)** — cryptographic
  engine: X3DH, Double Ratchet, PQXDH (ML-KEM-768 + X25519), and hybrid Ed25519 +
  ML-DSA-65 signatures — implemented and cross-verified, not yet active on the wire.
  Shared Rust core with UniFFI bindings.
- **[construct-protos](https://github.com/konstruct-msg/construct-protos)** — protobuf
  definitions shared by the server and every client.

**Server and transport**

- **[construct-server](https://github.com/konstruct-msg/construct-server)** — E2EE server:
  eight gRPC services behind a gateway — messaging, identity, key distribution, groups,
  signalling, media, VEIL and an optional edge binary.
- **[construct-transport](https://github.com/konstruct-msg/construct-transport)** —
  QUIC / HTTP-3 / gRPC transport. The live path is a long-lived bidirectional stream over
  QUIC with HTTP/2 fallback.
- **[construct-veil](https://github.com/konstruct-msg/construct-veil)** —
  censorship-resistant transport. The obfuscation protocol is **veil-front**, an
  honest-front HTTPS relay: unauthenticated traffic is proxied to an ordinary cover site,
  and only clients holding a capability enter the tunnel.

**Clients**

- **[construct-ios](https://github.com/konstruct-msg/construct-ios)** — iOS client (SwiftUI).
- **[construct-desktop](https://github.com/konstruct-msg/construct-desktop)** — desktop
  client (Rust). In development, no public build.
- **[construct-tui](https://github.com/konstruct-msg/construct-tui)** — terminal client.

**Documentation**

- **[construct-protocol](https://github.com/konstruct-msg/construct-protocol)** — protocol
  specification, whitepaper and architecture decision records. Published at
  [konstruct-msg.github.io/construct-protocol](https://konstruct-msg.github.io/construct-protocol/).

An Android client and a store-and-forward relay are developed in private
repositories and are not linked here — a link a visitor cannot open is worse
than no link.

### What is deliberately not published

Which obfuscation techniques are currently deployed, and where they do or do not
get through, are not published anywhere. That is not security through obscurity:
the protocol is open and reviewable in `construct-veil`. It is the difference
between publishing a design and publishing a live target list.

### Licensing

Per component: **AGPL-3.0** for the server and relay, **MPL-2.0** for the clients
and VEIL, **Apache-2.0** for libraries.

Report a security issue privately through
[GitHub Security Advisories](https://github.com/konstruct-msg/construct-core/security/advisories/new).

— [konstruct.cc](https://konstruct.cc)
