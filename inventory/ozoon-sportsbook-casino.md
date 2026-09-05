# Ozoon inventory (discovery seed 2026-09-02)
# NOTE: hosts below are discovery candidates from passive DNS/CT; confirm in-scope vs program scope before active testing.
ozoon.com
www.ozoon.com

## PASSIVE RECON 2026-09-02 (read-only, non-intrusive)

> Recon observations only. These are NOT confirmed vulnerabilities; ownership/in-scope of each host must be confirmed against the program scope before any active testing. Hosts resolve + serve HTTP — investigation requires scoped authorization.

**Probed:** 2 hosts | **Live HTTP:** 1

| Host | Status | Server/Tech |
|---|---|---|
| `www.ozoon.com` | 200 | Server: PWS/8.3.1.0.8; Via: 1.1 PS-MAA-01KXf40:1 ( |

**CNAME review signals (1):**
- `www.ozoon.com` -> `www.ozoon.com.wnacloud.com`

## DEEP SERVICE SCAN 2026-09-02 (read-only connect+banner)
**Host:** `www.ozoon.com` | **Ports:** [80, 443, 1080, 1433, 2082, 2083, 2086, 2087, 3306, 7001, 7070, 8000, 8009, 8080, 8081, 8082, 8083, 8088, 8090, 8443, 8800, 8888, 9000, 9090, 9200, 9999, 10000, 50070]
**Non-web ports observed:** [1080, 1433, 2082, 2083, 2086, 2087, 3306, 7001, 7070, 8000, 8009, 8080, 8081, 8082, 8083, 8088, 8090, 8443, 8800, 8888, 9000, 9090, 9200, 9999, 10000, 50070]
> NOTE: repeated identical non-web port sets (e.g. 2082,2083,2086,2087,8080,8443) across many hosts and wide port sets are likely a shared edge/proxy answering EOF, NOT confirmed real services. Verify with a proper port scanner (e.g. nmap) under authorization before treating as real. These are surface-map hints only, not findings.

## 2026-09-02 21:55:20 UTC

## 2026-09-03 00:00:11 UTC

## 2026-09-03 04:18:13 UTC

## 2026-09-03 09:03:56 UTC

## 2026-09-03 13:36:17 UTC

## 2026-09-03 17:24:39 UTC
- NEW www.ozoon.com live HTTP 200 with Server: PWS/8.3.1.0.8; CNAME to wnacloud.com (shared hosting edge)
- NEW Wildcard-dominated subdomain space — 0 genuinely dedicated hosts after filtering
- NEW 6 code-level hypotheses from reposcan (hardcoded creds, SQLi, debug mode, API keys) in Ozoon GitHub repos
- CHANGED Deep port scan shows 27 non-web ports but likely shared proxy EOF responses, not real services
- NEW `ozoon.eu` — actual gambling platform (sportsbook/casino/poker), NOT in inventory. Discovered via web search; ozoon.com is marketing-only.
- NEW `services.ozoon.eu` — backend service domain found in HTML `<link rel="preconnect">` hints on ozoon.eu.
- NEW 3 test pages exposed in ozoon.eu sitemap: `/promotions-test` (live, empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404).
- NEW Legacy "Bodog" branding at `/contents/chatbox-psat` — customer survey still references old brand name.
- NEW ozoon.com runs Next.js on PWS/8.3.1.0.8 with CNAME to `wnacloud.com`; ozoon.eu uses Next.js + micro-frontend architecture (React 19 + TanStack Query shared runtime).

## 2026-09-03 20:08:18 UTC

## 2026-09-03 22:51:06 UTC
- NEW `ozoon.eu` confirmed as core gambling platform (sportsbook/casino/poker) with Next.js + micro-frontend (React 19 + TanStack Query), NOT in original inventory
- NEW `services.ozoon.eu` confirmed as backend API domain via HTML `<link rel="preconnect">` and SDK bundle analysis
- NEW Bigpickle mapped specific API surface: `/services/*/v1/profiles/{uuid}/...` pattern across referral, wallet-gateway, transaction-group, player-verification, player-messages (all return 401 unauthentic
- NEW Bigpickle discovered `X-MOCK-2FA-VERIFICATION` client-controlled header on `/services/player-verification/v1/profiles/{sid}/verifications/verify` — potential 2FA/KYC bypass backdoor
- NEW Bigpickle confirmed `/api/v1/signup` endpoint accepts `attributes` and `address` sub-objects client-controlled
- NEW 3 test pages exposed on ozoon.eu: `/promotions-test` (200 empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404)
- NEW Legacy "Bodog" branding at `/contents/chatbox-psat` — old brand references in customer survey
- CHANGED Probes only executed against `www.ozoon.com` (marketing edge); **zero probes** against `ozoon.eu` or `services.ozoon.eu` (core platform + API)
- CHANGED Risk elevated: nemotron3=75, bigpickle=72 (was 65/65) — core platform confirmed, API surface mapped, critical vectors identified

## 2026-09-04 00:48:41 UTC
- CHANGED Zero probes executed against `ozoon.eu` or `services.ozoon.eu` (core platform + API) despite confirmed critical attack surface; all probes to date target only `www.ozoon.com` marketing edge
- CHANGED Risk elevated to 78 (from 65/75) — core platform confirmed, API surface fully mapped via public SDK bundles, three critical vectors identified (BOLA UUID 62, 2FA mock-header 48, SSRF metadata 55)
- NEW `services.ozoon.eu/services/*/v1/profiles/{uuid}/...` pattern confirmed across 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) — all return 401 unauthent
- NEW `X-MOCK-2FA-VERIFICATION` client-controlled header discovered in production SDK on verification endpoint
- NEW `/api/v1/signup` endpoint confirmed accepting client-controlled `attributes` and `address` sub-objects
- NEW 3 test pages exposed on `ozoon.eu`: `/promotions-test` (200 empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404)
- NEW Legacy "Bodog" branding at `/contents/chatbox-psat` — old brand references in customer survey

## 2026-09-04 05:13:58 UTC
- NEW Bigpickle confirmed `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT currencies with mandatory province/postal code fields — maps server-side validation surface for mass-ass
- NEW Bigpickle confirmed reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config
- NEW Bigpickle confirmed static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified
- NEW Bigpickle confirmed `two_factor_authenticator.allow-permanent-skip:["true"]` in production config — supports mock-2FA header bypass hypothesis
- NEW Bigpickle live probes to random + VIP UUIDs on profile-keyed endpoints return 401 unauthenticated — binding unresolved
- CHANGED Risk stable at 78 — three critical vectors confirmed via public SDK/config: BOLA UUID (62), mock-2FA header (48→50), SSRF metadata (55)
- CHANGED Zero authenticated probes executed against core platform (ozoon.eu/services.ozoon.eu); all probes to date target only www.ozoon.com marketing edge

## 2026-09-04 09:50:28 UTC
- NEW Bigpickle confirmed `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT currencies with mandatory province/postal code fields — maps server-side validation surface for mass-ass
- NEW Bigpickle confirmed reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config
- NEW Bigpickle confirmed static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified
- NEW Bigpickle confirmed `two_factor_authenticator.allow-permanent-skip:["true"]` in production config — supports mock-2FA header bypass hypothesis
- NEW Bigpickle live probes to random + VIP UUIDs on profile-keyed endpoints return 401 unauthenticated — binding unresolved
- CHANGED Risk stable at 78 — three critical vectors confirmed via public SDK/config: BOLA UUID (62), mock-2FA header (48→50), SSRF metadata (55)
- CHANGED Zero authenticated probes executed against core platform (ozoon.eu/services.ozoon.eu); all probes to date target only www.ozoon.com marketing edge

## 2026-09-04 14:26:01 UTC
- NEW Bigpickle risk elevated to 80 (from 78) with three refined FINAL hypotheses: BOLA UUID (confidence 65↑), Mock-2FA header (55↑), Mass-assignment (60↑) — nemotron3 remains at 78
- NEW Bigpickle next action shifted to AUTH_HELPED gateway: POST https://www.ozoon.eu/api/v1/signup (register throwaway for session cookie) vs nemotron3's passive GET profileupdateform
- CHANGED Both models confirm zero authenticated probes executed against ozoon.eu/services.ozoon.eu despite confirmed critical attack surface; all probes to date target only www.ozoon.com marketing edge
- NEW Bigpickle PRIO scores: services.ozoon.eu UUID endpoints 9.5, player-verification 8.5, www.ozoon.eu signup 7.5 — all AUTH_HELPED gate_ease 6-8

## 2026-09-04 18:02:21 UTC

## 2026-09-04 20:04:44 UTC
- NEW `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
- NEW Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
- CHANGED Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-04 22:19:14 UTC
- CHANGED No new recon executed this cycle; KB is pure consolidation of prior findings. All three FINAL hypotheses remain AUTH_HELPED and unprobed.
- CHANGED KB now flags the signup gateway as `account_creation:restricted` (BOLA LEARN tail) — POST /api/v1/signup is a mutating create on a live real-money platform requiring explicit program authorization, no
- NEW `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
- NEW Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
- CHANGED Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 00:16:35 UTC
- NEW `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
- NEW Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
- CHANGED Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 04:46:32 UTC
- NEW `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
- NEW Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
- CHANGED Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
- CHANGED No new recon executed this cycle; KB is pure consolidation of prior findings. All three FINAL hypotheses remain AUTH_HELPED and unprobed.
- CHANGED KB now flags the signup gateway as `account_creation:restricted` (BOLA LEARN tail) — POST /api/v1/signup is a mutating create on a live real-money platform requiring explicit program authorization, no
- NEW `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
- NEW Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
- CHANGED Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
- CHANGED No new recon executed this cycle; KB is pure consolidation of prior findings. All three FINAL hypotheses remain AUTH_HELPED and unprobed.
- CHANGED KB now flags the signup gateway as `account_creation:restricted` (BOLA LEARN tail) — POST /api/v1/signup is a mutating create on a live real-money platform requiring explicit program authorization, no
- NEW `ozoon.eu` — actual gambling platform (sportsbook/casino/poker), NOT in inventory. Discovered via web search; ozoon.com is marketing-only.
- NEW `services.ozoon.eu` — backend service domain found in HTML `<link rel="preconnect">` hints
- NEW First direct probes of the public core-API host: `GET https://services.ozoon.eu/` → 743KB Next.js SSR shell; extracts `servicesBaseUrl=https://services.services.ozoon.eu` (internal-only hostname, publ
- NEW Structured auth-gate discriminator: `GET /services/wallet-gateway/v1/profiles/{bogus-uuid}/balances` → `401 {"message":"Authentication required for requested route","errorCode":"unauthorized"}` — auth
- NEW `GET /api-docs` → Spring Boot Whitelabel JSON 404 ⇒ backend = Spring Boot microservices behind the BFF; no actuator/swagger leak on `/openapi.json`, `/v3/api-docs`, `/actuator/health`, `/services/*/v{
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for the mock-2FA path → confidence 60→55.
- NEW Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` but knowledge base + all model leads confirm `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all 18 probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 08:43:46 UTC
- NEW Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` but knowledge base + all model leads confirm `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all 18 probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
- NEW Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 12:12:10 UTC
- NEW Inventory file lists only `ozoon.com`/`www.ozoon.com` (marketing edge); `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but absent from inven
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
- CHANGED Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
- NEW Direct core-API probes executed this cycle: Spring Boot stack behind BFF confirmed; structured 401 `errorCode:"unauthorized"` for valid-route bogus UUID = auth pre-check precedes resource lookup; inte
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
- NEW `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
- NEW `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- NEW `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak

## 2026-09-05 15:34:45 UTC
- NEW Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` (marketing edge); `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) confirmed in-scope per sc
- NEW Direct core-API probes executed: Spring Boot stack behind BFF confirmed; structured 401 `errorCode:"unauthorized"` for valid-route bogus UUID = auth pre-check precedes resource lookup; internal-only `
- NEW `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
- NEW `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- NEW `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
- CHANGED Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 17:45:43 UTC
- NEW `ozoon.eu` (core gambling platform: sportsbook/casino/poker) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but **absent from inventory file** (only `ozoon.com`/`www.ozoon.com`
- NEW Direct core-API probes executed: `GET https://services.ozoon.eu/` → 743KB Next.js SSR shell; `servicesBaseUrl=https://services.services.ozoon.eu` (internal-only, NXDOMAIN publicly = split-horizon)
- NEW Structured auth-gate: `GET /services/wallet-gateway/v1/profiles/{bogus-uuid}/balances` → `401 {"errorCode":"unauthorized"}` — auth pre-check precedes resource lookup; anonymous differential baseline r
- NEW `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak
- NEW `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
- NEW `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- NEW `www.ozoon.eu/api/v1`: GET `/api/v1/login` and `/api/v1/whoami` → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WAF "Request Rejected" (support_id 6279731412718049849) →
- NEW `/services/site-config/v1/countries/{CC}/profileupdateform`: unknown country → structured 404 leaking internal exception `io.crazy88.beatrix.siteconfig.exception.CountryNotFoundException` (GoBet/Beatr
- NEW `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT (XBT unpublished) with mandatory province/postal code; US=USD — update DTO strict but signup DTO separate
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
- CHANGED Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)

## 2026-09-05 19:37:04 UTC
- NEW `ozoon.eu` (core gambling platform: sportsbook/casino/poker) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but **absent from inventory file** (only `ozoon.com`/`www.ozoon.com`
- NEW Direct core-API probes executed: `GET https://services.ozoon.eu/` → 743KB Next.js SSR shell; `servicesBaseUrl=https://services.services.ozoon.eu` (internal-only, NXDOMAIN publicly = split-horizon)
- NEW Structured auth-gate: `GET /services/wallet-gateway/v1/profiles/{bogus-uuid}/balances` → `401 {"errorCode":"unauthorized"}` — auth pre-check precedes resource lookup; anonymous differential baseline r
- NEW `www.ozoon.eu/api/v1`: GET `/api/v1/login` and `/api/v1/whoami` → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WAF "Request Rejected" (support_id 6279731412718049849) →
- NEW `/services/site-config/v1/countries/{CC}/profileupdateform`: unknown country → structured 404 leaking internal exception `io.crazy88.beatrix.siteconfig.exception.CountryNotFoundException` (GoBet/Beatr
- NEW `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT (XBT unpublished) with mandatory province/postal code; US=USD — update DTO strict but signup DTO separate
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway

## 2026-09-05 21:47:51 UTC
- NEW Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` (marketing edge); `ozoon.eu` (core gambling platform: sportsbook/casino/poker) and `services.ozoon.eu` (backend API) 
- NEW Direct core-API probes executed this cycle: `GET https://services.ozoon.eu/` → 743KB Next.js SSR shell; `servicesBaseUrl=https://services.services.ozoon.eu` (internal-only, NXDOMAIN publicly = split-h
- NEW Structured auth-gate discriminator confirmed: `GET /services/wallet-gateway/v1/profiles/{bogus-uuid}/balances` → `401 {"errorCode":"unauthorized"}` — auth pre-check precedes resource lookup; anonymous
- NEW `www.ozoon.eu/api/v1`: GET `/api/v1/login` and `/api/v1/whoami` → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WAF "Request Rejected" (support_id 6279731412718049849) →
- NEW `/services/site-config/v1/countries/{CC}/profileupdateform`: unknown country → structured 404 leaking internal exception `io.crazy88.beatrix.siteconfig.exception.CountryNotFoundException` (GoBet/Beatr
- NEW `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT (XBT unpublished) with mandatory province/postal code; US=USD — update DTO strict but signup DTO separate
- CHANGED Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all 18+ probe rounds; all probes target only `www.ozoon.com` marketing edge
- CHANGED Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
- CHANGED Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
