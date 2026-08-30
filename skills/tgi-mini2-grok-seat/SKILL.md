---
name: tgi-mini2-grok-seat
description: >
  Mini2 owns Grok OIDC for coding@/agents@. Mini is hop only. 403 is a rate tier, not auth.
owner_seat: shared
approval_boundary: Never Mini grok login for those two accounts
---

# reference-grok-coding-oidc-is-mini2-owned.md

---
name: reference-grok-coding-oidc-is-mini2-owned
description: "Grok pool is THREE accounts — tony@, fallover@, agents@. coding@ SuperGrok was cancelled by Tony 2026-08-29 and is stripped everywhere. agents@ OIDC lives on mini2. A Grok 403 is never auth: read the seat rate tier."
metadata:
  node_type: memory
  type: reference
---

`ssh mini2 cat ~/browser-box/GROK-OIDC-LAW.md` is the authority (2026-08-07). It **supersedes**
the `~/.grok-acc2 = coding@` line in [[reference-grok-pool-flip-order]] and in `xai-oauth-sync`'s
"LOCKED v6 2026-07-31" header:

- coding@ RT = **mini2 `~/browser-box/coding-oidc/auth.json`**; agents@ RT = **mini2
  `~/browser-box/agents-oidc/auth.json`** (identity stored as userId, not an email)
- Mini is **hop only** — `~/bin/coding-oidc-ecp-hop` (LaunchAgent 1800 s) → ECP
  `/opt/forge-term/.grok/auth-coding.json`. **Never Mini `grok login` for these two.**
- Re-mint on mini2 with `GROK_HOME=$HOME/browser-box/<acct>-oidc`, approve on that account's
  own CDP seat (coding :9231, agents :9233), then run the hop.

## A 403 is never auth. Read the tier.
`~/browser-box/grok-usage-pull.cjs <port>` (run it with mini2's node at
`/Users/anthonymini2/opt/node/bin/node`) returns the account's real state:

- **`rates.fast.totalQueries` = 30** → free tier. **The SuperGrok subscription is GONE.** Tony's
  call, nobody else buys ([[feedback-no-agent-authorizes-spend]]).
- **`rates.fast.totalQueries` = 400** → SuperGrok active. If it still 403s, check
  `credits.creditUsagePercent`: **100 % = weekly window spent, resets by itself** at
  `credits.currentPeriod.end`. Never page on this.

## coding@ is GONE (Tony cancelled it 2026-08-29)
The pool is **three** accounts now: tony@, fallover@, agents@. coding@ was stripped from every
list — do not re-add it without Tony resubscribing first. Stripped in: Mini
`xai-auth-keeper-probe.py` (POOLS/REMOTE_SRC/SEAT_PORT/cmds/ECP file sweep), `gusage.py`
(GROK_ACCOUNTS), `grok-home-invariant`, `xai-oauth-sync` (coding block now a one-line RETIRED log);
LaunchAgents `com.tgi.coding-oidc-ecp-hop` (Mini) and `com.tgi.coding-oidc-ecp-sync` (mini2) both
booted out and renamed `.OFF-grok-cancelled`; mini2 seat **:9231 stopped** and removed from
`chrome-grok-seats-watchdog.sh`; fleet box `xai-token-health`, `xai-token-sync`,
`run-agent-inner.sh`, `forge-grok-pool-audit` POOLS, `/opt/bin/forge-pool-pin` (no longer accepts
`coding`); deleted `auth-coding.json`, `xai-oauth-coding.json`, `xai-limit-coding`.
After: keeper board 3/3 `reason=ok`, `grok-home-invariant` exit 0, pool audit `status=PASS`,
`xai-token-health healthy=2/3` (3/3 once agents@ resets).

## Fixes made 2026-08-29 (all backed up `.bak-*`; none are repo-tracked or mirrored)
- `~/bin/xai-auth-keeper-probe.py` — `REMOTE_SRC` probes the mini2 homes for coding@/agents@
  instead of the retired Mini paths; a spend 403 is classified via `seat_tier()` into
  `NO SUBSCRIPTION` (pages) vs weekly-spent (logs only); pre-alert `PRE` 43200→**7200** because a
  12 h warning on a 6 h token fired on every pool every sweep — 100 % false, and most of the
  `AUTH_KEEPER_NEEDS_TONY.txt` backlog.
- `mini2 ~/browser-box/chrome-grok-seats-watchdog.sh` — the blanket `accounts.x.ai/*` steering
  exemption never released, so a seat that finished a device flow sat on
  `/oauth2/device/done` forever and reported no usage at all. `device/done` is now steered home;
  logins still exempt. This is why agents@ read blank from Aug 25.
- `~/bin/grok-home-invariant` — `.grok-acc2` / `.grok-agents` are `RETIRED`, reported as NOTE not
  FAIL (it exited 1 for three days over a stale fallover@ token nothing reads).
- `~/.grok/scripts/gusage.py` — `home_retired` on coding@/agents@; identity comes from the seat
  session, killing the standing `HOME MISMATCH coding@` on the deck quota chip.

`xai-oauth-sync` was left alone — its OIDC-home header is Tony-locked
([[feedback-never-touch-frozen-files]]), so it still logs a benign `CODING SKIP` /
`AGENTS SKIP`. Its `fides oauth push` leg fails separately on
`PermissionError: /var/lib/ecp/xai-oauth-tony.json`.

See [[reference-usage-quota-board]], [[reference-grok-pool-flip-order]].

---

# feedback-agents-oidc-login-on-mini2.md

---
name: feedback-agents-oidc-login-on-mini2
description: "Tony 2026-08-25 — run the agents@ Grok OIDC login on mini2 (the warm browser box) only (\"NOT on the fucking mini\"); Mini grok homes are tony@/coding@/fallover@ only."
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 83003703-c361-4c21-8ceb-51d528138747
  modified: 2026-08-28T17:26:16.957Z
---

I ran `GROK_HOME=~/.grok-agents grok login --device-auth` on the Mini to restore the agents@ pool. Tony: "NOT on the
fucking mini idiot." The sync header already said it: `agents@ ~/.grok-agents (ultimate spill; OIDC login on mini2)`.

**Why:** the Mini's grok homes are Tony's working Build tabs (tony@/coding@) plus fallover@; an agents@ session on the
Mini crosses homes and feeds the dual-refresh trap. mini2 is where agents@'s browser session and login live.

**How to apply:** agents@ token work = `ssh mini2` (its grok home + keeper), or the warm mini2 browser with Tony's OK.
Every agents@ device-auth flow starts on mini2. Related: [[reference-xai-pool-403-bodies-20260825]],
[[feedback-browser-mini2-warm-approval]].

---

# feedback-browser-mini2-warm-approval.md

---
name: feedback-browser-mini2-warm-approval
description: "HARD: Vultr is the primary browser fleet; mini2 is hard-lane only (GoDaddy/Xero/Grok) and runs only after Tony's explicit approval"
metadata: 
  node_type: memory
  type: feedback
  date: 2026-07-30
  originSessionId: 83003703-c361-4c21-8ceb-51d528138747
  modified: 2026-08-28T17:28:19.890Z
---

# Browser lanes (binding — Tony 2026-07-30)

## Fleet priority

| Priority | Where | When |
|----------|--------|------|
| **1 Primary** | **Vultr/BRAIN Chrome fleet** (10 local CDP, pool `:3201`, mcp `:3205`) | **All routine** agent browser work |
| **2 Hard / overflow** | **mini2** residential CDP `:19222` | GoDaddy / Xero / Grok / xAI / DataDome **only**, after **Tony explicit yes**. Also if Vultr full/down. |
| **3 Last failover** | **MacBook** CDP `:19223` | Only if Vultr + mini2 unavailable |
| **Never** | atmini GUI, local Playwright/Chrome on Mini, BrowserBase | — |

**Default browser machine = Vultr.** mini2 is scarce residential trust, reserved for the hard lane.

Live pool (BRAIN `start.sh`):
- `CDP_ENDPOINTS` = 10 local Vultr Chromes (9224–9232 + 9235)
- `CDP_FAILOVER_ENDPOINTS` = mini2 then MacBook reverse tunnels
- `CDP_HARD_ENDPOINTS` = residential pair
- `CDP_HARD_SERVICES` = godaddy, xero, grok, xai, datadome

## Warm sessions on mini2 (protect)

- GoDaddy (DataDome)
- Xero
- Grok / xAI

Keep these logged in and focused work planned — log out, parallel-drive, or take focus only with a plan.

## Approval for mini2 (HARD)

Before any agent drives mini2 browser:

1. Ask Tony in this conversation (site, action, why).
2. Wait for explicit yes.
3. This gate holds even under always-approve / bypass-permissions for hard services.

## Remote control of mini2

Prefer **Parsec** host on mini2 for human mouse/keyboard. Screen Sharing virtual display causes focus/mouse hell. Automation still uses CDP, not Parsec.

## Related

- reference-browser-lane-mini2-cdp
- feedback_never_use_mini_browser
- feedback-never-drive-atmini-gui

---

# reference-browser-lane-mini2-cdp.md

---
name: reference-browser-lane-mini2-cdp
description: "Browser automation = mini2 warm residential Chrome (CDP 9222). GoDaddy/Xero/Grok warm there. Tony approval REQUIRED before any agent uses the browser. BrowserBase cancelled. MacBook failover only."
metadata: 
  node_type: memory
  type: reference
  originSessionId: 4e2f3f5f-ab3b-455b-b716-0ef62c80b73a
---

**2026-07-30 BINDING — warm sessions + approval guardrail (Tony):**

- **GoDaddy, Xero, and Grok/xAI warm browser = mini2 only** (residential Chrome CDP `:9222`, `adminbot-profile`). Not atmini. Not a local launch. MacBook = failover only.
- **HARD approval:** any agent (Claude / Grok / Forge / ECP) that will drive the mini2 browser for those sites — or for any other mini2 browser use — must **ask Tony first and get an explicit yes in that conversation**. Always-approve / bypass does **not** waive GoDaddy / Xero / Grok.
- Full rule: [[feedback-browser-mini2-warm-approval]].

**2026-07-20: BrowserBase SaaS fully torn down.** All browser automation now runs on **mini2's warm residential Chrome**, not BrowserBase. Proven end-to-end: pool sessions egress from mini2's residential IP **72.222.196.139**.

**Architecture (pool = the gateway; backend swapped underneath every consumer):**
- **mini2** (`ssh mini2`, user anthonymini2, residential Cox IP 72.222.196.139) runs Chrome CDP on `127.0.0.1:9222`, persistent logged-in profile `~/browser-box/adminbot-profile` (owned by pid that also holds SingletonLock; a 2nd personal Chrome runs separately — never touch it). Kept alive by launchd **`com.tgi.chrome-9222-watchdog`** (`~/browser-box/chrome-9222-watchdog.sh`, 30s) — relaunches ONLY if CDP is down, clears stale SingletonLock, never disturbs a live session.
- Reverse tunnel: launchd **`com.tgi.mini2-pool-tunnel`** on mini2 holds `ssh -N -R 127.0.0.1:19222:127.0.0.1:9222 root@144.202.88.43` (KeepAlive), exposing mini2 CDP on the pool box **loopback `:19222`** (loopback = the security boundary for unauthenticated CDP). Key `~/.ssh/id_ed25519_pooltunnel`, authorized (restricted) on the pool box.
- **browser-pool** (`tgi-browser-01` 144.202.88.43, runs from `/opt/browser-pool/dist/` via `bash start.sh`, NOT a git checkout — edit dist + `pm2 restart`). start.sh now sets `BROWSER_BACKEND=cdp`, `CDP_ENDPOINT=http://127.0.0.1:19222`, `BB_MAX_SESSIONS=2`. `createSession` short-circuits to **`createCdpSession`** (added in dist/pool.js): `puppeteer.connect` to mini2, Stagehand via `env:'LOCAL' + localBrowserLaunchOptions:{cdpUrl}` for act/extract/observe, and `close()` does `browser.disconnect()` + closes only its own tabs — **NEVER closes mini2's Chrome**.
- Every consumer is transparent: browser MCP `mcp__browser__*` → browser-mcp `:3205` → pool `:3201`; ECP procurement + Forge "BrowserBase pool" tool also hit pool `:3201`. None call BrowserBase directly.
- **MacBook** (`ssh macbook`, Chrome CDP :9222) = FAILOVER only (would tunnel to loopback `:19223`). ONE serial browser lane by design — never parallel-scale (see [[no-agent-bursts]]).
- Pool `lane:'local'` = the pool box's own Linux Chrome (Azure-B2C portals); per-box Chrome (rh/render model) handles box-local jobs.

**BrowserBase is CANCELLED** — tony@tiedemannglobe.com account, Startup ($137/mo) → Free ($0/mo) effective 2026-08-10, next payment $0.00, NO prepaid credits existed. Infisical `ecp/prod` keys `BROWSERBASE_API_KEY` + `BROWSERBASE_PROJECT_ID` DELETED; removed from pool start.sh. Never propose reconnecting/paying for BrowserBase. (VW has two items: `BrowserBase`=admin-bot@ stale, `Browserbase`=tony@ real login id `47a65a8e-eed7-49f8-8d19-c0963a8e1096`.)

**AI CAPTCHA solving** (replaces BrowserBase's built-in `solveCaptchas`): the pool has `POST /session/solve-captcha` `{sessionId, selector, hint?}` → grabs the captcha `<img>` → vision model → text; hard $2/UTC-day cap; `GET /captcha-budget`. **Sovereign: GMI Cloud, NOT OpenRouter** (Tony corrected 2026-07-21 — OpenRouter is banned). **LIVE + proven 2026-07-21** (read a `K7QF9P` image exactly on GMI after Tony topped up the account). Serverless vision model = `google/gemma-4-31b-it` (qwen3-vl-235b is GMI catalog-only/dedicated, not serverless; GPT-4o/MiniMax-M2.5/Nemotron-3-ultra are confirmed-vision fallbacks via `CAPTCHA_VLM_MODEL`). gemma-4 nails clean text; if heavy distortion needs more muscle, swap the env to a bigger vision model. Details in [[reference-humanless-captcha-qwen3vl]].

**Smart verbs (act/extract/observe) — FIXED 2026-07-21** (were "No action found" over the CDP attach). THREE root causes, all fixed + on main:
1. **Wrong-tab targeting:** Stagehand `env:'LOCAL'+cdpUrl` attaches to mini2's whole Chrome and its default active page = newest target (`activePage()` in `understudy/context.js`), NOT the pool's puppeteer tab (`tracked[0]`) — so it observed a random admin-bot tab (huge DOM → "capped oversized DOM to 48000" was the tell). Fix: `createCdpSession` pageProxy now passes the live puppeteer page as `options.page` to every act/extract/observe (`s.act(i, { ...opts, page: tracked[0] })`). Stagehand's `resolvePage`→`normalizeToV3Page` has an `isPuppeteerPage` branch that resolves it to the matching v3Page by main-frame id. observe/extract worked immediately after this.
2. **act(string) internal-observe flaky:** standalone observe was more reliable than act's own internal observe. Fix: `/session/act` now does observe→act — `observe(instruction)` then `act(observeResult[0])` (deterministic ObserveResult branch), not `act(string)`.
3. **DECISIVE — the Stagehand model.** The observe/act LLM was `kimi-k2.7-code` (Fireworks) — a CODING model that grounds the a11y tree poorly (~2/4 observe in-bench, empties worse under load). Fix: `buildLlmClient` provider → GMI Cloud (`api.gmi-serving.com`, `GMI_CLOUD_API_KEY`) + model `google/gemini-3.5-flash` (via `STAGEHAND_MODEL_NAME` Infisical ecp/prod; PR #22 → main `b20afd2`). Gemini grounds Stagehand reliably: **5/5 observe bench, act 3/3 live**. Fireworks `glm-5p2` stays the 429/5xx failover. **Lesson: use Gemini (or a strong instruct/vision model) for Stagehand observe — code models (kimi-code/glm) fail grounding** (matches [[reference_browser_pool_stagehand]]'s "Gemini reliable, Qwen-class not").
Proven E2E: `act("click the More information link")` on example.com → iana.org, repeatably. Deterministic verbs remain 100%. All changes on `main` (repo=box parity, PRs #21+#22). Related: [[reference_browser_pool_stagehand]].

**REPO RECONCILED 2026-07-21 (durability fixed):** `/opt/browser-pool` runs from on-box `dist/` (NOT a git checkout); the `e2p-ai/browser-pool` repo had drifted badly — many prior sessions' dist-only edits were never committed. **Now RECONCILED to full box parity and merged to `main`** (PR #21 → squash `a7c98e7`): `tsc` builds clean, pool.js functions + 36 routes match the box, this session's changes preserved. Brought in the prior uncommitted drift too: `withReasoningRetry`+retry consts, `capDomInBody`, the GLM-5.2/GMI+Fireworks `buildLlmClient` rewrite (off OpenRouter), `/session/upload` + `/session/evaluate-frame` + `/session/frame-elements`, host-keyed login-guard + fingerprinting, self-healing credentials, amazon alert-send. Subsumed + closed the two stale PRs #7 (tab/popup) and #20. **A rebuild from `main` now = the proven box code (no regression).** Verified structurally (build + function/route parity), NOT by a live-browser run — so build+smoke-test before any real deploy. Backups: box `/root/browser-pool-backups/browser-pool-run-2026-07-21.tgz` + Mac `~/build/browser-pool-snapshots/`. MacBook failover watchdog+tunnel: `~/browser-box/` + `~/Library/LaunchAgents/com.tgi.chrome-9222-watchdog.plist` + `com.tgi.macbook-pool-tunnel.plist` (user anthonytiedemann).

**2026-07-24 Grok: self-heal + MacBook failover hardened.** Do NOT bare-spawn Chrome from launchd (keychain fails). Use `~/browser-box/start-chrome-cdp.sh` (`open -na "Google Chrome" --args …`) from the gui-domain watchdog `chrome-9222-watchdog.sh` (60s rate limit, 5-fail budget → `/tmp/chrome-9222-ALERT`). mini2 profile=`adminbot-profile`; MacBook failover profile=`failover-profile`. Pool `CDP_ENDPOINTS=19222,19223` on browser-01. Failover proven: bootout mini2 tunnel → session on 19223; restore → 19222. atmini is NOT a browser lane.

**2026-07-24 Grok: browser-01 DESTROYED — pool lives on BRAIN.** Vultr `tgi-browser-01` 144.202.88.43 (uuid abe32678-…) deleted. Live path: ecp DO nginx `/browser/` → BRAIN 45.63.39.217 → browser-mcp :3205 → browser-pool :3201 (CDP backend) → mini2 :19222 / MacBook :19223 reverse tunnels (`ssh -p 22122`, keys restricted permitlisten). Root cause of earlier BRAIN hang: stale BrowserBase-era pool dist (fixed by rsync from browser-01). `/opt/bin/browser-do` required for explicit MCP `lane:cdp` (local Chrome+Xvfb on BRAIN); default external lane is pool→mini2. Rollback: restore DO nginx proxy to a new box only if BRAIN fails (box is gone).

**2026-07-30 LANE CHAIN (live on BRAIN):**

| Priority | Where | When |
|----------|--------|------|
| 1 Primary | Vultr BRAIN Chrome fleet (`9224–9232`,`9235`) | Default agent browser work |
| 2 Overflow | mini2 residential (`:19222`) | All Vultr Chromes down/full |
| 3 Failover | MacBook (`:19223`) | mini2 also unavailable |
| Hard | mini2 then MacBook | godaddy/xero/grok/xai/datadome (+ Tony approval) |

Pool env: `CDP_ENDPOINTS` (Vultr), `CDP_FAILOVER_ENDPOINTS` (mini2,macbook), `CDP_HARD_ENDPOINTS` (mini2,macbook). Capacity full → overflow before force-close.

**2026-07-30 world-class pass (items 1-8):**
1. MacBook failover CDP live + tunnel :19223; watchdog re-enabled
2. Fleet health: `/opt/bin/browser-fleet-health.py` + systemd timer 5m + ECP alerts on degrade/recover
3. admin-bot :9235 warmed (example.com + generate_204)
4. Least-tabs pick under `CDP_MAX_TABS=28` (skips busy Chromes)
5. Hard-lane audit: `hard-lane.log.jsonl` + rate-limited ECP `browser-hard-lane` alert
6. Git: browser-pool `9df0143` on `feat/openrouter-prc-pin` (dist/pool.js, start.sh, scripts/*)
7. Hygiene: blank-tab close when over tab budget in health job
8. Observability: `session_create` rows (lane/endpoint/tabs) + fleet_health metrics in sessions.log.jsonl

---

# reference-amazon-personal-seat-mini2.md

---
name: reference-amazon-personal-seat-mini2
description: Amazon personal seat lives on mini2 CDP 9226 / tunnel 19228 as t@iambetterandbetter.com; OTP arrives in the tony@ mailbox
metadata: 
  node_type: memory
  type: reference
  originSessionId: 61ac3453-7fa7-494c-aff5-cc7528e7dd6a
  modified: 2026-08-28T17:34:01.554Z
---

Amazon personal = **mini2 CDP 9226 → BRAIN tunnel 19228**, profile
`~/chrome-hard/amazon-personal`, identity **t@iambetterandbetter.com** (VW item
"Personal Amazon Account - Tony"). Armed 2026-08-03. It stays on mini2 —
amazon.com serves the bot wall to datacenter IPs (Vultr seat 9237 included); mini2's
residential egress gets the real form.

- **OTP:** `t@iambetterandbetter.com` is an alias, not a mailbox — impersonating it via DWD
  returns `invalid_grant`. The mail lands in **tony@iambetterandbetter.com**, readable with
  the DWD key on the ECP fleet (64.247.206.107). From `account-update@amazon.com`, subject
  "amazon.com: Sign-in attempt", body "your verification code is: NNNNNN".
- **Cookies are persistent** (1-year `at-main` / `sess-at-main` / `x-main`); there is no
  "keep me signed in" checkbox on this flow. After closing the seat, confirm
  `Default/Cookies` has a fresh mtime before trusting the restart.
- **Launcher:** `~/bin/chrome-seat-amazon-personal.sh` on mini2. Health-check the listener's
  open files via `lsof -p` rather than `pgrep -f remote-debugging-port=` (self-match), and use
  `grep -c` rather than `grep -q` — under `set -o pipefail` a `grep -q` SIGPIPEs `lsof` and a healthy
  seat reports "not this seat".
- **Pool routing:** `dist/pool.js` `primaryList()` now pins a service to its dedicated mini2
  seat from `dedicated-seats.json`, so `amazon` can only open on 19228. A pinned service gets
  zero failover. The `/session/create` body key is **`service`** — `serviceName` silently drops
  to the 'general' Vultr fleet lane.

Typing mechanics: [[reference-cdp-keys-need-bringtofront]]. Seat approval rule:
[[feedback-browser-mini2-warm-approval]].
