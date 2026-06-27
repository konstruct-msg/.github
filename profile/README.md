## Construct Messenger

**Construct** — это федеративный мессенджер с сквозным шифрованием (E2EE) и пост-квантовой устойчивостью. Серверная архитектура построена на zero-knowledge принципе: сервер никогда не видит открытый текст сообщений — вся криптография выполняется на стороне клиента в Rust-ядре (`construct-core`) с платформенными оболочками под iOS, Android и macOS через UniFFI.

Протокол базируется на X3DH + Double Ratchet (Signal Protocol) с гибридным PQXDH-режимом (ML-KEM-768 + X25519), обеспечивая forward secrecy и post-compromise security. Сеть состоит из 14+ микросервисов (gRPC, Kafka, Redis, Envoy), поддерживает федерацию, DPI-устойчивые транспорты (obfs4, WebTunnel), и прошла два завершённых security аудита.
