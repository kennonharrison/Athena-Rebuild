# Athena Reconstitution Kit — 2026-07-27

Purpose: rebuild Athena from zero on a clean host, without recreating the failure modes that have taken the gateway down.

Source of truth for this kit: workspace `/home/ubuntu/.openclaw/workspace`, restored 2026-07-27 from `backups/athena-state-2026-06-21/`.

---

## 1. What is in this kit

- `core/` — the 11 workspace root files that define identity, rules, and live strategic state:
  AGENTS.md, SOUL.md, IDENTITY.md, USER.md, TOOLS.md, HEARTBEAT.md, MEMORY.md,
  OPEN-LOOPS.md, RADAR.md, RECOVERY.md, execution-guardrails.md
- `memory/topics/` — 10 topic files loaded on demand (certify-ai, khan-university, norwood,
  ets-architecture, ets-applied-intelligence-studio, nte-insightstack, contacts-ets,
  certify-ai-cc-gtm-context, aladdin-pedagogy-competitive-scan, infra-notes)
- `memory/concept-briefs.md` — concept brief log through #57
- `openclaw.config.redacted.json` — current runtime config, all tokens/keys/secrets replaced with `<REDACTED>`
- `supervisord-openclaw.conf.reference` — process supervision config
- `start-gateway.reference.sh` — boot script, gateway token redacted
- this file

## 2. What is NOT in this kit, and must be re-supplied by hand

These did not survive and cannot be recovered from backup. Reconstitution is incomplete without them.

- **Gateway auth token** — regenerate on the new host.
- **MyClaw provider API key** (`models.providers.myclaw.apiKey`) — redacted out of the config in this kit. Without it there is no model access at all.
- **MyClaw publish env** — the live boot script at `/home/ubuntu/.openclaw/start-gateway.sh` provisions `/etc/myclaw/publish.env` (publish API key, VM id, API URL) from an embedded blob. That script is deliberately **not** included here because the blob is a live credential. `start-gateway.reference.sh` in this kit is the shorter variant without it. On rebuild, the publish.env provisioning step must be re-supplied from the platform side.
- **Telegram bot token + chat id** — not present in any config or backup. Kennon must re-issue via BotFather. Chat id was `8358402175`.
- **Gmail / Google OAuth** — no `credentials.json`, no `token.json`, no `client_secret`, no `googleapis` module. The `gmail/gmail.js` and `gmail/calendar.js` scripts referenced throughout HEARTBEAT.md do not exist on disk anymore and must be rewritten.
- **Google Calendar work import id** — `n37l3eq42bavjgrtfb937sb8fkt2bg8s@import.calendar.google.com` (recorded here so it survives).
- **SearXNG config** — instance exists at `/home/ubuntu/searxng` but is not currently serving on 8888; `etc/limiter.toml` missing.
- **Browser binary** — no Chrome/Chromium/Brave/Edge installed. Browser tooling is unavailable until one is.
- **Cron jobs** — all five are gone from the scheduler. Re-create per section 5.
- **Startup-failure monitor script** — `/home/ubuntu/.openclaw/startup-failure-monitor.sh` is absent. Rebuild per section 4.
- **Daily memory logs after 2026-06-21** — the workspace has none. The gap 2026-06-22 → 2026-07-27 is unrecorded.
- **exports/ and reference/** — ~98 strategic artifacts live in `backups/athena-state-2026-06-21/{exports,reference}/`. Too large for email; copy the backup directory separately.

## 3. Rebuild order

Do these in order. The order is the point — it is what prevents the crash loop.

1. Install OpenClaw. **Pin to the version the config was written for.** As of this kit that is `2026.5.6`. Do not take the latest.
2. Create the workspace directory, drop `core/` files in the root and `memory/topics/` under `memory/`.
3. Write `openclaw.json` from `openclaw.config.redacted.json`, substituting a fresh gateway token. Leave channels out for now.
4. **Set `startretries` to 5, not 2147483647.** See section 4.
5. Install the startup-failure monitor before first boot. See section 4.
6. Boot the gateway once. Let it fully settle. Confirm `openclaw status` is clean.
7. Add config back one key at a time via `config.patch` (hot-reloads, no restart).
8. Protected paths last, alone, with a single settled restart: `agents.defaults.heartbeat`, `agents.defaults.bootstrapMaxChars`, `discovery.mdns.mode`.
9. Re-supply credentials from section 2.
10. Re-create cron, weekly doctor first. See section 5.

## 4. The failure modes to design against

Documented in `memory/topics/infra-notes.md`. Three of them.

### 4a. Schema rejection plus infinite retry = multi-hour outage

The 2026-05-07 outage ran 02:58 UTC → 2026-05-08 00:25 UTC, about 21.5 hours. OpenClaw upgraded to 2026.5.6, the config carried `channels.telegram.dmPolicy` at a path the new stricter schema rejected as `additionalProperties`, and supervisord crash-looped every ~60s with `startretries=2147483647`. Nothing alerted, because the Telegram bot was down the whole time. Fixed by `openclaw doctor --fix`.

Mitigations, all three:
- `startretries=5`. A loop that stops is diagnosable. A loop that never stops burns a day.
- Run `openclaw doctor` before any upgrade, and pin versions.
- Keep an alerting path that does not depend on the gateway being alive.

### 4b. Restart storms stall the event loop

Three gateway restarts in four minutes reliably produces 10-20s event-loop delays, which surface as "incomplete terminal response" and `payloads=0`. Each restart stages `chokidar` via pnpm install, blocking 1.4-9.7s.

Rule: prefer `config.patch` over `restart`. Never stack restarts. Let one fully settle.

### 4c. Misdiagnosis rule

"Incomplete terminal response" plus `event_loop_delay` warnings is a **host/gateway problem, not a model problem.** Do not swap the model or the fallback chain. Investigate restart storms, Bonjour/mDNS churn, memory pressure.

Corollary: Bonjour loops `probing → advertised → restarting` every 10s forever in a container with no mDNS consumers. Cosmetic, small steady CPU. Disable via `discovery.mdns.mode: "off"` or `OPENCLAW_DISABLE_BONJOUR=1`. Both are protected paths — direct file edit plus restart, not `config.patch`.

### 4d. Startup-failure monitor spec

Rebuild to this spec. It is the only thing with visibility when the gateway is dead.

- Script: `/home/ubuntu/.openclaw/startup-failure-monitor.sh`
- Supervisord program: `openclaw-startup-monitor`, runs as user `ubuntu`
- Watches `/home/ubuntu/.openclaw/logs/stability/` for `gateway.startup_failed` bundles
- Threshold: 3 failures in 300s triggers an alert; 1h cooldown between alerts
- Delivery: direct Telegram Bot API call, reading token and chat id straight from `openclaw.json`. Deliberately bypasses OpenClaw so it works when the gateway is down.
- Log: `/home/ubuntu/.openclaw/startup-failure-monitor.log`

Note the dependency: this monitor cannot alert until the Telegram token is restored. Until then there is no out-of-band alerting at all.

## 5. Cron set to re-create

All five are currently absent from the scheduler.

- **Weekly doctor** — Mon 10:00 ET. Catches config drift before an upgrade forces a crash. Re-create this one first.
- **Morning briefing** — 11:00 UTC daily (7 AM ET), Telegram direct. Full procedure is in `core/HEARTBEAT.md`. Depends on Gmail + Calendar + web search.
- **Concept brief** — every 48h. Last delivered was #57 (Embeddings & Vector Search, 2026-06-21 00:38 UTC). Queue candidates in `memory/concept-briefs.md`.
- **Quarterly distill** — 11:30 UTC on the first of each quarter-month. Reviews MEMORY.md plus topics, archives stale items.
- **Weekly Athena backup** — Sun 03:00 UTC. Writes `backups/athena-state-YYYY-MM-DD/` with core/, memory/, exports/, reference/, README.md, STATE-SUMMARY.md. Retention: 8 most recent.

Old job ids are recorded in MEMORY.md for reference but will not be reused; new jobs get new ids.

- **Heartbeat** (not cron, agent config): `every = "30m"`, model `myclaw/claude-haiku-4.5`, active hours 08:00-23:00 America/New_York, `lightContext` + `skipWhenBusy` on. Protected path — direct JSON edit, hot-reload by touching the config. `config.patch` rejects all heartbeat keys.

## 6. Known-open security posture

Flagging rather than silently carrying forward. Current config has:

- `gateway.controlUi.allowInsecureAuth: true`
- `gateway.controlUi.dangerouslyDisableDeviceAuth: true`
- `gateway.bind: "lan"` with `trustedProxies: ["0.0.0.0/0"]`

That is an unauthenticated control surface reachable on the LAN. It is not the cause of the connection breaks, but a from-scratch rebuild is the natural moment to fix it. Recommend `bind: "loopback"` unless remote access is actually needed, and dropping both dangerous flags.

## 7. Recovery phrases

From `core/RECOVERY.md`. Kennon says one of these; Athena executes.

- `Re-anchor to Athena backup.` — treat current state as possibly drifted, read the backup snapshot, resume from that baseline, preserve clearly user-approved later work.
- `Hard re-anchor to Athena backup.` — same, but assume substantial drift, prioritize backup over recent inferred context, ask as little as possible.

RECOVERY.md still names `backups/athena-state-2026-05-04/` as the authoritative snapshot. That is stale and should be updated to the newest backup on rebuild — currently `backups/athena-state-2026-06-21/`.

## 8. Open loops carried in, highest urgency first

Full detail in `core/OPEN-LOOPS.md` and `core/RADAR.md`. Both were stale at the 2026-06-21 backup (OPEN-LOOPS three cycles, RADAR one) and the 2026-06-22 → 07-27 gap is unrecorded, so treat all of these as needing status confirmation before acting.

- **BofA $27,000 check #114 on acct 0191** — flagged unusual 6/18, still unconfirmed 6/20. Confirm or fraud-report. Now five weeks stale.
- **Certify.ai** — term sheet v2 ($8M / $40M pre / $48M post, Delaware PBC, target close end of July 2026). That target date is now. Six pre-circulation gates open.
- **Gmail OAuth in Testing mode** — 7-day refresh token expiry, root cause of repeated breakage. Fix is publishing to Production, free. Kennon agreed 2026-03-22, never done. Now moot until OAuth is rebuilt entirely.
- **Steven Bridges (WFSGD)** — substantive reply owed since 5/29-30. Structural answer is ready.
- **Eric Bendfeldt (VT Extension)** — 32+ days silent at backup time. Norwood regen-ag grant strategy.
- **Sal Khan** — 5/18 call outcome carry, 34+ days at backup.
- **Adams Sutphin** — Norwood bathroom plans decision pending (6/17).
- **Web search degraded 10+ consecutive briefings** — SearXNG base URL unconfigured, Kimi API key missing.

---

_Dum spiro spero._
