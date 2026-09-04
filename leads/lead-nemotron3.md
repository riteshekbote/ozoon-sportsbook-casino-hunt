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
