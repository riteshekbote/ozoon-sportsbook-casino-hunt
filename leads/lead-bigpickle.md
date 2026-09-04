## 2026-09-03 17:19:37 UTC [target] (model bigpickle)
[NEW] `ozoon.eu` — actual gambling platform (sportsbook/casino/poker), NOT in inventory. Discovered via web search; ozoon.com is marketing-only.
[NEW] `services.ozoon.eu` — backend service domain found in HTML `<link rel="preconnect">` hints on ozoon.eu.
[NEW] 3 test pages exposed in ozoon.eu sitemap: `/promotions-test` (live, empty), `/blackjack-main-test` (404), `/casino/how-to-play-casino/odds-test` (404).
[NEW] Legacy "Bodog" branding at `/contents/chatbox-psat` — customer survey still references old brand name.
[NEW] ozoon.com runs Next.js on PWS/8.3.1.0.8 with CNAME to `wnacloud.com`; ozoon.eu uses Next.js + micro-frontend architecture (React 19 + TanStack Query shared runtime).
[PRIO] ozoon.eu, 9.0, attack_surface=10, business_value=10, tech_exposure=7 (Next.js micro-frontend, PWA, registration/login/auth flows, crypto payments), gate_ease=6 (registration form exposed, no auth required for browsing), cloud_surface=5, freshness=10.
[PRIO] services.ozoon.eu, 7.5, attack_surface=8, business_value=9, tech_exposure=8 (backend API service), gate_ease=4 (requires auth), cloud_surface=6, freshness=10.
[PRIO] www.ozoon.com, 3.5, attack_surface=4, business_value=3, tech_exposure=3 (WordPress+Next.js marketing), gate_ease=9 (fully public), cloud_surface=3, freshness=5.
[HYP] Registration Form Mass Assignment / Business Logic Bypass
class: BUSLOGIC
asset: https://www.ozoon.eu (registration form)
confidence: 55
reasoning: Registration form exposes fields: First Name, Last Name, DOB, Mobile, Email, Password, Country, ZIP, Referral Email. Country defaults to "United States" but platform is Canada-focused. No visible CAPTCHA on registration (reCAPTCHA badge present but may not enforce). Referral email field could be abused for referral bonus manipulation. The form is client-side rendered via Next.js micro-frontend, meaning server-side validation is the only control.
evidence_needed: Intercept registration POST to identify server-side field validation; test if extra fields (e.g., role, balance, vip_level) are accepted via mass assignment.
verify_steps: 1. Use browser devtools to capture the registration form submission request (POST endpoint, headers, body). 2. Test if adding extra fields (e.g., `balance`, `role`, `admin`) in the POST body are accepted. 3. Test if referral email field accepts any email without verification.
impact: Medium — Potential for referral bonus abuse, privilege escalation if mass assignment accepted, or account creation with elevated permissions.
testability: AUTH_HELPED
[HYP] services.ozoon.eu API Endpoint Discovery & IDOR/BOLA
class: IDOR
asset: https://services.ozoon.eu
confidence: 50
reasoning: `services.ozoon.eu` is explicitly referenced as a preconnect/dns-prefetch target in the HTML source, indicating it serves backend API requests. The micro-frontend architecture (React 19 + TanStack Query) likely communicates with this domain for sports data, user accounts, betting operations, and financial transactions. API endpoints are not exposed in sitemap or robots.txt. Standard gambling platform API patterns include /api/v1/user, /api/v1/bets, /api/v1/deposits, etc.
evidence_needed: Discover actual API paths on services.ozoon.eu by analyzing application JS bundles or intercepting network requests during authenticated session.
verify_steps: 1. Load ozoon.eu in browser with network monitoring, perform login, navigate sports/casino sections. 2. Capture all XHR/fetch requests to identify API endpoint patterns. 3. Test common gambling API paths: /api/user, /api/v1/account, /api/v1/bets, /api/v1/deposits, /api/v1/withdrawals. 4. For any discovered endpoints with numeric IDs, test incrementing/decrementing IDs to check for IDOR.
impact: High — If IDOR exists on user accounts, betting history, or financial data, attacker could access other users' PII, bet history, or initiate unauthorized transactions.
testability: AUTH_HELPED
[HYP] JWT/Session Token Leakage via Shared Runtime Global
class: AUTH
asset: https://www.ozoon.eu (client-side)
confidence: 45
reasoning: The platform exposes `window.__SHARED_RUNTIME__` as a global object containing the React 19 + TanStack Query infrastructure. TanStack Query manages server state including auth tokens. If the QueryClient is configured to persist tokens in the global state or if the HydrationBoundary preloads sensitive data, tokens could be accessible via browser console. The micro-frontend architecture means multiple independent bundles share this runtime, increasing the surface for accidental token exposure.
evidence_needed: Inspect `window.__SHARED_RUNTIME__` during authenticated session to check if QueryClient contains auth tokens, session data, or user PII in its cache.
verify_steps: 1. Log in to ozoon.eu. 2. Open browser console and inspect `window.__SHARED_RUNTIME__` for any QueryClient instance. 3. Check if `queryClient.getQueryCache()` contains queries with auth tokens or user data. 4. Check localStorage/sessionStorage for any persisted auth state.
impact: Medium — Session token leakage would allow account takeover.
testability: PASSIVE
[PARKED] JWT/Session Token Leakage via Shared Runtime Global: confidence 45 (borderline), testability PASSIVE only. Reason: `window.__SHARED_RUNTIME__` analysis showed it's a clean library bundle without application state. Auth tokens are more likely in HttpOnly cookies or separate storage. Park until authenticated testing can confirm.
[FINAL] survivors ranked:
[NEXT] PROBE: Intercept the ozoon.eu registration form submission by loading the page and examining the JavaScript bundles for API endpoint patterns. Exact action: Fetch `https://www.ozoon.eu/assets/_next/static/chunks/app/%5Blocale%5D/layout-df512a1a581be543.js` and `https://www.ozoon.eu/assets/_next/static/chunks/pantheon-modules-54ad3b04423ff20f.js` to find the registration/auth API endpoint URLs used by the frontend.
[LEARN] ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the only defense. Mass assignment testing requires authenticated interception.
[LEARN] ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domain. Discovery requires browser-based interception.
[LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoon.com/ozoon.eu infrastructure. Low confidence without deployment confirmation.
[LEARN] REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmation.
[RISK] ozoon-sportsbook-casino: 65/100. Reason: Platform is a real-money gambling site with crypto payments, 2FA, and reCAPTCHA. Surface is partially mapped (ozoon.com marketing + ozoon.eu platform). Backend API (services.ozoon.eu) is uncharted. Reposcan findings from Ozoon GitHub org are in learning repos, not confirmed deployed. High business value but moderate attack difficulty due to security controls (reCAPTCHA, 2FA, SSL).
## 2026-09-03 20:08:10 UTC [target] (model bigpickle)
[HYP] BOLA/IDOR on profile-keyed UUID API endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{sessionId}/...
confidence: 62
reasoning: Every sensitive resource (referral, messages, transactions, wallet balances, profile, rewards) is addressed by a UUID in the URL path: `/services/profile/v1/profiles/{sid}`, `/services/wallet-gateway/v1/profiles/{pid}/balances`, `/services/transaction-group/v1/profiles/{sid}/transactions`. All confirmed real (401 unauthenticated). The reCaptcha action map confirms these exact patterns. If server authenticates via `sid` cookie but does not bind it to the path UUID, altering the path UUID yields cross-account read/write.
evidence_needed: With a valid own session, issue GET to own `/services/wallet-gateway/v1/profiles/{ownPID}/balances` then swap {ownPID} for another profile UUID; observe 200 with foreign data vs 401/403 (bound) — distinguishes BOLA vs proper binding.
verify_steps: 1. Login (AUTH_HELPED) to capture own sessionId cookie. 2. GET `/services/referral/v1/profiles/{own-id}` → capture baseline. 3. GET same with incremented/decremented consecutive UUIDs and random other UUIDs; compare 200 (BOLA confirmed) vs 403/401 (bound). 4. Repeat on `/transactions`, `/player-messages`, `/balances`.
impact: High — Cross-tenant PII dump (names, DOB, messages, emails), financial transaction history, referral data; write variants (profile PUT, message PATCH) → account modification.
testability: AUTH_HELPED
[HYP] 2FA bypass via X-MOCK-2FA-VERIFICATION control header
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/...
confidence: 48
reasoning: The production SDK sends an `X-MOCK-2FA-VERIFICATION` header (value as boolean-string) on `/services/player-verification/v1/profiles/{sid}/verifications/verify`. Presence of a client-controlled "mock verification" header in a money-platform verification path indicates a test backdoor that, if honored with a valid session, trusts the client to claim 2FA/phone/email verification was completed.
evidence_needed: With own session, POST the `/verifications/verify` endpoint with `X-MOCK-2FA-VERIFICATION: true` vs `false` and an unverified attribute; observe whether mocked verification is accepted (bypass) vs rejected.
verify_steps: 1. Login, obtain session. 2. Call own `/services/player-verification/v1/profiles/{sid}/verifications/verify` with `X-MOCK-2FA-VERIFICATION: true` (documented mutation) and inspect status/result. 3. Compare to header absent. Only on a self-created throwaway account.
impact: Critical if honored — bypass 2FA/phone/PIN/KYC verification gates on a withdrawal/bonus flow (money security control bypass).
testability: AUTH_HELPED
[HYP] Registration mass assignment / business-logic field injection
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup
confidence: 55
reasoning: `/api/v1/signup` POST accepts `{email, password, username, firstName, lastName, phone, dateOfBirth, address:{countryCode}, attributes:{currency}}`. The `attributes` and `address` sub-objects are client-controlled. Country default in form is "United States" while platform is Canada/softblocked; country has business-logic value (bonus eligibility, territory controls). Server-side validation is the only control (client-side rendering confirmed). Referral field `referringSiteToken` flows into invitation signup.
evidence_needed: On a throwaway account, POST `/api/v1/signup` adding extra keys (`attributes.role`, `vip_level`, `is_verified`, `balance`) and observe whether they persist/alter privileges or bonus amounts vs a control signup.
verify_steps: 1. Register control account via normal form, capture full POST body. 2. Register test account adding extra body fields (role/vip/balance) — observe acceptance/rejection. 3. Test country override to a restricted territory to check territory-control bypass (softblock enforcement).
impact: Medium-High — Referral bonus manipulation, territory/regulatory controls bypass, possible privilege elevation if mass assignment on role.
testability: AUTH_HELPED
[NEXT] PROBE: GET https://services.ozoon.eu/services/referral/v1/profiles/{candidate-uuid} — but this needs auth. Instead: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform (public profile update form schema) to enumerate mass-assignment-acceptable fields for the signup hypothesis, read-only.
[RISK] ozoon-sportsbook-casino: 72/100. Raison: Full backend API surface now mapped via public config + SDK bundles; BOLA/mass-assignment/MFA-bypass vectors identified on a real-money platform with crypto + Interac payments. Mitigations present but notable (F5 ASM `TS` WAF cookies, 401 enforcement, reCAPTCHA v3, 2FA). Backend binding correctness (session↔path-UUID) and mock-2FA header handling are unresolved and could be critical. Verification gated behind authenticated testing (AUTH_HELPED).
## 2026-09-03 22:50:55 UTC [target] (model bigpickle)
## 2026-09-04 00:36:44 UTC [target] (model bigpickle)
[HYP] Registration/referral mass assignment, captcha-free signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup + /services/signup-service/v1/invitation/signup
confidence: 62
reasoning: Public config confirms reCAPTCHA v3 enforced only on `login`, NOT on `signup` or `signup_from_invitation`; signup action is POST /api/v1/signup. Static referral token `refSiteToken` (=32hex) present and `affiliate.check-referring-site.enabled:["false"]`, so the referral/site field is unverified. `/services/site-config/v1/countries/US/profileupdateform` returns the whitelisted profile-update fields (email/phone/addressLine/postalCode/language/timeZone/oddsFormat/subscriptionChannels), giving a baseline of server-accepted mass-assignment fields.
evidence_needed: On a throwaway self-created account, POST signup adding extra body keys (balance/role/vip/is_verified/currency override) and observe persistence vs rejection against a control signup; test referral token impersonation via static refSiteToken.
verify_steps: 1. Register control via /api/v1/signup (no captcha token required). 2. Register test adding attributes.role/vip_level/balance — compare accepted fields. 3. Craft invitation signup POST with static refSiteToken=sha256... and unverified referral email.
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation if role/vip mass-assigned.
testability: AUTH_HELPED (self-created account OK)
[HYP] BOLA on referral/profile UUID endpoints — still top
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 62
reasoning: Confirmed from public config reCAPTCHA action map that profile-keyed GETs (/referral/v1/profiles/{uuid}, /profile_settings, /get_transactions, /messages) all key sensitive resources by UUID path with session cookie; live 401 when unauthenticated (my probes to random + VIP UUIDs). If session (sid) is not bound to path UUID, swapping UUID yields cross-tenant PII. Not circumvented by captcha absence (still auth-gated).
evidence_needed: With own session, GET own /services/wallet-gateway/v1/profiles/{ownPID}/balances then swap PID for another — 200 foreign data = BOLA vs 401/403 = bound.
impact: High — cross-tenant PII, transaction history, referral data, financial balances.
testability: AUTH_HELPED
[HYP] Mock-2FA header honored → verification bypass (now supported by MFA map)
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 50
reasoning: Config confirms the MFA operations map (validatePhone:CONFIRMATION_CODE:SMS, withdraw:CONFIRMATION_CODE:SMS, enable2fa:PIN_CODE) and `two_factor_authenticator.allow-permanent-skip:["true"]`; the production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION` header on the verification endpoint. If honored, client can claim SMS/PIN/phone verification complete.
evidence_needed: With own session, POST verify with X-MOCK-2FA-VERIFICATION:true vs absent — mocked verification accepted vs rejected.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus.
testability: AUTH_HELPED
[HYP] BOLA/IDOR on profile-keyed UUID API endpoints
class: IDOR
asset: https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...
confidence: 62
reasoning: Public reCAPTCHA action map confirms profile-UUID-path GETs (referral, profile_settings, get_transactions, get_messages) are keyed by UUID with session cookie. Live probes to zero + real VIP UUID return 401 unauthenticated, so binding is unresolved. If server authenticates via sid but fails to bind sid to path UUID, swapping UUID yields cross-tenant PII. Captcha is login-only and irrelevant here (still auth-gated).
evidence_needed: With own session, GET own `/services/wallet-gateway/v1/profiles/{ownPID}/balances` then swap for another profile UUID → 200 foreign data (BOLA) vs 401/403 (bound).
verify_steps: 1. Login (AUTH_HELPED) to capture own sid. 2. GET own `/services/referral/v1/profiles/{own-id}` baseline. 3. GET same with consecutive/random other UUIDs; 200→BOLA, 403/401→bound. 4. Repeat on /transactions, /player-messages, /balances.
impact: High — cross-tenant PII (names, DOB, email), transaction history, financial balances, referral data; write variants → account modification.
testability: AUTH_HELPED
[HYP] Registration/referral mass assignment, captcha-free signup
class: BUSLOGIC
asset: https://www.ozoon.eu/api/v1/signup + /services/signup-service/v1/invitation/signup
confidence: 62
reasoning: Config confirms reCAPTCHA enforced only on `login`; `/api/v1/signup` (POST) and `signup_from_invitation` (POST) are captcha-free. Static referral `refSiteToken` present and `affiliate.check-referring-site.enabled:["false"]` → referral/site field unverified. `/services/site-config/v1/countries/US|CA/profileupdateform` returns whitelisted update fields (email,phone,addressLine,postalCode,language,timeZone,oddsFormat,subscriptionChannels), giving the server-accepted field baseline for mass-assignment comparison.
evidence_needed: On a self-created throwaway account, POST signup adding extra keys (attributes.role, vip_level, is_verified, balance, currency override) and observe persistence vs rejection over a control signup; test territory softblock override and referral token reuse.
verify_steps: 1. Register control via /api/v1/signup (no captcha). 2. Register test adding role/vip/balance fields — compare. 3. Invitation signup POST with static refSiteToken (a0b5...b084, sha256 7f2c...) + unverified referral email.
impact: Medium-High — referral bonus fraud, territory/regulatory softblock bypass, possible privilege elevation on mass-assigned role/vip.
testability: AUTH_HELPED
[HYP] Mock-2FA header honored → verification/2FA bypass
class: AUTH
asset: https://services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify
confidence: 50
reasoning: Config exposes the MFA ops map (`validatePhone:CONFIRMATION_CODE:SMS`, `withdraw:CONFIRMATION_CODE:SMS`, `enable2fa:PIN_CODE`) and `two_factor_authenticator.allow-permanent-skip:["true"]`. Production SDK sends client-controlled `X-MOCK-2FA-VERIFICATION:true` on the verify endpoint. If honored, client can claim SMS/PIN/phone verification complete on a money flow.
evidence_needed: With own session, POST verify with `X-MOCK-2FA-VERIFICATION:true` vs absent on a self-created account — mocked accept vs reject.
verify_steps: 1. Login, get sid. 2. POST /verifications/verify with header true + unverified attribute; inspect result. 3. Repeat header absent; compare. Self-created throwaway account only.
impact: Critical if honored — bypass 2FA/SMS/PIN/KYC gates on withdraw/deposit/bonus (money security control bypass).
testability: AUTH_HELPED
[NEXT] PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/CA/profileupdateform (already captured this pass: CAD + XBT currencies, mandatory province/postal) — done. Next needed probe is auth-gated, so instead: GET https://www.ozoon.eu/services/site-config/v1/countries/DE/profileupdateform and /countries/IN/profileupdateform to compare restricted vs unrestricted territory form schemas and confirm whether softblock only affects registration or also field surface (read-only contour of the regulatory bypass).
[RISK] ozoon-sportsbook-casino: 78/100. Raison: Full backend API surface now mapped via an unauthenticated config endpoint including exact endpoint/verb map and captcha posture (login-only). Real-money + crypto + Interac platform with 2FA permanent-skip enabled, static referral token, softblock territory controls, and live 401-enforced profile-keyed BOLA surface. BOLA/mass-assignment/mock-2FA all unresolved and gated on authenticated testing (AUTH_HELPED); recon depth and money-bearing materiality are high.
