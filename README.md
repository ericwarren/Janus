# Janus

> A personal edge network, built in Rust, one protocol at a time.

Janus is a long-horizon learning project: a self-hosted egress proxy that will grow into a small, multi-node private network — my own miniature version of the infrastructure that products like Cloudflare WARP are built on. It is named for the Roman god of gates, doorways, and transitions, which felt right for a project about routing traffic through thresholds — and about a career transition into systems programming.

**This is a learning project.** I am a line-of-business developer of 20 years teaching myself Rust, Linux networking, and distributed systems in public. Expect honest commits, honest benchmarks, and honest mistakes. Do not route traffic you care about through this — see [Non-Goals](#non-goals).

## Why this exists

I set myself a three-year goal: go from zero systems programming experience to being genuinely qualified for senior network-infrastructure roles. Courses and books alone don't get you there — running something does. Janus is the "running something." When it breaks, my own internet breaks, which is the closest a solo developer gets to a real on-call rotation.

## What it will become

A device (laptop, phone) connects over WireGuard to a Janus node running on a cheap VPS. The node terminates the tunnel and egresses traffic to the destination, with every connection measured, logged, and visible on a dashboard. Eventually: multiple nodes in multiple regions, health checks, failover, and a small control plane distributing configuration.

```
[ device ] --WireGuard--> [ janus node ] --egress--> [ the internet ]
                               |
                        metrics + logs
                               |
                    [ Prometheus / Grafana / ClickHouse ]
```

## Roadmap

The project is structured as five phases over roughly three years. Each phase ends with a milestone I can demonstrate, not just describe.

### Phase 0 — Foundations
- [ ] TCP echo server in Rust (std::net, blocking I/O)
- [ ] netcat clone: connect, listen, pipe stdin/stdout over TCP
- [ ] Notes on TCP handshakes, sockets, and byte-order footguns in `/docs`

### Phase 1 — Speak the protocol
- [ ] HTTP/1.1 server that parses raw bytes off the socket — no frameworks
- [ ] Forward proxy with `CONNECT` tunneling
- [ ] **Milestone:** browse the web with my browser's proxy settings pointed at my own code

### Phase 2 — Make it real
- [ ] Async rewrite on tokio
- [ ] TLS via rustls; SOCKS5 support
- [ ] Graceful config reload without dropping connections
- [ ] Prometheus metrics + Grafana dashboard
- [ ] **Milestone:** deployed on a VPS, my phone routed through it full-time

### Phase 3 — Become a network
- [ ] WireGuard tunnel termination (studying [boringtun](https://github.com/cloudflare/boringtun))
- [ ] Second node in a second region; health checks and failover
- [ ] Minimal control plane distributing node config
- [ ] Connection logs flowing into ClickHouse
- [ ] **Milestone:** kill one node mid-browsing and stay online

### Phase 4 — Prove it
- [ ] Load testing (wrk/vegeta) with published, reproducible numbers
- [ ] Flamegraph-driven optimization; kernel tuning; io_uring experiments
- [ ] Upstream contributions to Rust networking OSS
- [ ] **Milestone:** a benchmark report I'd be willing to defend in an interview

## Non-Goals

- **Not a product.** No releases, no support, no stability promises.
- **Not a privacy tool.** Until Phase 3 is mature, assume the crypto and the operational security are wrong. Use a real VPN for anything that matters.
- **Not a framework showcase.** Early phases deliberately avoid libraries that would do the learning for me. Dependencies get added when I understand what they're replacing.

## Principles

1. **Run what you build.** If it isn't deployed and carrying my own traffic, it doesn't count.
2. **Measure, don't assert.** Performance claims come with methodology and raw numbers or they don't get made.
3. **Spec first.** Non-trivial changes start as a short design doc in `/docs/specs`.
4. **Learn in public.** Wrong turns stay in the history. The commit log is part of the portfolio.

## Reading list

The books and resources driving each phase live in [`/docs/reading.md`](docs/reading.md). Highlights: *The Rust Programming Language*, Beej's Guide to Network Programming, *TCP/IP Illustrated Vol. 1*, and *Designing Data-Intensive Applications*.

## Status

🚧 **Phase 0.** Watch this space — for about three years.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.
