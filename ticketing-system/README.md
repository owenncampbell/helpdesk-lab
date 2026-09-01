# Ticketing System Setup

A real, self-hosted [osTicket](https://osticket.com/) instance running via Docker Compose — not a mockup.

## Run it

```bash
cd ticketing-system
cp .env.example .env   # then edit .env with real passwords — never commit .env
docker compose up -d
```

Wait ~30-60 seconds for the health check to pass, then visit:

- **Customer portal:** http://localhost:8080/
- **Agent/staff login:** http://localhost:8080/scp/

Default admin login (from the [devinsolutions/osticket](https://github.com/devinsolutions/docker-osticket) image):

- Username: `ostadmin`
- Password: `Admin1`

**Change this password immediately after first login** — it's a public default.

> Status: done on this instance — admin password has been changed from the default.

## Stack

| Service | Image | Purpose |
|---|---|---|
| `mysql` | `mysql:5.7` | osTicket's database |
| `osticket` | `devinsolutions/osticket:1.17.5` | osTicket app (nginx + PHP-FPM), pinned version |

Credentials are read from `.env` (gitignored) — see `.env.example` for the required variables.

## Notes from getting this running

The first image I tried (`tiredofit/osticket`) doesn't exist on Docker Hub. The second (`osticket/osticket:latest`) pulls fine but is abandoned since 2020 and throws a PHP parse error on install with current osTicket source — a good reminder to check an image's last-updated date and actual install behavior before trusting a README. `devinsolutions/osticket` is actively maintained with real version tags, which is why it's pinned here instead of using `:latest`.

## What to document next

- [x] Change the default admin password (see above)
- [x] Configure ticket categories/priorities/departments — see [`categories.md`](categories.md)
- [ ] Create example tickets that map to each [runbook](../runbooks/) (e.g. a "printer not working" ticket resolved using the printer runbook)
- [ ] Screenshot the agent dashboard once populated with example tickets

## Why this matters for a helpdesk role

Real helpdesk work isn't just fixing the issue — it's tracking it: correct categorization, clear notes for handoff, and timely status updates. Practicing with an actual ticketing system (not just a troubleshooting list) builds that habit.
