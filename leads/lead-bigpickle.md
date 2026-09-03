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
