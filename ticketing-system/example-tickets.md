# Example Tickets

Four synthetic tickets (fake requesters at `@example.com`, an address reserved for documentation use), one per help topic, each worked the way a real agent would: an internal triage note, then a reply to the requester, then a status change.

| Ticket | Requester | Topic | Priority | Resolution | Status |
|---|---|---|---|---|---|
| #396919 | Alex Rivera | Password Reset / Account Lockout | Normal | Verified identity, checked lockout was from repeated failed logins with no attack pattern, unlocked account and reset password | Resolved |
| #847565 | Jordan Lee | Printer Not Working | Normal | Found a stuck job blocking the print queue, cleared the spooler and restarted the print service | Resolved |
| #366040 | Taylor Brooks | Network / Wi-Fi Connectivity | High | Device had a valid IP but failing DNS resolution; switched DNS server, confirmed fix, scoped to single device | Resolved |
| #378940 | Sam Patel | Slow Computer | Low | Found high disk usage from a stuck Windows Update plus a nearly-full disk; freed space and is monitoring before replying — **left open intentionally** | Open |

## Why one ticket is still open

A real queue always has something in progress. The Slow Computer ticket represents a partial triage: the internal note documents what was checked and what was done so far, without a customer-facing reply promising a fix that hasn't been confirmed yet. If performance doesn't improve, the [slow-computer runbook](../runbooks/slow-computer.md)'s escalation criteria point to Hardware/Procurement next.

## Mapping to departments

All four were created directly in the IT Support 1 department (the default for every help topic — see [`categories.md`](categories.md)). None needed escalation to Network Team, Security/SOC, or Hardware/Procurement in this pass, since each resolved at Tier 1 during initial triage — which is exactly what those departments are *for*, not something every ticket is expected to hit.
