## Konstruct

**Privacy is a fundamental human right, not a luxury feature.**

Konstruct is an open-source, federated encrypted messenger built on the Signal Protocol design (X3DH + Double Ratchet) with a hybrid post-quantum key exchange (ML-KEM-768). The server operates on a zero-knowledge principle — messages are held in Redis Streams and deleted once the client acknowledges delivery via cursor-based trim. An hourly background task enforces a 30-day retention ceiling for unacknowledged messages. The database stores only public keys and non-decryptable metadata. All cryptography executes client-side in a shared Rust core with platform-specific Swift/Kotlin wrappers via UniFFI.

The network is evolving from a centralised federated server into a decentralised mesh of lightweight relay nodes.

### Repository Map

| Repository | Description |
|---|---|
| [construct-core](https://github.com/konstruct-msg/construct-core) | Cryptographic engine: X3DH, Double Ratchet, PQXDH (ML-KEM-768 + X25519), hybrid signatures (Ed25519 + ML-DSA-65). Shared Rust core with UniFFI bindings. |
| [construct-server](https://github.com/konstruct-msg/construct-server) | Federated E2EE server: 14+ gRPC microservices, federation gateway, VEIL anti-censorship transport, key distribution, MLS groups. |
| [construct-engine](https://github.com/konstruct-msg/construct-engine) | QUIC transport engine for relay-to-relay and relay-to-client communication. |
| [construct-veil](https://github.com/konstruct-msg/construct-veil) | DPI-resistant transport layer: obfs4, WebTunnel, and the next-generation veil-front protocol. |
| [construct-ios](https://github.com/konstruct-msg/construct-ios) | iOS and macOS client (Swift). |
| [construct-protocol](https://github.com/konstruct-msg/construct-protocol) | Protocol specification, whitepaper, and architecture decision records. Published at [konstruct-msg.github.io/construct-protocol](https://konstruct-msg.github.io/construct-protocol/). |
