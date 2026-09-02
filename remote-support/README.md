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

## Known issue: Colima blocks UDP registration to hbbs

Pointing a client's ID/Relay server at this Mac's `localhost` (or LAN IP) does **not** work as-is. Diagnosis:

- `docker compose ps` and `lsof -i :21116` etc. show the ports are published and something is listening.
- But `hbbs` never logs an incoming registration, even for a raw UDP packet sent straight at the port from the host.
- `lsof` shows the published ports are actually held open by an `ssh` process — Colima (in this default config, no VM IP from `colima list`) forwards Docker's published ports to `localhost` via an SSH tunnel, and plain SSH `-L` forwarding is **TCP-only**.
- `hbbs`'s ID registration/heartbeat protocol needs **UDP** on 21116 (see the ports table above) — it silently never reaches the container, so the client sits at "Not ready. Please check your connection" indefinitely.

This is a Colima networking limitation, not a RustDesk or docker-compose misconfiguration — the fix (`socket_vmnet` + `colima start --network-address`, to give the VM a real routable IP with full UDP support) is a bigger, invasive change to shared infra (needs sudo, briefly restarts the already-running osTicket containers). Given this lab is scoped to demonstrating helpdesk skills rather than hypervisor networking, that fix was set aside in favor of validating remote support the way a Tier 1 tech would actually use it day-to-day: direct peer-to-peer.

## Connecting a client — validated via Direct IP Access

Since the self-hosted ID/relay server can't be reached over UDP under the current Colima setup, connectivity was validated using RustDesk's **Direct IP Access** instead — a pure-TCP, peer-to-peer mode that bypasses `hbbs`/`hbbr` entirely.

1. On the "tech" machine (this Mac): RustDesk → Settings → Security → set a permanent password, then enable **Direct IP Access**.
2. On a second machine, standing in for the end user (in this case, a second physical machine running Ubuntu — no Windows client VM exists yet): install RustDesk and connect to `<tech machine's LAN IP>:21118` using that permanent password.
3. ✅ Confirmed working: connected from the Ubuntu machine to this Mac (`192.168.0.148:21118`) over the LAN.

> Status: self-hosted `hbbs`/`hbbr` server is up and running (documented above as a real, if currently UDP-blocked, deployment); remote support itself — the actual helpdesk-relevant skill — is validated end-to-end via Direct IP Access between two real machines.

## Why this matters for a helpdesk role

Ticketing gets an issue tracked; remote support tooling is how you actually *fix* it without physical access to the machine — the core workflow for most Tier 1/2 helpdesk work once phone/chat triage is done.
