# keepwarm

Free-tier keep-warm pingers. Render's free instances spin down after 15 idle minutes and
take 1–2 minutes to cold-boot a JVM, so the scheduled workflow here pings a DB-free
`/healthz` endpoint to keep the app warm for real visitors.

This is the **backup** pinger — the primary is a Cloudflare Worker cron (`warmer/` in the
app repo). Two independent free schedulers beat one: if either provider has a cron outage,
the other keeps the site fast. This repo is public because GitHub Actions minutes are
unlimited on public repos; the only thing in here is a `curl` to a URL that is public anyway.

Both pingers skip **01:00–02:59 UTC** (≈ 03–05 in Norway) so the app stays inside Render's
750 free instance-hours per month (awake 24/7, a 31-day month needs 744 h).

The workflow also makes an empty commit every ~50 days: GitHub disables scheduled workflows
in repositories with no activity for 60 days, and the commit resets that clock.
