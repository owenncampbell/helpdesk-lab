# Remote Support Tooling

A real, self-hosted [RustDesk](https://rustdesk.com/) server (`hbbs` + `hbbr`) running via Docker Compose — not a mockup. This is the rendezvous/relay backend a helpdesk tech would use to remote into an end user's machine, instead of relying on RustDesk's public servers.

## Run it

```bash
cd remote-support
docker compose up -d
```

Both containers come up immediately — no install wizard, unlike osTicket. On first start, `hbbs` generates a keypair under `data/` (gitignored) and prints its public key to the logs:

```bash
docker compose logs hbbs | grep Key:
```

## Stack

| Service | Image | Purpose |
|---|---|---|
| `hbbs` | `rustdesk/rustdesk-server:1.1.16` | ID/rendezvous server — client discovery, NAT traversal, auth |
| `hbbr` | `rustdesk/rustdesk-server:1.1.16` | Relay server — proxies the session when a direct P2P connection fails |

Pinned to `1.1.16` rather than `:latest` — same reasoning as the ticketing system: a moving tag makes a "working" setup silently stop being reproducible.

### Ports

| Port | Protocol | Used by | Purpose |
|---|---|---|---|
| 21115 | TCP | hbbs | NAT type test |
| 21116 | TCP + UDP | hbbs | ID registration, heartbeat, rendezvous |
| 21118 | TCP | hbbs | Web client (websocket) |
| 21117 | TCP | hbbr | Relay |
| 21119 | TCP | hbbr | Web relay (websocket) |

## Connecting a client

1. Install the RustDesk client (e.g. `brew install --cask rustdesk` on macOS, or from [rustdesk.com](https://rustdesk.com/)).
2. In the client's network settings, set:
   - **ID server:** `<this Mac's IP or localhost>:21116`
   - **Relay server:** `<this Mac's IP or localhost>:21117`
   - **Key:** the public key from `docker compose logs hbbs | grep Key:`
3. Repeat on a second machine (or a second client install) to actually test a remote session end-to-end.

> Status: server is up and reachable at `localhost` — client-to-client testing against a *simulated end user* is blocked on the [end-user client VM](../README.md#build-log), which isn't built yet. In the meantime this can be validated with two client installs on real machines.

## Why this matters for a helpdesk role

Ticketing gets an issue tracked; remote support tooling is how you actually *fix* it without physical access to the machine — the core workflow for most Tier 1/2 helpdesk work once phone/chat triage is done.
