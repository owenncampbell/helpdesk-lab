# Ticket Taxonomy: Departments & Help Topics

How tickets are categorized and routed in this osTicket instance, designed to mirror the escalation paths already defined in the [runbooks](../runbooks/).

## Departments

| Department | Role |
|---|---|
| IT Support 1 | Default queue for all incoming tickets — first triage happens here |
| Network Team | Escalation target when a runbook's criteria point to infrastructure (switch/AP/WAN) rather than an end device |
| Security / SOC | Escalation target when a runbook's criteria suggest malicious activity (brute force, malware, unauthorized access) |
| Hardware / Procurement | Escalation target when the resolution is "replace/upgrade hardware," not a software fix |

## Help Topics

| Help Topic | Default Department | Priority | Runbook |
|---|---|---|---|
| Password Reset / Account Lockout | IT Support 1 | Normal | [password-reset-account-lockout.md](../runbooks/password-reset-account-lockout.md) |
| Printer Not Working | IT Support 1 | Normal | [printer-troubleshooting.md](../runbooks/printer-troubleshooting.md) |
| Slow Computer | IT Support 1 | Low | [slow-computer.md](../runbooks/slow-computer.md) |
| Network / Wi-Fi Connectivity | IT Support 1 | High | [network-connectivity.md](../runbooks/network-connectivity.md) |

Network connectivity is set to **High** priority since a user with no network access typically can't work at all, unlike the other categories.

## Why route everything through Tier 1 first

Each runbook's own "Escalation criteria" section is the actual decision point for whether a ticket moves to Network, Security, or Hardware — not the initial category. A "Slow Computer" ticket might turn out to be malware (→ Security) or aging hardware (→ Hardware/Procurement), but you don't know that until Tier 1 triage runs the runbook's diagnostic steps. Routing straight to a specialist queue based only on the initial topic would skip that triage step.
