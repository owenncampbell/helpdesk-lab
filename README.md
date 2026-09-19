# Helpdesk Lab

> Part of my [IT & Cybersecurity Portfolio](https://github.com/owenncampbell/it-cybersecurity-portfolio)

A simulated helpdesk environment built to practice and demonstrate the core IT support workflow of taking a ticket, triaging it, working it to resolution, and documenting it clearly. This project includes running infrastructure (Docker containers, a ticketing system, and a self-hosted remote-support server).

![Closed ticket queue in osTicket](ticketing-system/screenshots/closed-tickets-queue.png)

## What this demonstrates

- **Ticket triage & categorization** — 4 departments and 4 help topics, each mapped to a specific escalation path. See [`ticketing-system/categories.md`](ticketing-system/categories.md).
- **Troubleshooting methodology** — 4 runbooks covering the most common Tier 1 ticket types, each following the same plan: triage questions → diagnostic steps → resolution → escalation criteria. See [`runbooks/`](runbooks/).
- **Real ticket handling** — 4 worked example tickets with internal triage notes and customer-facing replies, one left open intentionally to show a realistic in-progress queue. See [`ticketing-system/example-tickets.md`](ticketing-system/example-tickets.md).
- **Remote support tooling** — a self-hosted RustDesk server, with a peer-to-peer connection validated end-to-end between two physical machines. See [`remote-support/`](remote-support/).

## Target architecture

```mermaid
flowchart TB
    USER[End User<br/>Windows client VM]
    TICKET[Ticketing System<br/>osTicket, self-hosted via Docker]
    TECH[Helpdesk Tech Workstation<br/>Remote support tooling]
    AD[Active Directory<br/>user/group management]

    USER -- submits ticket --> TICKET
    TICKET -- assigned to --> TECH
    TECH -- remote session --> USER
    TECH -- resets password / unlocks account --> AD
```

This is the target design end-to-end. What's actually built and running today is the ticketing system and the remote-support server (see the build log below) — the Windows client VM and AD integration are still ahead.

## Why this design

- **Ticketing system** gives real experience with intake, categorization, prioritization, and closing tickets — the core workflow of any helpdesk role.
- **Troubleshooting runbooks** turn general IT support knowledge into a repeatable process, then get refined against real worked tickets.
- **Remote support tooling** mirrors how real helpdesk techs assist users without physical access to the machine.
- **AD integration** (once the home lab it depends on exists) adds real account/group management tasks — password resets, lockouts, permission issues — against a live directory instead of just a documented process.

## Build log

| Date | Component | Status | Notes |
|---|---|---|---|
| 2026-08-31 | Ticketing system install (Docker) | ✅ Running | Real osTicket instance via Docker Compose — see [`ticketing-system/`](ticketing-system/) |
| 2026-09-01 | Remote support tool setup (Docker) | ✅ Running | Self-hosted RustDesk server (hbbs/hbbr) via Docker Compose — see [`remote-support/`](remote-support/) |
| 2026-09-02 | Remote support connectivity validated | ✅ Validated | Self-hosted server's UDP registration is blocked by a Colima networking limitation (documented); validated remote support the way it's actually used — RustDesk Direct IP Access, tested end-to-end between two real machines — see [`remote-support/`](remote-support/) |
| _TBD_ | End-user client VM | Not started | Needs a Windows license/ISO — my own work to do |
| _TBD_ | AD integration | Not started | Depends on home lab existing |

## Contents

- [`ticketing-system/`](ticketing-system/) — setup notes for the self-hosted ticketing system, ticket categorization, and worked example tickets
- [`remote-support/`](remote-support/) — setup notes for the self-hosted RustDesk server, including a real networking issue hit and worked around
- [`runbooks/`](runbooks/) — troubleshooting runbooks for common ticket types

## Tools

- Ticketing: [osTicket](https://osticket.com/), self-hosted (running — see [`ticketing-system/`](ticketing-system/))
- Remote support: [RustDesk](https://rustdesk.com/), self-hosted (running — see [`remote-support/`](remote-support/))
- Client OS: Windows 10/11 VM (not started — see build log)
