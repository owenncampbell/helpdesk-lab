# Runbook: Network / Wi-Fi Connectivity

## Triage questions

- Wired or wireless?
- Is it affecting one device or multiple devices in the same location?
- Any recent change (moved desks, new router/AP, ISP outage reported)?

## Diagnostic steps

1. Confirm basic connectivity: can the device reach anything at all? (`ping 8.8.8.8` — tests raw connectivity without DNS)
2. If that fails, check `ipconfig`/`ifconfig` for a valid IP address. An address like `169.254.x.x` means the device failed to get one from DHCP.
3. If the raw ping works but browsing doesn't, test DNS specifically (`nslookup example.com`) — this narrows it to a DNS problem rather than full connectivity loss.
4. For Wi-Fi specifically: check signal strength, and whether other devices nearby have the same problem (isolates device vs. AP/router issue).

## Resolution

1. **No IP / APIPA address:** release/renew the DHCP lease; if that fails, check the switch port or AP for a DHCP scope issue.
2. **DNS failure only:** try an alternate DNS server (e.g. 1.1.1.1) to confirm; if that fixes it, the issue is with the configured DNS server, not the network path.
3. **Single device affected:** check its network adapter driver, or try a different port/AP to isolate hardware vs. configuration.
4. **Multiple devices affected in one area:** likely an AP/switch/uplink issue — treat as infrastructure, not an individual ticket.

## Escalation criteria

- Escalate to network team if: multiple devices/locations are affected, or the issue traces to switch/router/AP configuration rather than the end device.
- Escalate as a possible outage if: the ISP/WAN link itself appears down (no connectivity even from the router/firewall itself).
