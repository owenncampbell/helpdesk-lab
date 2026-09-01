# Runbook: Printer Not Working

## Triage questions

- Is the issue affecting one user or everyone on that printer?
- Is it a network printer or a locally connected (USB) printer?
- What's the exact symptom — not printing at all, printing garbled output, stuck queue, error message on the printer's display?

## Diagnostic steps

1. Check the printer itself: powered on, no paper jam/low toner alerts, connected to the network (if applicable).
2. On the user's machine, check the print queue — is the job stuck? Are there old failed jobs clogging it?
3. Ping the printer's IP (network printers) to confirm it's reachable from the user's machine/subnet.
4. If only one user is affected: check whether their printer driver is missing/outdated, or the wrong printer is set as default.

## Resolution

1. **Stuck queue:** clear the print spooler (stop the Print Spooler service, delete stuck jobs from the spool folder, restart the service).
2. **Driver issue:** reinstall/update the driver, or re-add the printer via its network address.
3. **Network printer unreachable:** confirm printer's IP hasn't changed (DHCP reassignment is a common cause) and update the connection if needed.
4. **Affects everyone:** treat as a printer/server-side issue, not a per-user one — check the print server or the printer's own network settings first.

## Escalation criteria

- Escalate to network team if: the printer is unreachable network-wide and isn't a DHCP/IP issue (could be a switch/VLAN problem).
- Escalate to a vendor/hardware ticket if: the printer itself reports a hardware fault (fuser, drum, etc.).
