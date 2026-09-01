# Ticketing System Setup

Plan for standing up a self-hosted ticketing system to practice a real helpdesk queue.

## Option A: osTicket

```bash
# Example using Docker (fill in with actual steps once run)
docker run -d --name osticket -p 80:80 \
  -e MYSQL_HOST=db -e MYSQL_USER=osticket -e MYSQL_PASSWORD=changeme \
  tiredofit/osticket
```

## Option B: Zammad

```bash
# Zammad publishes an official docker-compose setup
git clone https://github.com/zammad/zammad-docker-compose.git
cd zammad-docker-compose
docker compose up -d
```

## What to document once set up

- [ ] Screenshot of the admin/agent dashboard
- [ ] Ticket categories/priorities configured
- [ ] Example tickets created (linked to the [runbooks](../runbooks/) that resolve them)
- [ ] SLA or response-time settings (if configured)

## Why this matters for a helpdesk role

Real helpdesk work isn't just fixing the issue — it's tracking it: correct categorization, clear notes for handoff, and timely status updates. Practicing with an actual ticketing system (not just a troubleshooting list) builds that habit.
