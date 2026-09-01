# Runbook: Password Reset / Account Lockout

## Triage questions

- Is the account locked out, or has the user simply forgotten their password?
- How many failed attempts occurred, and over what time period? (repeated rapid failures could indicate a brute-force attempt, not user error — see the security note below)
- Is this a local machine account, a domain account, or a cloud/SaaS account (e.g. Microsoft 365)?

## Diagnostic steps

1. Confirm the user's identity per your organization's verification policy before making any change.
2. Check Active Directory (or the relevant identity system) for the account's lockout status and last bad password timestamp.
3. Check whether the lockout is tied to a specific device (e.g. a saved credential on a phone or mapped drive retrying with an old password) — this is one of the most common repeat-lockout causes.

## Resolution

1. Unlock the account (`Unlock-ADAccount` in AD, or the equivalent admin console action).
2. Reset the password if requested, following your org's complexity policy, and set "user must change password at next logon."
3. If the cause was a stored credential on another device, walk the user through updating it there too — otherwise the account will lock again shortly.

## Escalation criteria

- Escalate to security/SOC if: failed attempts came from an unfamiliar location/IP, or the pattern matches automated brute-force behavior rather than user error.
- Escalate to a senior admin if: the account shows signs of prior unauthorized access (unrecognized sent mail, unfamiliar sign-in locations, changed recovery info).
