## 2026-09-03 17:24:27 UTC [target] (model nemotron3)
[NEW] www.ozoon.com live HTTP 200 with Server: PWS/8.3.1.0.8; CNAME to wnacloud.com (shared hosting edge)
[NEW] Wildcard-dominated subdomain space — 0 genuinely dedicated hosts after filtering
[NEW] 6 code-level hypotheses from reposcan (hardcoded creds, SQLi, debug mode, API keys) in Ozoon GitHub repos
[CHANGED] Deep port scan shows 27 non-web ports but likely shared proxy EOF responses, not real services
[PRIO] www.ozoon.com,7.5,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[PRIO] ozoon.com (root),4.0,attack_surface=4|business_value=8|tech_exposure=3|gate_ease=10|cloud_surface=4|freshness=5
[PRIO] hyper PHP app (if deployed),6.8,attack_surface=7|business_value=6|tech_exposure=9|gate_ease=5|cloud_surface=4|freshness=7
[PRIO] mas-film PHP app (if deployed),6.3,attack_surface=6|business_value=5|tech_exposure=9|gate_ease=5|cloud_surface=4|freshness=7
[HYP] PWS Edge SSRF to Cloud Metadata
class: SSRF
asset: www.ozoon.com
confidence: 55
reasoning: Server header shows PWS/8.3.1.0.8 behind wnacloud.com CNAME. Shared cloud hosting edges often have fetch/fetch-like functionality for image proxying, webhooks, or PDF generation. Port scan shows 8080,8443,8000,8081,8082,8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch) and test with 169.254.169.254/latest/meta-data/
verify_steps: GET https://www.ozoon.com/ (map all params), GET https://www.ozoon.com/api/ (if exists), GET https://www.ozoon.com/robots.txt, GET https://www.ozoon.com/sitemap.xml, test any url=, callback=, image=, webhook= param with http://169.254.169.254/latest/meta-data/iam/security-credentials/
impact: Cloud metadata credentials -> full cloud account compromise (HIGH/CRITICAL)
testability: PASSIVE
[HYP] Deployed PHP App SQL Injection via Reused Code
class: OTHER
asset: www.ozoon.com (potential hyper/mas-film deployment)
confidence: 45
reasoning: Reposcan found SQLi in oZoon/hyper/core/functions.php (7 locations) and oZoon/secure-query-string/settings.php using string concatenation. If any of these PHP apps are deployed on www.ozoon.com or subdomains, SQLi is highly likely. PWS server suggests PHP capability.
evidence_needed: Confirm PHP app deployment on www.ozoon.com; identify input vectors (id, uid, cat, page, search, user_id)
verify_steps: GET https://www.ozoon.com/ (check for .php extensions, PHPSESSID), GET https://www.ozoon.com/hyper/, GET https://www.ozoon.com/mas-film/, GET https://www.ozoon.com/api/, test ' OR '1'='1 on any numeric/id param found
impact: Database compromise, authentication bypass, RCE potential (HIGH)
testability: PASSIVE
[HYP] Hardcoded Credentials Reused in Production
class: SECRET
asset: www.ozoon.com (MySQL 3306 if exposed)
confidence: 40
reasoning: hyper repo has MySQL user='hyper' pass='12345'; mas-film has user='masha' pass='12345'. Deep scan shows port 3306 responding on www.ozoon.com (likely shared proxy EOF but must verify). If real MySQL and creds reused, full DB compromise.
evidence_needed: Confirm port 3306 is real MySQL (not proxy EOF); test credentials if service banner confirms MySQL
verify_steps: HEAD https://www.ozoon.com:3306/ (invalid - MySQL not HTTP), but passive: check if any subdomain has phpmyadmin, adminer, or db admin panel; test MySQL auth only if banner confirms real service via authorized scan
impact: Full database access, PII, gaming/financial data (CRITICAL)
testability: PASSIVE (banner grab only; no auth attempts without confirmation)
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 likely shared proxy EOF not real MySQL; no verify steps without confirmed real service
[FINAL] PWS Edge SSRF to Cloud Metadata (confidence 55) — highest value, passive verify possible
[FINAL] Deployed PHP App SQL Injection via Reused Code (confidence 45) — code evidence strong, deployment unconfirmed
[NEXT] PROBE: GET https://www.ozoon.com/ — full header/body capture to map all endpoints, parameters, PHP session cookies, and identify any url=, callback=, image=, webhook=, fetch=, import=, pdf= parameters for SSRF testing
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[RISK] ozoon-sportsbook-casino: 65 — Single live asset (www.ozoon.com) on shared cloud edge with PWS server; code repos show systemic PHP security anti-patterns (SQLi, hardcoded creds, debug mode); if any repo code deployed, HIGH impact likely. Wildcard DNS limits subdomain attack surface but main app is high-value gaming platform.
## 2026-09-03 20:00:27 UTC [target] (model nemotron3)
[PRIO] ozoon.eu,8.85,attack_surface=9|business_value=10|tech_exposure=8|gate_ease=10|cloud_surface=6|freshness=8
[PRIO] services.ozoon.eu,8.40,attack_surface=8|business_value=10|tech_exposure=9|gate_ease=7|cloud_surface=7|freshness=8
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[PRIO] hyper PHP app (if deployed),6.80,attack_surface=7|business_value=6|tech_exposure=9|gate_ease=5|cloud_surface=4|freshness=7
[PRIO] mas-film PHP app (if deployed),6.30,attack_surface=6|business_value=5|tech_exposure=9|gate_ease=5|cloud_surface=4|freshness=7
[PRIO] ozoon.com (root),4.00,attack_surface=4|business_value=8|tech_exposure=3|gate_ease=10|cloud_surface=4|freshness=5
[HYP] Registration Form Mass Assignment / Country Override Bypass
class: BUSLOGIC
asset: ozoon.eu
confidence: 55
reasoning: Registration form includes referral email field and country selector rendered client-side (Next.js + React 19 + TanStack Query). Server-side validation is sole defense. Micro-frontend architecture implies API calls to services.ozoon.eu. Test pages exposed (/promotions-test, /blackjack-main-test, /casino/how-to-play-casino/odds-test) indicate active development branches where validation may differ.
evidence_needed: Identify registration API endpoint and full request schema; confirm server accepts extra fields (role, affiliate_id, country_override, kyc_level, bonus_eligible)
verify_steps: GET https://ozoon.eu/register (capture form action, fetch/XHR to services.ozoon.eu); POST https://services.ozoon.eu/api/v1/auth/register with JSON body including extra fields (role=admin, country=US, kyc_level=verified) alongside required fields; observe response for mass assignment acceptance or validation error
impact: Account takeover via role escalation, bonus fraud, geo-restriction bypass, KYC evasion — HIGH/CRITICAL on gambling platform
testability: AUTH_HELPED (requires valid registration flow interception)
[HYP] IDOR/BOLA on Micro-frontend API Endpoints
class: IDOR
asset: services.ozoon.eu
confidence: 55
reasoning: HTML source on ozoon.eu contains <link rel="preconnect" href="https://services.ozoon.eu"> confirming backend API domain. Micro-frontend architecture (React 19 + TanStack Query shared runtime) means all user actions (wallet, bets, promotions, profile) route through this API. Sequential numeric IDs (user_id, bet_id, transaction_id, promotion_id) likely used without ownership checks.
evidence_needed: Enumerate API endpoints via browser interception; find object references with predictable IDs; confirm cross-user access
verify_steps: GET https://ozoon.eu/ (open DevTools Network, filter fetch/XHR, map all services.ozoon.eu calls); GET https://services.ozoon.eu/api/v1/user/profile (check for user_id param); GET https://services.ozoon.eu/api/v1/wallet/balance?user_id=123 (test IDOR); GET https://services.ozoon.eu/api/v1/bets/history?user_id=123; GET https://services.ozoon.eu/api/v1/promotions/claim?promo_id=1&user_id=123
impact: Cross-tenant PII dump, balance manipulation, bet history theft, promotion abuse — HIGH/CRITICAL
testability: AUTH_HELPED (requires authenticated session for API calls)
[HYP] PWS Edge SSRF to Cloud Metadata
class: SSRF
asset: www.ozoon.com
confidence: 55
reasoning: Server header shows PWS/8.3.1.0.8 behind wnacloud.com CNAME (shared hosting edge). Shared cloud edges often have fetch/proxy functionality for image proxying, webhooks, PDF generation. Deep port scan shows 8080,8443,8000,8081-8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch, url, src, link) and test with 169.254.169.254/latest/meta-data/
verify_steps: GET https://www.ozoon.com/ (full header/body capture, map all params, PHPSESSID, .php extensions); GET https://www.ozoon.com/api/ (if exists); GET https://www.ozoon.com/robots.txt; GET https://www.ozoon.com/sitemap.xml; test any url=, callback=, image=, webhook=, fetch=, import=, pdf= param with http://169.254.169.254/latest/meta-data/iam/security-credentials/
impact: Cloud metadata credentials -> full cloud account compromise (HIGH/CRITICAL)
testability: PASSIVE
[PARKED] Deployed PHP App SQL Injection via Reused Code: confidence 45 but deployment unconfirmed (hyper/mas-film repos not verified on www.ozoon.com or subdomains); no verify steps without confirmed PHP endpoints
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 likely shared proxy EOF not real MySQL; no auth testing without confirmed real service
[FINAL] 1. Registration Form Mass Assignment / Country Override Bypass (55) — ozoon.eu
[FINAL] 2. IDOR/BOLA on Micro-frontend API Endpoints (55) — services.ozoon.eu
[FINAL] 3. PWS Edge SSRF to Cloud Metadata (55) — www.ozoon.com
[NEXT] PROBE: GET https://ozoon.eu/register — full header/body capture with DevTools Network recording to map registration form action, fetch/XHR endpoints to services.ozoon.eu, request schema, and identify all mutable fields (referral_email, country, plus hidden/mass-assignment candidates)
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[RISK] ozoon-sportsbook-casino: 75 — Core gambling platform (ozoon.eu) confirmed with micro-frontend API (services.ozoon.eu), test pages exposed, legacy code paths (Bodog), and client-side validation only on registration. Marketing edge (www.ozoon.com) on shared PWS/wnacloud adds SSRF-to-metadata risk. Systemic PHP anti-patterns in repos (SQLi, hardcoded creds, debug mode) if any deployed code shares lineage. High business value + high attack surface + low gate ease = elevated risk.
## 2026-09-03 22:35:59 UTC [target] (model nemotron3)
[NEW] `ozoon.eu` confirmed as core gambling platform (sportsbook/casino/poker) with Next.js + micro-frontend (React 19 + TanStack Query), NOT in original inventory
[NEW] `services.ozoon.eu` confirmed as backend API domain via HTML `<link rel="preconnect">` and SDK bundle analysis
[NEW] Bigpickle mapped specific API surface: `/services/*/v1/profiles/{uuid}/...` pattern across referral, wallet-gateway, transaction-group, player-verification, player-messages (all return 401 unauthenticated)
[NEW] Bigpickle discovered `X-MOCK-2FA-VERIFICATION` client-controlled header on `/services/player-verification/v1/profiles/{sid}/verifications/verify` — potential 2FA/KYC bypass backdoor
[NEW] Bigpickle confirmed `/api/v1/signup` endpoint accepts `attributes` and `address` sub-objects client-controlled
[NEW] 3 test pages exposed on ozoon.eu: `/promotions-test` (200 empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404)
[NEW] Legacy "Bodog" branding at `/contents/chatbox-psat` — old brand references in customer survey
[CHANGED] Probes only executed against `www.ozoon.com` (marketing edge); **zero probes** against `ozoon.eu` or `services.ozoon.eu` (core platform + API)
[CHANGED] Risk elevated: nemotron3=75, bigpickle=72 (was 65/65) — core platform confirmed, API surface mapped, critical vectors identified
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu,8.85,attack_surface=9|business_value=10|tech_exposure=10|gate_ease=7|cloud_surface=7|freshness=9
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[HYP] BOLA/IDOR on Profile-Keyed UUID API Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 62
reasoning: Bigpickle SDK analysis confirms every sensitive resource (wallet balances, transactions, referral, messages, profile, rewards) addressed by UUID in path: `/services/wallet-gateway/v1/profiles/{pid}/balances`, `/services/transaction-group/v1/profiles/{sid}/transactions`, `/services/referral/v1/profiles/{sid}`, `/services/player-messages/v1/profiles/{sid}/messages`. All return 401 unauthenticated. reCAPTCHA action map confirms these exact patterns. If session cookie (`sid`) not bound to path UUID, cross-account read/write via UUID manipulation.
evidence_needed: With valid own session, GET own `/services/wallet-gateway/v1/profiles/{ownPID}/balances` then swap {ownPID} for another UUID; observe 200 with foreign data (BOLA confirmed) vs 403/401 (bound)
verify_steps: 1. Login (AUTH_HELPED) capture own sessionId cookie. 2. GET `/services/referral/v1/profiles/{own-id}` → baseline. 3. GET same with incremented/decremented/random UUIDs; compare 200 (BOLA) vs 403/401 (bound). 4. Repeat on `/transactions`, `/player-messages`, `/balances`. 5. Test write: PUT `/services/profile/v1/profiles/{other-id}` with modified data
impact: Cross-tenant PII dump (names, DOB, emails, messages), financial transaction history, referral data, wallet balances; write variants → account modification — HIGH/CRITICAL
testability: AUTH_HELPED
[HYP] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 48
reasoning: Production SDK sends `X-MOCK-2FA-VERIFICATION` header (boolean string) on verification endpoint. Presence of client-controlled "mock verification" header in money-platform verification path indicates test backdoor; if honored with valid session, trusts client to claim 2FA/phone/email/PIN verification completed.
evidence_needed: With own session, POST `/verifications/verify` with `X-MOCK-2FA-VERIFICATION: true` vs `false` and unverified attribute; observe whether mocked verification accepted (bypass) vs rejected
verify_steps: 1. Login, obtain session. 2. POST `/services/player-verification/v1/profiles/{sid}/verifications/verify` with `X-MOCK-2FA-VERIFICATION: true` (documented mutation) and inspect status/result. 3. Compare to header absent. Only on self-created throwaway account.
impact: Critical if honored — bypass 2FA/phone/PIN/KYC verification gates on withdrawal/bonus flow (money security control bypass) — CRITICAL
testability: AUTH_HELPED
[HYP] PWS Edge SSRF to Cloud Metadata
class: SSRF
asset: https://www.ozoon.com
confidence: 55
reasoning: Server header `PWS/8.3.1.0.8` behind `wnacloud.com` CNAME (shared hosting edge). Shared cloud edges often have fetch/proxy functionality for image proxying, webhooks, PDF generation. Deep port scan shows 8080,8443,8000,8081-8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch, url, src, link) and test with `169.254.169.254/latest/meta-data/`
verify_steps: GET https://www.ozoon.com/ (full header/body capture, map all params, PHPSESSID, .php extensions); GET https://www.ozoon.com/api/ (if exists); GET https://www.ozoon.com/robots.txt; GET https://www.ozoon.com/sitemap.xml; test any url=, callback=, image=, webhook=, fetch=, import=, pdf= param with `http://169.254.169.254/latest/meta-data/iam/security-credentials/`
impact: Cloud metadata credentials → full cloud account compromise (HIGH/CRITICAL)
testability: PASSIVE
[PARKED] Registration Form Mass Assignment / Country Override Bypass (ozoon.eu): confidence 55 but **subsumed** by Bigpickle's confirmed `/api/v1/signup` endpoint with concrete schema; new hypothesis covers same vector with higher precision
[PARKED] IDOR/BOLA on Micro-frontend API Endpoints (services.ozoon.eu): confidence 55 but **superseded** by Bigpickle's specific UUID-path BOLA hypothesis (confidence 62) with confirmed endpoint patterns
[PARKED] Deployed PHP App SQL Injection via Reused Code: confidence 45, deployment unconfirmed (hyper/mas-film not verified on www.ozoon.com); REJECTED class OTHER per knowledge base
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 confirmed shared proxy EOF; REJECTED class SECRET per knowledge base
[FINAL] 1. BOLA/IDOR on Profile-Keyed UUID API Endpoints (62) — services.ozoon.eu
[FINAL] 2. 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header (48) — services.ozoon.eu
[FINAL] 3. PWS Edge SSRF to Cloud Metadata (55) — www.ozoon.com
[NEXT] PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptable fields for signup hypothesis; read-only, no auth required, maps server-side validation surface
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by profile UUID in path.
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potential 2FA/KYC bypass backdoor.
[RISK] ozoon-sportsbook-casino: 78 — Core gambling platform (ozoon.eu) confirmed with micro-frontend API (services.ozoon.eu) fully mapped via public SDK bundles. Three critical vectors identified: (1) BOLA on UUID-keyed profile endpoints across wallet/transactions/referral/messages (confidence 62), (2) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header (confidence 48), (3) SSRF-to-metadata on marketing edge (confidence 55). Test pages exposed, legacy Bodog code paths, client-side validation only on signup. F5 ASM WAF, reCAPTCHA v3, 2FA present but backend binding correctness and mock-header handling unverified. High business value (real-money gaming, crypto/Interac) + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 00:48:32 UTC [target] (model nemotron3)
[CHANGED] Zero probes executed against `ozoon.eu` or `services.ozoon.eu` (core platform + API) despite confirmed critical attack surface; all probes to date target only `www.ozoon.com` marketing edge
[CHANGED] Risk elevated to 78 (from 65/75) — core platform confirmed, API surface fully mapped via public SDK bundles, three critical vectors identified (BOLA UUID 62, 2FA mock-header 48, SSRF metadata 55)
[NEW] `services.ozoon.eu/services/*/v1/profiles/{uuid}/...` pattern confirmed across 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) — all return 401 unauthenticated
[NEW] `X-MOCK-2FA-VERIFICATION` client-controlled header discovered in production SDK on verification endpoint
[NEW] `/api/v1/signup` endpoint confirmed accepting client-controlled `attributes` and `address` sub-objects
[NEW] 3 test pages exposed on `ozoon.eu`: `/promotions-test` (200 empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404)
[NEW] Legacy "Bodog" branding at `/contents/chatbox-psat` — old brand references in customer survey
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu,8.85,attack_surface=9|business_value=10|tech_exposure=10|gate_ease=7|cloud_surface=7|freshness=9
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[HYP] BOLA/IDOR on Profile-Keyed UUID API Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 62
reasoning: Bigpickle SDK analysis confirms every sensitive resource (wallet balances, transactions, referral, messages, profile, rewards) addressed by UUID in path: `/services/wallet-gateway/v1/profiles/{pid}/balances`, `/services/transaction-group/v1/profiles/{sid}/transactions`, `/services/referral/v1/profiles/{sid}`, `/services/player-messages/v1/profiles/{sid}/messages`. All return 401 unauthenticated. reCAPTCHA action map confirms these exact patterns. If session cookie (`sid`) not bound to path UUID, cross-account read/write via UUID manipulation.
evidence_needed: With valid own session, GET own `/services/wallet-gateway/v1/profiles/{ownPID}/balances` then swap {ownPID} for another UUID; observe 200 with foreign data (BOLA confirmed) vs 403/401 (bound)
verify_steps: 1. Login (AUTH_HELPED) capture own sessionId cookie. 2. GET `/services/referral/v1/profiles/{own-id}` → baseline. 3. GET same with incremented/decremented/random UUIDs; compare 200 (BOLA) vs 403/401 (bound). 4. Repeat on `/transactions`, `/player-messages`, `/balances`. 5. Test write: PUT `/services/profile/v1/profiles/{other-id}` with modified data
impact: Cross-tenant PII dump (names, DOB, emails, messages), financial transaction history, referral data, wallet balances; write variants → account modification — HIGH/CRITICAL
testability: AUTH_HELPED
[HYP] Registration Mass Assignment / Business Logic Field Injection
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 55
reasoning: `/api/v1/signup` POST accepts `{email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode}, attributes:{currency}}`. The `attributes` and `address` sub-objects are client-controlled. Country default in form is "United States" while platform is Canada/softblocked; country has business-logic value (bonus eligibility, territory controls). Server-side validation is the only control (client-side rendering confirmed). Referral field `referringSiteToken` flows into invitation signup.
evidence_needed: On a throwaway account, POST `/api/v1/signup` adding extra keys (`attributes.role`, `vip_level`, `is_verified`, `balance`) and observe whether they persist/alter privileges or bonus amounts vs a control signup
verify_steps: 1. Register control account via normal form, capture full POST body. 2. Register test account adding extra body fields (role/vip/balance) — observe acceptance/rejection. 3. Test country override to a restricted territory to check territory-control bypass (softblock enforcement)
impact: Medium-High — Referral bonus manipulation, territory/regulatory controls bypass, possible privilege elevation if mass assignment on role
testability: AUTH_HELPED
[HYP] PWS Edge SSRF to Cloud Metadata
class: SSRF
asset: https://www.ozoon.com
confidence: 55
reasoning: Server header `PWS/8.3.1.0.8` behind `wnacloud.com` CNAME (shared hosting edge). Shared cloud edges often have fetch/proxy functionality for image proxying, webhooks, PDF generation. Deep port scan shows 8080,8443,8000,8081-8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch, url, src, link) and test with `169.254.169.254/latest/meta-data/`
verify_steps: GET https://www.ozoon.com/ (full header/body capture, map all params, PHPSESSID, .php extensions); GET https://www.ozoon.com/api/ (if exists); GET https://www.ozoon.com/robots.txt; GET https://www.ozoon.com/sitemap.xml; test any url=, callback=, image=, webhook=, fetch=, import=, pdf= param with `http://169.254.169.254/latest/meta-data/iam/security-credentials/`
impact: Cloud metadata credentials → full cloud account compromise (HIGH/CRITICAL)
testability: PASSIVE
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header: confidence 48 but same asset (services.ozoon.eu) as higher-confidence BOLA hypothesis (62); verify steps require AUTH_HELPED on self-created throwaway only; keep for depth phase after BOLA confirmation
[PARKED] Deployed PHP App SQL Injection via Reused Code: confidence 45, deployment unconfirmed (hyper/mas-film not verified on www.ozoon.com); REJECTED class OTHER per knowledge base
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 confirmed shared proxy EOF; REJECTED class SECRET per knowledge base
[FINAL] 1. BOLA/IDOR on Profile-Keyed UUID API Endpoints (62) — services.ozoon.eu
[FINAL] 2. Registration Mass Assignment / Business Logic Field Injection (55) — ozoon.eu
[FINAL] 3. PWS Edge SSRF to Cloud Metadata (55) — www.ozoon.com
[NEXT] PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptable fields for signup hypothesis; read-only, no auth required, maps server-side validation surface
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by profile UUID in path.
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potential 2FA/KYC bypass backdoor.
[RISK] ozoon-sportsbook-casino: 78 — Core gambling platform (ozoon.eu) confirmed with micro-frontend API (services.ozoon.eu) fully mapped via public SDK bundles. Three critical vectors identified: (1) BOLA on UUID-keyed profile endpoints across wallet/transactions/referral/messages (confidence 62), (2) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header (confidence 48), (3) SSRF-to-metadata on marketing edge (confidence 55). Test pages exposed, legacy Bodog code paths, client-side validation only on signup. F5 ASM WAF, reCAPTCHA v3, 2FA present but backend binding correctness and mock-header handling unverified. High business value (real-money gaming, crypto/Interac) + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 05:13:49 UTC [target] (model nemotron3)
[NEW] Bigpickle confirmed `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT currencies with mandatory province/postal code fields — maps server-side validation surface for mass-assignment comparison
[NEW] Bigpickle confirmed reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config
[NEW] Bigpickle confirmed static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified
[NEW] Bigpickle confirmed `two_factor_authenticator.allow-permanent-skip:["true"]` in production config — supports mock-2FA header bypass hypothesis
[NEW] Bigpickle live probes to random + VIP UUIDs on profile-keyed endpoints return 401 unauthenticated — binding unresolved
[CHANGED] Risk stable at 78 — three critical vectors confirmed via public SDK/config: BOLA UUID (62), mock-2FA header (48→50), SSRF metadata (55)
[CHANGED] Zero authenticated probes executed against core platform (ozoon.eu/services.ozoon.eu); all probes to date target only www.ozoon.com marketing edge
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu,8.85,attack_surface=9|business_value=10|tech_exposure=10|gate_ease=7|cloud_surface=7|freshness=9
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[HYP] BOLA/IDOR on Profile-Keyed UUID API Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 62
reasoning: Bigpickle SDK analysis confirms every sensitive resource (wallet balances, transactions, referral, messages, profile, rewards) addressed by UUID in path: `/services/wallet-gateway/v1/profiles/{pid}/balances`, `/services/transaction-group/v1/profiles/{sid}/transactions`, `/services/referral/v1/profiles/{sid}`, `/services/player-messages/v1/profiles/{sid}/messages`. All return 401 u[0m
testability: AUTH_HELPED
[HYP] Registration Mass Assignment / Business Logic Field Injection
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 55
reasoning: `/api/v1/signup` POST accepts `{email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode}, attributes:{currency}}`. The `attributes` and `address` sub-objects are client-controlled. Country default in form is "United States" while platform is Canada/softblocked; country has business-logic value (bonus eligibility, territory controls). Server-side validation is the only control (client-side rendering confirmed). reCAPTCHA v3 enforced only on `login`, NOT on `signup` or `signup_from_invitation`. Static referral token `refSiteToken` with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified. Profile update form schema (from `/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On a throwaway account, POST `/api/v1/signup` adding extra keys (`attributes.role`, `vip_level`, `is_verified`, `balance`) and observe whether they persist/alter privileges or bonus amounts vs a control signup
verify_steps: 1. Register control account via normal form, capture full POST body. 2. Register test account adding extra body fields (role/vip/balance) — observe acceptance/rejection. 3. Test country override to a restricted territory to check territory-control bypass (softblock enforcement). 4. Test invitation signup with static refSiteToken and unverified referral email.
impact: Medium-High — Referral bonus manipulation, territory/regulatory controls bypass, possible privilege elevation if mass assignment on role
testability: AUTH_HELPED
[HYP] PWS Edge SSRF to Cloud Metadata
class: SSRF
asset: https://www.ozoon.com
confidence: 55
reasoning: Server header `PWS/8.3.1.0.8` behind `wnacloud.com` CNAME (shared hosting edge). Shared cloud edges often have fetch/proxy functionality for image proxying, webhooks, PDF generation. Deep port scan shows 8080,8443,8000,8081-8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch, url, src, link) and test with `169.254.169.254/latest/meta-data/`
verify_steps: GET https://www.ozoon.com/ (full header/body capture, map all params, PHPSESSID, .php extensions); GET https://www.ozoon.com/api/ (if exists); GET https://www.ozoon.com/robots.txt; GET https://www.ozoon.com/sitemap.xml; test any url=, callback=, image=, webhook=, fetch=, import=, pdf= param with `http://169.254.169.254/latest/meta-data/iam/security-credentials/`
impact: Cloud metadata credentials → full cloud account compromise (HIGH/CRITICAL)
testability: PASSIVE
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header: confidence 48→50 but same asset (services.ozoon.eu) as higher-confidence BOLA hypothesis (62); verify steps require AUTH_HELPED on self-created throwaway only; keep for depth phase after BOLA confirmation
[PARKED] Deployed PHP App SQL Injection via Reused Code: confidence 45, deployment unconfirmed (hyper/mas-film not verified on www.ozoon.com); REJECTED class OTHER per knowledge base
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 confirmed shared proxy EOF; REJECTED class SECRET per knowledge base
[FINAL] 1. BOLA/IDOR on Profile-Keyed UUID API Endpoints (62) — services.ozoon.eu
[FINAL] 2. Registration Mass Assignment / Business Logic Field Injection (55) — ozoon.eu
[FINAL] 3. PWS Edge SSRF to Cloud Metadata (55) — www.ozoon.com
[NEXT] PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptable fields for signup hypothesis; read-only, no auth required, maps server-side validation surface
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by profile UUID in path.
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potential 2FA/KYC bypass backdoor.
[RISK] ozoon-sportsbook-casino: 78 — Core gambling platform (ozoon.eu) confirmed with micro-frontend API (services.ozoon.eu) fully mapped via public SDK bundles. Three critical vectors identified: (1) BOLA on UUID-keyed profile endpoints across wallet/transactions/referral/messages (confidence 62), (2) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header (confidence 48), (3) SSRF-to-metadata on marketing edge (confidence 55). Test pages exposed, legacy Bodog code paths, client-side validation only on signup. F5 ASM WAF, reCAPTCHA v3, 2FA present but backend binding correctness and mock-header handling unverified. High business value (real-money gaming, crypto/Interac) + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 09:50:17 UTC [target] (model nemotron3)
[NEW] Bigpickle confirmed `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT currencies with mandatory province/postal code fields — maps server-side validation surface for mass-assignment comparison  
[NEW] Bigpickle confirmed reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config  
[NEW] Bigpickle confirmed static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified  
[NEW] Bigpickle confirmed `two_factor_authenticator.allow-permanent-skip:["true"]` in production config — supports mock-2FA header bypass hypothesis  
[NEW] Bigpickle live probes to random + VIP UUIDs on profile-keyed endpoints return 401 unauthenticated — binding unresolved  
[CHANGED] Risk stable at 78 — three critical vectors confirmed via public SDK/config: BOLA UUID (62), mock-2FA header (48→50), SSRF metadata (55)  
[CHANGED] Zero authenticated probes executed against core platform (ozoon.eu/services.ozoon.eu); all probes to date target only www.ozoon.com marketing edge  
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9  
[PRIO] services.ozoon.eu,8.85,attack_surface=9|business_value=10|tech_exposure=10|gate_ease=7|cloud_surface=7|freshness=9  
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6  
[HYP] BOLA/IDOR on Profile-Keyed UUID API Endpoints  
class: IDOR  
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...  
confidence: 62  
reasoning: Bigpickle SDK analysis confirms every sensitive resource (wallet balances, transactions, referral, messages, profile, rewards) addressed by UUID in path: `/services/wallet-gateway/v1/profiles/{pid}/balances`, `/services/transaction-group/v1/profiles/{sid}/transactions`, `/services/referral/v1/profiles/{sid}`, `/services/player-messages/v1/profiles/{sid}/messages`. All return 401 unauthenticated. reCAPTCHA action map confirms these exact patterns. If session cookie (`sid`) not bound to path UUID, cross-account read/write via UUID manipulation.  
evidence_needed: With valid own session, GET own `/services/wallet-gateway/v1/profiles/{ownPID}/balances` then swap {ownPID} for another UUID; observe 200 with foreign data (BOLA confirmed) vs 403/401 (bound)  
verify_steps: 1. Login (AUTH_HELPED) capture own sessionId cookie. 2. GET `/services/referral/v1/profiles/{own-id}` → baseline. 3. GET same with incremented/decremented/random UUIDs; compare 200 (BOLA) vs 403/401 (bound). 4. Repeat on `/transactions`, `/player-messages`, `/balances`. 5. Test write: PUT `/services/profile/v1/profiles/{other-id}` with modified data  
impact: Cross-tenant PII dump (names, DOB, emails, messages), financial transaction history, referral data, wallet balances; write variants → account modification — HIGH/CRITICAL  
testability: AUTH_HELPED  
[HYP] Registration Mass Assignment / Business Logic Field Injection  
class: BUSLOGIC  
asset: https://www.ozoon.eu/api/v1/signup  
confidence: 55  
reasoning: `/api/v1/signup` POST accepts `{email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode}, attributes:{currency}}`. The `attributes` and `address` sub-objects are client-controlled. Country default in form is "United States" while platform is Canada/softblocked; country has business-logic value (bonus eligibility, territory controls). Server-side validation is the only control (client-side rendering confirmed). reCAPTCHA v3 enforced only on `login`, NOT on `signup` or `signup_from_invitation`. Static referral token `refSiteToken` with `affiliate.check-referring-site.enabled:["false"]` — referral field unverified. Profile update form schema returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.  
evidence_needed: On a throwaway account, POST `/api/v1/signup` adding extra keys (`attributes.role`, `vip_level`, `is_verified`, `balance`) and observe whether they persist/alter privileges or bonus amounts vs a control signup  
verify_steps: 1. Register control account via normal form, capture full POST body. 2. Register test account adding extra body fields (role/vip/balance) — observe acceptance/rejection. 3. Test country override to a restricted territory to check territory-control bypass (softblock enforcement). 4. Test invitation signup with static refSiteToken and unverified referral email.  
impact: Medium-High — Referral bonus manipulation, territory/regulatory controls bypass, possible privilege elevation if mass assignment on role  
testability: AUTH_HELPED  
[HYP] PWS Edge SSRF to Cloud Metadata  
class: SSRF  
asset: https://www.ozoon.com  
confidence: 55  
reasoning: Server header `PWS/8.3.1.0.8` behind `wnacloud.com` CNAME (shared hosting edge). Shared cloud edges often have fetch/proxy functionality for image proxying, webhooks, PDF generation. Deep port scan shows 8080,8443,8000,8081-8083,8088,8090,8800,8888,9000,9090,9200 responding — consistent with internal metadata proxy endpoints.  
evidence_needed: Identify any parameter accepting URLs (image, callback, webhook, pdf, import, fetch, url, src, link) and test with `169.254.169.254/latest/meta-data/`  
verify_steps: GET https://www.ozoon.com/ (full header/body capture, map all params, PHPSESSID, .php extensions); GET https://www.ozoon.com/api/ (if exists); GET https://www.ozoon.com/robots.txt; GET https://www.ozoon.com/sitemap.xml; test any url=, callback=, image=, webhook=, fetch=, import=, pdf= param with `http://169.254.169.254/latest/meta-data/iam/security-credentials/`  
impact: Cloud metadata credentials → full cloud account compromise (HIGH/CRITICAL)  
testability: PASSIVE  
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header: confidence 48→50 but same asset (services.ozoon.eu) as higher-confidence BOLA hypothesis (62); verify steps require AUTH_HELPED on self-created throwaway only; keep for depth phase after BOLA confirmation  
[PARKED] Deployed PHP App SQL Injection via Reused Code: confidence 45, deployment unconfirmed (hyper/mas-film not verified on www.ozoon.com); REJECTED class OTHER per knowledge base  
[PARKED] Hardcoded Credentials Reused in Production: confidence 40 < threshold; port 3306 confirmed shared proxy EOF; REJECTED class SECRET per knowledge base  
[FINAL] 1. BOLA/IDOR on Profile-Keyed UUID API Endpoints (62) — services.ozoon.eu  
[FINAL] 2. Registration Mass Assignment / Business Logic Field Injection (55) — ozoon.eu  
[FINAL] 3. PWS Edge SSRF to Cloud Metadata (55) — www.ozoon.com  
[NEXT] PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptable fields for signup hypothesis; read-only, no auth required, maps server-side validation surface
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation  
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration  
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.  
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.  
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.  
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.  
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by profile UUID in path.  
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potential 2FA/KYC bypass backdoor.  
[RISK] ozoon-sportsbook-casino: 78 — Core gambling platform (ozoon.eu) confirmed with micro-frontend API (services.ozoon.eu) fully mapped via public SDK bundles. Three critical vectors identified: (1) BOLA on UUID-keyed profile endpoints across wallet/transactions/referral/messages (confidence 62), (2) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header (confidence 48→50), (3) SSRF-to-metadata on marketing edge (confidence 55). Test pages exposed, legacy Bodog code paths, client-side validation only on signup. F5 ASM WAF, reCAPTCHA v3, 2FA present but backend binding correctness and mock-header handling unverified. High business value (real-money gaming, crypto/Interac) + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 14:25:52 UTC [target] (model nemotron3)
[NEW] Bigpickle risk elevated to 80 (from 78) with three refined FINAL hypotheses: BOLA UUID (confidence 65↑), Mock-2FA header (55↑), Mass-assignment (60↑) — nemotron3 remains at 78
[NEW] Bigpickle next action shifted to AUTH_HELPED gateway: POST https://www.ozoon.eu/api/v1/signup (register throwaway for session cookie) vs nemotron3's passive GET profileupdateform
[CHANGED] Both models confirm zero authenticated probes executed against ozoon.eu/services.ozoon.eu despite confirmed critical attack surface; all probes to date target only www.ozoon.com marketing edge
[NEW] Bigpickle PRIO scores: services.ozoon.eu UUID endpoints 9.5, player-verification 8.5, www.ozoon.eu signup 7.5 — all AUTH_HELPED gate_ease 6-8
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public config/SDK confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) all address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled X-MOCK-2FA-VERIFICATION header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and two_factor_authenticator.allow-permanent-skip:["true"]. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with X-MOCK-2FA-VERIFICATION:true vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header X-MOCK-2FA-VERIFICATION:true + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup + /services/signup-service/v1/invitation/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on login action; signup and signup_from_invitation are captcha-free per public config. Static referral token refSiteToken (32-hex) with affiliate.check-referring-site.enabled:["false"] → referral/site field unverified. Signup accepts client-controlled attributes and address sub-objects. Profile update form schema (/services/site-config/v1/countries/US|CA/profileupdateform) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header (nemotron3 @ 48): superseded by bigpickle's refined hypothesis @ 55 with config evidence (allow-permanent-skip)
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified
[LEARN] REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth testing without service confirmation
[LEARN] ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 18:02:09 UTC [target] (model nemotron3)
## 2026-09-04 20:04:32 UTC [target] (model nemotron3)
[NEW] `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
[NEW] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
[CHANGED] Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
[NEW] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] www.ozoon.com,7.75,attack_surface=8|business_value=9|tech_exposure=6|gate_ease=10|cloud_surface=5|freshness=6
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header (nemotron3 @ 48): superseded by bigpickle's refined hypothesis @ 55 with config evidence (allow-permanent-skip)
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing.
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-04 22:19:03 UTC [target] (model nemotron3)
[NEW] `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
[NEW] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
[CHANGED] Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
[NEW] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header (nemotron3 @ 48): superseded by bigpickle's refined hypothesis @ 55 with config evidence (allow-permanent-skip)
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing.
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 00:16:05 UTC [target] (model nemotron3)
[NEW] `ozoon.eu` and `services.ozoon.eu` confirmed as core gambling platform + backend API but **still absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
[NEW] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`); all 18 probe rounds to date target only `www.ozoon.com` marketing edge
[CHANGED] Bigpickle risk elevated to 80 (from 78); nemotron3 at 78 — divergence on next action (bigpickle: active signup POST vs nemotron3: passive GET profileupdateform)
[NEW] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] 2FA/KYC Bypass via X-MOCK-2FA-VERIFICATION Header (nemotron3 @ 48): superseded by bigpickle's refined hypothesis @ 55 with config evidence (allow-permanent-skip)
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing.
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 04:44:13 UTC [target] (model nemotron3)
[NEW] Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` but knowledge base + all model leads confirm `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) are in-scope per scope.yml "All infrastructure, brands, services and gaming platforms operated by Ozoon" — both absent from inventory
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all 18 probe rounds; all probes target only `www.ozoon.com` marketing edge
[CHANGED] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
[NEW] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing.
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 08:43:38 UTC [target] (model nemotron3)
[NEW] Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` but knowledge base + all model leads confirm `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) are in-scope per scope.yml "All infrastructure, brands, services and gaming platforms operated by Ozoon" — both absent from inventory
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all 18 probe rounds; all probes target only `www.ozoon.com` marketing edge
[CHANGED] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
[NEW] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. If sid not bound to path UUID, swapping UUID yields cross-tenant data.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticated BOLA/2FA/mass-assignment testing. Body: minimal required fields from profileupdateform schema (email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode:"CA"}, attributes:{currency:"CAD"})
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing.
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[RISK] ozoon-sportsbook-casino: 80 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 12:11:58 UTC [target] (model nemotron3)
[NEW] Inventory file lists only `ozoon.com`/`www.ozoon.com` (marketing edge); `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but absent from inventory
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
[CHANGED] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
[CHANGED] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[NEW] Direct core-API probes executed this cycle: Spring Boot stack behind BFF confirmed; structured 401 `errorCode:"unauthorized"` for valid-route bogus UUID = auth pre-check precedes resource lookup; internal-only `services.services.ozoon.eu` (NXDOMAIN) = split-horizon
[CHANGED] Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
[NEW] `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
[NEW] `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[NEW] `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.10,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. Structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline recorded.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification. SSR `isMockProviderEnabled:false` is client-side, does not resolve server-side handling.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] chat.ozoon.eu: third-party chat widget on Arrays CDN; no Ozoon API logic
[PARKED] games.glovefrog.plus: jackpot third-party; ownership unconfirmed → out-of-scope
[PARKED] services.services.ozoon.eu: split-horizon/internal-only (public NXDOMAIN), not reachable
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.eu/api/v1/signup `{"email":"<throwaway>@<valid-domain>","password":"Test1234!","country":"CA","language":"en","attributes":{},"address":{"country":"CA","addressLine":"123 Test St","postalCode":"K1A 0A6"}}` → capture sid + ownPID, then baseline GET https://services.ozoon.eu/services/wallet-gateway/v1/profiles/{ownPID}/balances (expected 200 vs recorded 401 errorCode:unauthorized) to unlock the BOLA/2FA/mass-assignment differential
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing
[LEARN] ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline
[LEARN] ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
[LEARN] REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
[LEARN] REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
[LEARN] REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[RISK] ozoon-sportsbook-casino: 81/100 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 15:34:36 UTC [target] (model nemotron3)
[NEW] Inventory file (ozoon-sportsbook-casino.md) lists only `ozoon.com`/`www.ozoon.com` (marketing edge); `ozoon.eu` (core gambling platform) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but absent from inventory
[NEW] Direct core-API probes executed: Spring Boot stack behind BFF confirmed; structured 401 `errorCode:"unauthorized"` for valid-route bogus UUID = auth pre-check precedes resource lookup; internal-only `services.services.ozoon.eu` (NXDOMAIN) = split-horizon
[NEW] `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
[NEW] `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[NEW] `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
[CHANGED] Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
[CHANGED] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
[CHANGED] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.1,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] chat.ozoon.eu,2.0,attack_surface=2|business_value=1|tech_exposure=1|gate_ease=10|cloud_surface=2|freshness=3
[HYP] BOLA on Profile-Keyed UUID Endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. Structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline recorded.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment — Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification. SSR `isMockProviderEnabled:false` is client-side, does not resolve server-side handling.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] chat.ozoon.eu: third-party chat widget on Arrays CDN; no Ozoon API logic
[PARKED] games.glovefrog.plus: jackpot third-party; ownership unconfirmed → out-of-scope
[PARKED] services.services.ozoon.eu: split-horizon/internal-only (public NXDOMAIN), not reachable
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.eu/api/v1/signup `{"email":"<throwaway>@<valid-domain>","password":"Test1234!","country":"CA","language":"en","attributes":{},"address":{"country":"CA","addressLine":"123 Test St","postalCode":"K1A 0A6"}}` → capture sid + ownPID, then baseline GET https://services.ozoon.eu/services/wallet-gateway/v1/profiles/{ownPID}/balances (expected 200 vs recorded 401 errorCode:unauthorized) to unlock the BOLA/2FA/mass-assignment differential
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing
[LEARN] ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline
[LEARN] ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
[LEARN] REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
[LEARN] REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
[LEARN] REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[RISK] ozoon-sportsbook-casino: 81/100 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
## 2026-09-05 17:45:33 UTC [target] (model nemotron3)
[NEW] `ozoon.eu` (core gambling platform: sportsbook/casino/poker) and `services.ozoon.eu` (backend API) confirmed in-scope per scope.yml but **absent from inventory file** (only `ozoon.com`/`www.ozoon.com` listed)
[NEW] Direct core-API probes executed: `GET https://services.ozoon.eu/` → 743KB Next.js SSR shell; `servicesBaseUrl=https://services.services.ozoon.eu` (internal-only, NXDOMAIN publicly = split-horizon)
[NEW] Structured auth-gate: `GET /services/wallet-gateway/v1/profiles/{bogus-uuid}/balances` → `401 {"errorCode":"unauthorized"}` — auth pre-check precedes resource lookup; anonymous differential baseline recorded
[NEW] `/services/*/v{2,3}/api-docs` and `/actuator*` → gateway 404; no anonymous schema/actuator leak
[NEW] `chat.ozoon.eu` live in-scope subdomain (CNAME `ozoon.eu.glb.network`); widget-only, no Ozoon API logic
[NEW] `games.glovefrog.plus` jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[NEW] `www.ozoon.eu/api/v1`: GET `/api/v1/login` and `/api/v1/whoami` → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WAF "Request Rejected" (support_id 6279731412718049849) → WAF staged on mutating signup path only
[NEW] `/services/site-config/v1/countries/{CC}/profileupdateform`: unknown country → structured 404 leaking internal exception `io.crazy88.beatrix.siteconfig.exception.CountryNotFoundException` (GoBet/Beatrix backend fingerprint)
[NEW] `/services/site-config/v1/countries/CA/profileupdateform` returns CAD + XBT (XBT unpublished) with mandatory province/postal code; US=USD — update DTO strict but signup DTO separate
[CHANGED] Zero authenticated probes executed against core platform (`ozoon.eu`/`services.ozoon.eu`) across all probe rounds; all probes target only `www.ozoon.com` marketing edge
[CHANGED] Production SSR flag `isMockProviderEnabled:false` (client MockProvider chunk) is mild contrary evidence for mock-2FA path → confidence 60→55
[CHANGED] Three FINAL hypotheses refined with AUTH_HELPED test paths: BOLA UUID (65), Mass-assignment (60), Mock-2FA header (55) — all require authenticated session via signup gateway
[CHANGED] Bigpickle PRIO scores confirm `services.ozoon.eu` UUID endpoints (9.5) and `player-verification` (8.5) as highest-value targets, both gate_ease 6-7 (AUTH_HELPED)
[PRIO] services.ozoon.eu/services/*/v1/profiles/{uuid}/...,9.5,attack_surface=10|business_value=10|tech_exposure=8|gate_ease=7|cloud_surface=6|freshness=9
[PRIO] services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify,8.5,attack_surface=8|business_value=9|tech_exposure=7|gate_ease=6|cloud_surface=6|freshness=8
[PRIO] www.ozoon.eu/api/v1/signup,7.5,attack_surface=7|business_value=8|tech_exposure=6|gate_ease=8|cloud_surface=5|freshness=7
[PRIO] ozoon.eu,9.1,attack_surface=10|business_value=10|tech_exposure=9|gate_ease=10|cloud_surface=6|freshness=9
[PRIO] chat.ozoon.eu,2.0,attack_surface=2|business_value=1|tech_exposure=1|gate_ease=10|cloud_surface=2|freshness=3
[HYP] BOLA on Profile-Keyed UUID Endpoints Across 5 Services
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 65
reasoning: Public SDK/config confirms 5 services (wallet-gateway, transaction-group, referral, player-verification, player-messages) address sensitive resources by UUID path with session cookie (sid). Live 401 on unauthenticated probes to random/VIP UUIDs proves auth-gating exists but binding unresolved. Structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline recorded.
evidence_needed: With valid own session (sid + ownPID), GET /services/wallet-gateway/v1/profiles/{ownPID}/balances returns 200; swap PID for another UUID → 200 with foreign data = BOLA confirmed vs 401/403 = bound
verify_steps: 1. POST https://www.ozoon.eu/api/v1/signup (captcha-free) → register throwaway account. 2. Login → capture sid cookie + ownPID from response. 3. GET /services/referral/v1/profiles/{ownPID} → baseline 200. 4. GET same with consecutive/random UUIDs → compare. 5. Repeat on /wallet-gateway/balances, /transaction-group/transactions, /player-messages/messages. 6. Test write: PUT /services/profile/v1/profiles/{other-id}
impact: Critical — cross-tenant PII (names, DOB, emails, messages), financial transaction history, wallet balances, referral data; write variants → account modification
testability: AUTH_HELPED
[HYP] Registration/Referral Mass Assignment via Captcha-Free Signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 60
reasoning: reCAPTCHA v3 enforced ONLY on `login` action; `signup` and `signup_from_invitation` are captcha-free per public config. Static referral token `refSiteToken` (32-hex) with `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. Signup accepts client-controlled `attributes` and `address` sub-objects. Profile update form schema (`/services/site-config/v1/countries/US|CA/profileupdateform`) returns whitelisted fields: email, phone, addressLine, postalCode, language, timeZone, oddsFormat, subscriptionChannels — baseline for mass-assignment comparison.
evidence_needed: On self-created account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) → observe persistence vs rejection over control signup; test referral token impersonation via static refSiteToken
verify_steps: 1. Register control via POST /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare response/body. 3. Invitation signup POST with static refSiteToken + unverified referral email. 4. Test country override to restricted territory (softblock bypass).
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip
testability: AUTH_HELPED
[HYP] Mock-2FA Header Honored → Verification/KYC Bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 55
reasoning: Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on verification endpoint. Config exposes MFA ops map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`. If honored, client claims SMS/PIN/phone verification complete without actual verification. SSR `isMockProviderEnabled:false` is client-side, does not resolve server-side handling.
evidence_needed: With own session, POST /verifications/verify with `X-MOCK-2FA-VERIFICATION:true` vs absent → mocked acceptance (200) vs rejection (401/400)
verify_steps: 1. Register + login throwaway account (from HYP1/HYP2). 2. POST /services/player-verification/v1/profiles/{ownPID}/verifications/verify with header `X-MOCK-2FA-VERIFICATION:true` + minimal body. 3. Observe response. 4. Repeat without header as control. Self-created throwaway only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass)
testability: AUTH_HELPED
[PARKED] PWS Edge SSRF to Cloud Metadata (www.ozoon.com): confidence 55 but marketing edge only; no URL-accepting parameters identified in latest passive probes; lower business value than core platform vectors; keep for breadth but deprioritize
[PARKED] chat.ozoon.eu: third-party chat widget on Arrays CDN; no Ozoon API logic
[PARKED] games.glovefrog.plus: jackpot third-party; ownership unconfirmed → out-of-scope
[PARKED] services.services.ozoon.eu: split-horizon/internal-only (public NXDOMAIN), not reachable
[FINAL] 1. BOLA on Profile-Keyed UUID Endpoints (65) — services.ozoon.eu — AUTH_HELPED
[FINAL] 2. Registration/Referral Mass Assignment (60) — www.ozoon.eu/api/v1/signup — AUTH_HELPED
[FINAL] 3. Mock-2FA Header Honored → Verification Bypass (55) — services.ozoon.eu — AUTH_HELPED
[NEXT] HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.eu/api/v1/signup `{"email":"<throwaway>@<valid-domain>","password":"Test1234!","country":"CA","language":"en","attributes":{},"address":{"country":"CA","addressLine":"123 Test St","postalCode":"K1A 0A6"}}` → capture sid + ownPID, then baseline GET https://services.ozoon.eu/services/wallet-gateway/v1/profiles/{ownPID}/balances (expected 200 vs recorded 401 errorCode:unauthorized) to unlock the BOLA/2FA/mass-assignment differential
[LEARN] ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing required
[LEARN] ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip config; critical if honored
[LEARN] ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe to unlock authenticated testing
[LEARN] ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check precedes resource lookup; anonymous differential baseline
[LEARN] ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
[LEARN] ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
[LEARN] REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
[LEARN] REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
[LEARN] REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
[LEARN] REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
[LEARN] REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
[RISK] ozoon-sportsbook-casino: 81/100 — Core gambling platform (ozoon.eu) with micro-frontend API (services.ozoon.eu) fully mapped via public SDK/config. Three critical vectors with clear AUTH_HELPED test paths: (1) BOLA on UUID-keyed profile endpoints across 5 services (confidence 65), (2) Registration/referral mass assignment on captcha-free signup with unverified static referral token (confidence 60), (3) 2FA/KYC bypass via client-controlled X-MOCK-2FA-VERIFICATION header honored by allow-permanent-skip config (confidence 55). Zero authenticated probes executed; all prior probes target only www.ozoon.com marketing edge. Real-money gaming with crypto/Interac payments, F5 ASM WAF, reCAPTCHA v3 (login-only), 2FA present but backend binding correctness and mock-header handling unverified. High business value + high attack surface + low gate ease (public registration, public API config) = elevated risk.
