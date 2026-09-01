# Runbook: Slow Computer

## Triage questions

- Is it slow all the time, or only during specific tasks (startup, opening an app, browsing)?
- Did this start recently, or has it always been this way? If recent — what changed (update, new software, more browser tabs than usual)?
- Is it one machine or multiple machines reporting the same thing (could point to a network/server-side cause instead)?

## Diagnostic steps

1. Open Task Manager (or `top`/`htop` on Linux) and check CPU, memory, and disk usage. Identify what's consuming resources.
2. Check startup programs — too many background apps launching at boot is one of the most common causes of a "slow computer" complaint.
3. Check available disk space — a nearly-full disk (especially the OS drive) causes significant slowdowns.
4. Check for pending/stuck Windows updates or a backup job running silently in the background.
5. Run a malware/antivirus scan if usage patterns look abnormal (unfamiliar process names, sustained high CPU/network with no obvious cause).

## Resolution

1. Disable unnecessary startup programs.
2. Free up disk space (temp files, old downloads, empty recycle bin) if disk is near capacity.
3. Add RAM or recommend a hardware upgrade if the machine consistently maxes out memory under normal use — this is a common ticket outcome, not a failure to fix it in software.
4. If malware is found, follow your org's incident response process rather than just removing it and closing the ticket.

## Escalation criteria

- Escalate to security if malware/unusual persistent processes are found.
- Escalate to hardware/procurement if the root cause is aging hardware that can no longer be reasonably fixed in software.
