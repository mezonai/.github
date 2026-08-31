<div align="center">
  <h1>Mezon</h1>

  <p align="center">
    <strong>A high-performance, native-first alternative to Discord.</strong><br>
    Live, work, and play — without the latency, the middleware, or the lock-in.
  </p>

  <p align="center">
    <a href="https://mezon.ai"><img src="https://img.shields.io/badge/Try%20Live-mezon.ai-blue?style=flat-square" alt="Try Live"></a>
    <a href="https://mezon.ai/invite/1840696977034055680"><img src="https://img.shields.io/badge/Join-Community-purple?style=flat-square" alt="Join Community"></a>
    <a href="https://github.com/mezonai/mezon/blob/main/LICENSE"><img src="https://img.shields.io/github/license/mezonai/mezon?style=flat-square" alt="License"></a>
    <a href="https://github.com/orgs/mezonai/repositories"><img src="https://img.shields.io/badge/Contributions-Welcome-green?style=flat-square" alt="Contributions Welcome"></a>
  </p>
</div>

---

## Why Mezon exists

Most "Discord alternatives" are thin wrappers around the same generic stack: HTTP APIs, off-the-shelf WebRTC SDKs, a JS server, a hybrid mobile shell. That gets you feature parity, not performance.

Mezon is built the opposite way: **own the hot path end to end.** Chat, voice, and video run on infrastructure we wrote ourselves — native clients on every platform, a custom C server stack, and a purpose-built WebRTC SFU — so there's no generic middleware sitting between a user's action and the network. The result is sub-millisecond response times and support for millions of concurrent connections, on infrastructure that's fully open source.

## The native-first architecture

| Layer | Repo | What makes it fast |
|---|---|---|
| **Voice/video SFU** | [mezon-sfu](https://github.com/mezonai/mezon-sfu) | Custom C WebRTC SFU. Lock-free, per-room worker threads; packets are referenced through `io_uring` (`recv` + `SEND_ZC`), never copied. |
| **Native media engine** | [libmezia](https://github.com/mezonai/libmezia) | C11 client engine for iOS/Android, wire-compatible with mezon-sfu — no full `PeerConnection` tree, no hybrid runtime tax. Hardware-accelerated codecs, lock-minimal Opus voice. |
| **Protocol / data plane** | [mezon-protocol](https://github.com/mezonai/mezon-protocol) | High-performance C server on `io_uring` with SQPOLL and fixed files/buffers. Zero-copy encrypt/decrypt via a custom BoringSSL integration straight into registered buffers. |
| **API / backend** | Go backend | WebSocket fan-out, worker pools, ScyllaDB queries, Valkey caching, NATS messaging. |
| **Desktop** | [mezon-desktop](https://github.com/mezonai/mezon-desktop) | Rust + GPUI — renders UI directly on the GPU, not an Electron/Chromium shell. |
| **iOS** | [mezon-ios](https://github.com/mezonai/mezon-ios) | True native Swift client, not a hybrid wrapper. |
| **Android** | [mezon-android](https://github.com/mezonai/mezon-android) | True native Kotlin client, not a hybrid wrapper. |
| **Web** | [mezon](https://github.com/mezonai/mezon) | React/Nx monorepo — the web chat client and admin dashboard. |

Every layer speaks the same lean, binary-first protocol, so there's no translation tax between the client on someone's phone and the server handling millions of connections.

## What you get

- **⚡ Performance by construction** — sub-millisecond latency and massive concurrency come from architecture (io_uring, zero-copy I/O, lock-free workers), not from throwing hardware at a slow stack.
- **📱 Genuinely native clients** — desktop, iOS, Android, and web are each built for their platform, not one hybrid codebase stretched across all of them.
- **🔓 Open source, top to bottom** — the server, the SFU, the media engine, and every client are public repos in this org.
- **🔧 Extensible** — REST/WebSocket APIs, a bot framework, and SDKs in JS/TS, Go, Java, Python, .NET, and NestJS.
- **🔒 Security by default** — end-to-end encryption, TLS 1.3 / DTLS+SRTP on the media path, zero-knowledge architecture.

## Get started

**Using Mezon**
- Web: [mezon.ai](https://mezon.ai) — no install needed
- Desktop / iOS / Android: see the [mezon-desktop](https://github.com/mezonai/mezon-desktop), [mezon-ios](https://github.com/mezonai/mezon-ios), and [mezon-android](https://github.com/mezonai/mezon-android) repos for downloads

**Building with Mezon**
- Start with the [mezon](https://github.com/mezonai/mezon) web repo for the client stack, or dig into [mezon-sfu](https://github.com/mezonai/mezon-sfu), [libmezia](https://github.com/mezonai/libmezia), and [mezon-protocol](https://github.com/mezonai/mezon-protocol) for the media and protocol internals
- SDKs: [JS/TS](https://github.com/mezonai/mezon-js) · [Go](https://github.com/quangledang23/mezon-sdk-go) · [Java](https://github.com/mezonai/mezon-java-sdk) · [Python](https://github.com/phuvinh010701/mezon-sdk-python) · [.NET](https://github.com/huy-buidoanquang/Mezon.NET) · [NestJS](https://github.com/n0xgg04/nezon)
- Bots: [bot example repo](https://github.com/mezonai/mezon-bot-example)

## Contributing

We're building this in the open. Bug reports, feature ideas, and pull requests are all welcome — check each repo's issue tracker to get started, and join the [community](https://mezon.ai/invite/1840696977034055680) if you want to talk architecture with the team directly.

---

<div align="center">
  <p><strong>Mezon — your world, your clan.</strong></p>
  <p>
    <a href="https://mezon.ai">mezon.ai</a> ·
    <a href="https://mezon.ai/invite/1840696977034055680">Community</a> ·
    <a href="https://github.com/mezonai/mezon/issues/new/choose">Report an issue</a>
  </p>
</div>
