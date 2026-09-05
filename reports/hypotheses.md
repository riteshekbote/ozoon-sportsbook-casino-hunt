# Hypotheses (ranked)

## RANKED HYPOTHESES 2026-09-02 21:55:20 UTC

## RANKED HYPOTHESES 2026-09-03 00:00:11 UTC

## RANKED HYPOTHESES 2026-09-03 04:18:13 UTC

## RANKED HYPOTHESES 2026-09-03 09:03:56 UTC

## RANKED HYPOTHESES 2026-09-03 13:36:17 UTC

## RANKED HYPOTHESES 2026-09-03 17:24:39 UTC
- [55] www.ozoon.com: PWS Edge SSRF to Cloud Metadata (from art/lead_nemotron3.txt)
- [55] https://www.ozoon.eu: Registration Form Mass Assignment / Business Logic Bypass (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://www.ozoon.com/ — full header/body capture to map all endpoints, parameters, PHP session cookies, and identify any url=, callback=, image=, we
- NEXT(hypotheses-bigpickle.txt): PROBE: Intercept the ozoon.eu registration form submission by loading the page and examining the JavaScript bundles for API endpoint patterns. Exact action: Fet
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio

## RANKED HYPOTHESES 2026-09-03 20:08:18 UTC
- [62] https://services.ozoon.eu/services/*/v1/profiles/{sessionId}/...: BOLA/IDOR on profile-keyed UUID API endpoints (from art/lead_bigpickle.txt)
- [55] ozoon.eu: Registration Form Mass Assignment / Country Override Bypass (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://ozoon.eu/register — full header/body capture with DevTools Network recording to map registration form action, fetch/XHR endpoints to services
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://services.ozoon.eu/services/referral/v1/profiles/{candidate-uuid} — but this needs auth. Instead: GET https://www.ozoon.eu/services/site-confi
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio

## RANKED HYPOTHESES 2026-09-03 22:51:06 UTC
- [62] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA/IDOR on Profile-Keyed UUID API Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptab
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potent

## RANKED HYPOTHESES 2026-09-04 00:48:41 UTC
- [62] https://www.ozoon.eu/api/v1/signup: Registration/referral mass assignment, captcha-free signup (from art/lead_bigpickle.txt)
- [62] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA/IDOR on Profile-Keyed UUID API Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/CA/profileupdateform (already captured this pass: CAD + XBT currencies, mandatory province/pos
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptab
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potent

## RANKED HYPOTHESES 2026-09-04 05:13:58 UTC
- [62] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA/IDOR on Profile-Keyed UUID API Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptab
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potent

## RANKED HYPOTHESES 2026-09-04 09:50:28 UTC
- [65] services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session not bound to path UUID (from art/lead_bigpickle.txt)
- [62] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA/IDOR on Profile-Keyed UUID API Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: GET https://www.ozoon.eu/services/site-config/v1/countries/US/profileupdateform — public profile update form schema to enumerate mass-assignment-acceptab
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (no captcha) to capture session cookie for authenticated BOLA/2FA testing. Body: min
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: ACCEPTED BUSLOGIC @ ozoon.eu: Registration form with referral email field, country override, and client-side rendering suggests server-side validation is the on
- LEARN: ACCEPTED IDOR @ services.ozoon.eu: Backend API domain confirmed in HTML source. Micro-frontend architecture means API calls are likely routed through this domai
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Specific UUID-path BOLA pattern confirmed via SDK bundles; all sensitive resources keyed by
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/...: X-MOCK-2FA-VERIFICATION header present in production SDK on verification endpoint; potent
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified.

## RANKED HYPOTHESES 2026-09-04 14:26:01 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- [55] https://www.ozoon.eu/api/v1/signup: Territory softblock bypass via country field manipulation during signup (from art/lead_bigpickle.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (no captcha) to capture session cookie for authenticated BOLA/2FA testing. Body: min
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 — port 3306 response likely shared proxy EOF pattern (identical port set across hosts), not confirmed real MySQL; no auth t
- LEARN: ACCEPTED SSRF @ www.ozoon.com — PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film (reposcan): Hardcoded MySQL credentials in GitHub repos are in learning/demo repos, not confirmed deployed to ozoo
- LEARN: REJECTED OTHER @ oZoon/secure-query-string (reposcan): SQL injection patterns in learning repos. Not confirmed as deployed code. Requires deployment confirmatio
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified.

## RANKED HYPOTHESES 2026-09-04 18:02:21 UTC
- [60] https://services.ozoon.eu/services/player-verification/v1/profiles/{ownPID}/verifications/verify: Mock-2FA verification header honored in production — client-controlled bypass of SMS/PIN/KYC gates (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.

## RANKED HYPOTHESES 2026-09-04 20:04:44 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- [60] https://services.ozoon.eu/services/player-verification/v1/profiles/{ownPID}/verifications/verify: Mock-2FA verification header honored in production — client-controlled bypass of SMS/PIN/KYC gates (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)

## RANKED HYPOTHESES 2026-09-04 22:19:14 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- [60] https://services.ozoon.eu/services/player-verification/v1/profiles/{ownPID}/verifications/verify: Mock-2FA verification header may be honored in production — client-controlled 2FA/KYC gate bypass (from art/lead_bigpickle.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Obtain explicit program authorization (bugs.olivermaicher.eu) to create two throwaway accounts (synthetic emails) on ozoon.eu, unlocking the AUTH_HELPED 
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: mock-2FA header + allow-permanent-skip config form a sing
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA surface across 5 services confirmed via SDK bundles; session↔UUID binding unresolved; 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: captcha-free signup with client-controlled attributes/address; static referral token; gateway to all authenticat
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud shared edge is plausible SSRF param class; passive GET only; deprioritized vs core platform.
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)

## RANKED HYPOTHESES 2026-09-05 00:16:35 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)

## RANKED HYPOTHESES 2026-09-05 04:46:32 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: mock-2FA header + allow-permanent-skip config form a sing
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA surface across 5 services confirmed via SDK bundles; session↔UUID binding unresolved; 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: captcha-free signup with client-controlled attributes/address; static referral token; gateway to all authenticat
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud shared edge is plausible SSRF param class; passive GET only; deprioritized vs core platform.
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: mock-2FA header + allow-permanent-skip config form a sing
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA surface across 5 services confirmed via SDK bundles; session↔UUID binding unresolved; 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: captcha-free signup with client-controlled attributes/address; static referral token; gateway to all authenticat
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud shared edge is plausible SSRF param class; passive GET only; deprioritized vs core platform.
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable.
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak.
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic.
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified.
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: mock-2FA header + allow-permanent-skip = critical gate-by
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA surface across 5 services; binding unresolved; AUTH_HELPED.
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: captcha-free signup, client-controlled attributes/address, static referral token; gateway probe.
- LEARN: REJECTED SECRET @ www.ozoon.com:3306 / [LEARN] REJECTED SECRET @ oZoon/hyper, oZoon/mas-film / [LEARN] REJECTED OTHER @ oZoon/secure-query-string: unchanged sha
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge plausible SSRF param class; passive GET only; deprioritized.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)

## RANKED HYPOTHESES 2026-09-05 08:43:46 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session identity may not be bound to path UUID (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- NEXT(hypotheses-nemotron3.txt): PROBE: POST https://www.ozoon.eu/api/v1/signup — register throwaway account (captcha-free per config) to capture session cookie (sid) and ownPID for authenticat
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform).
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable.
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak.
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: Port 3306 response likely shared proxy EOF pattern; no auth testing without service confirmation.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: Hardcoded MySQL credentials in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)

## RANKED HYPOTHESES 2026-09-05 12:12:10 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session identity may not be bound to path UUID (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, drive the signup from a real browser p
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: unknown country → structured 404 leaking internal exception `io
- LEARN: ACCEPTED MISCONFIG @ www.ozoon.eu/api/v1: GET /api/v1/login and /api/v1/whoami → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WA
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat (email,phone/addre
- LEARN: ACCEPTED OTHER @ www.ozoon.eu: homepage SSR JSON references api.wicket-keeper.com as apmRum.url — third-party RUM/APM endpoint, ownership unconfirmed; off-scope
- LEARN: REJECTED OTHER @ www.ozoon.eu/robots.txt: only sitemap_index.xml + disallow: (all) — known-public-file class, no findings.
- LEARN: REJECTED OTHER @ services.ozoon.eu/services/site-config/v1/countries: bare /countries and unknown-country /countries/XX/profiles are descriptive-error 404 only 
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified

## RANKED HYPOTHESES 2026-09-05 15:34:45 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints (session identity may not be bound to path UUID) (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints (from art/lead_nemotron3.txt)
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified

## RANKED HYPOTHESES 2026-09-05 17:45:43 UTC
- [65] https://services.ozoon.eu/services/player-verification/v1/profiles/{ownPID}/verifications/verify: Mock-2FA verification header may be honored in production — client-controlled 2FA/KYC gate bypass (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints Across 5 Services (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic.
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform).
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed.
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable.
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak.
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified.
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: unknown country → structured 404 leaking internal exception `io
- LEARN: ACCEPTED MISCONFIG @ www.ozoon.eu/api/v1: GET /api/v1/login and /api/v1/whoami → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WA
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat (email,phone/addre
- LEARN: ACCEPTED OTHER @ www.ozoon.eu: homepage SSR JSON references api.wicket-keeper.com as apmRum.url — third-party RUM/APM endpoint, ownership unconfirmed; off-scope
- LEARN: REJECTED OTHER @ www.ozoon.eu/robots.txt: only sitemap_index.xml + disallow: (all) — known-public-file class, no findings.
- LEARN: REJECTED OTHER @ services.ozoon.eu/services/site-config/v1/countries: bare /countries and unknown-country /countries/XX/profiles are descriptive-error 404 only 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1/logout: GET → structured Spring 405 Method Not Allowed (real POST-only route, exact JSON {"status":405,"error":"Method Not 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1 + services.ozoon.eu/services: passive GET breadth (oauth/authorize/connect/password-forgot/reset/verify-email/session on ww
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified

## RANKED HYPOTHESES 2026-09-05 19:37:04 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session identity may not be bound to path UUID (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints Across 5 Services (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token; gateway to all authenticat
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL.
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos; not confirmed deployed.
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname; NXDOMAIN publicly = split-horizon, not externally reachable.
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak.
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat; CA=CAD+XBT, US=US
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: unknown country → structured 404 leaking internal exception `io
- LEARN: ACCEPTED MISCONFIG @ www.ozoon.eu/api/v1: GET /api/v1/login and /api/v1/whoami → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WA
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat (email,phone/addre
- LEARN: ACCEPTED OTHER @ www.ozoon.eu: homepage SSR JSON references api.wicket-keeper.com as apmRum.url — third-party RUM/APM endpoint, ownership unconfirmed; off-scope
- LEARN: REJECTED OTHER @ www.ozoon.eu/robots.txt: only sitemap_index.xml + disallow: (all) — known-public-file class, no findings
- LEARN: REJECTED OTHER @ services.ozoon.eu/services/site-config/v1/countries: bare /countries and unknown-country /countries/XX/profiles are descriptive-error 404 only 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1/logout: GET → structured Spring 405 Method Not Allowed (real POST-only route, exact JSON {"status":405,"error":"Method Not 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1 + services.ozoon.eu/services: passive GET breadth (oauth/authorize/connect/password-forgot/reset/verify-email/session on ww

## RANKED HYPOTHESES 2026-09-05 21:47:51 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session identity may not be bound to path UUID (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints Across 5 Services (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu — the single unlock for all three FINAL vectors. If
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: REJECTED OTHER @ services.ozoon.eu SSR: `services/b2b/api/v1` is jackpotApiBase to games.glovefrog.plus (bovada.lv brand block) — third-party, off-scope, same g
- LEARN: REJECTED OTHER @ www.ozoon.eu/contents/archat: embeds Bodog live-chat iframe endpoints (livechat/nchat/nchat2/prod.chat-bodog.eu); chat-widget class, no Ozoon A
- LEARN: REJECTED OTHER @ www.ozoon.eu sitemap: specialty-games-old2/table-games-old → 308→200 redirect to current casino pages; no legacy app wiring; contents/{usms,uns
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Reconfirmed — no newly-probed anonymous surface displaces the SDK-confirmed BOLA vector; bi
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: mock-2FA + allow-permanent-skip remains the single critic
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: captcha-free signup still the sole gateway to authenticated testing; AUTH_HELPED.
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: unknown country → structured 404 leaking internal exception `io
- LEARN: ACCEPTED MISCONFIG @ www.ozoon.eu/api/v1: GET /api/v1/login and /api/v1/whoami → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WA
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat (email,phone/addre
- LEARN: ACCEPTED OTHER @ www.ozoon.eu: homepage SSR JSON references api.wicket-keeper.com as apmRum.url — third-party RUM/APM endpoint, ownership unconfirmed; off-scope
- LEARN: REJECTED OTHER @ www.ozoon.eu/robots.txt: only sitemap_index.xml + disallow: (all) — known-public-file class, no findings
- LEARN: REJECTED OTHER @ services.ozoon.eu/services/site-config/v1/countries: bare /countries and unknown-country /countries/XX/profiles are descriptive-error 404 only 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1/logout: GET → structured Spring 405 Method Not Allowed (real POST-only route, exact JSON {"status":405,"error":"Method Not 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1 + services.ozoon.eu/services: passive GET breadth (oauth/authorize/connect/password-forgot/reset/verify-email/session on ww

## RANKED HYPOTHESES 2026-09-05 23:48:13 UTC
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on profile-keyed UUID endpoints — session identity may not be bound to path UUID (from art/lead_bigpickle.txt)
- [65] https://services.ozoon.eu/services/*/v1/profiles/{uuid}/...: BOLA on Profile-Keyed UUID Endpoints Across 5 Services (from art/lead_nemotron3.txt)
- NEXT(hypotheses-bigpickle.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account. If granted, fire exactly: POST https://www.ozoon.eu/api/v1/signup 
- NEXT(hypotheses-nemotron3.txt): HUMAN: Request program authorization (bugs.olivermaicher.eu) to create one throwaway account on www.ozoon.eu. If granted, fire exactly: POST https://www.ozoon.e
- LEARN: ACCEPTED IDOR @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: Confirmed UUID-path BOLA surface across 5 services; binding unresolved; AUTH_HELPED testing
- LEARN: ACCEPTED AUTH @ services.ozoon.eu/services/player-verification/v1/profiles/{sid}/verifications/verify: Mock-2FA header in production SDK + allow-permanent-skip 
- LEARN: ACCEPTED BUSLOGIC @ www.ozoon.eu/api/v1/signup: Captcha-free signup with client-controlled attributes/address; static referral token unverified. Gateway probe t
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/*/v1/profiles/{uuid}/...: structured 401 errorCode:"unauthorized" for valid-route bogus UUID = auth pre-check pr
- LEARN: ACCEPTED MISCONFIG @ chat.ozoon.eu: live in-scope subdomain (CNAME ozoon.eu.glb.network); widget-only, no Ozoon API logic
- LEARN: ACCEPTED SSRF @ www.ozoon.com: PWS/wnacloud edge is high-value SSRF target class; warrants parameter enumeration (deprioritized vs core platform)
- LEARN: REJECTED SECRET @ www.ozoon.com:3306: identical port-closed/EOF pattern across hosts = shared proxy artifact, not MySQL
- LEARN: REJECTED SECRET @ oZoon/hyper, oZoon/mas-film: creds in learning/demo repos, not confirmed deployed
- LEARN: REJECTED OTHER @ oZoon/secure-query-string: SQLi patterns in learning repos, not confirmed deployed
- LEARN: REJECTED OTHER @ services.services.ozoon.eu: internal-only backend hostname in servicesBaseUrl; NXDOMAIN publicly = split-horizon, not externally reachable
- LEARN: REJECTED OTHER @ services.ozoon.eu: /services/*/v{2,3}/api-docs and /actuator* → gateway 404; no anonymous schema/actuator leak
- LEARN: REJECTED OTHER @ games.glovefrog.plus: jackpotApiBase third-party; Ozoon ownership unconfirmed; off-scope until verified
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: unknown country → structured 404 leaking internal exception `io
- LEARN: ACCEPTED MISCONFIG @ www.ozoon.eu/api/v1: GET /api/v1/login and /api/v1/whoami → Spring Boot Whitelabel 404 JSON (no method guard, 404 not 405); signup GET → WA
- LEARN: ACCEPTED MISCONFIG @ services.ozoon.eu/services/site-config/v1/countries/{CC}/profileupdateform: per-country profile-update whitelist is flat (email,phone/addre
- LEARN: ACCEPTED OTHER @ www.ozoon.eu: homepage SSR JSON references api.wicket-keeper.com as apmRum.url — third-party RUM/APM endpoint, ownership unconfirmed; off-scope
- LEARN: REJECTED OTHER @ www.ozoon.eu/robots.txt: only sitemap_index.xml + disallow: (all) — known-public-file class, no findings
- LEARN: REJECTED OTHER @ services.ozoon.eu/services/site-config/v1/countries: bare /countries and unknown-country /countries/XX/profiles are descriptive-error 404 only 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1/logout: GET → structured Spring 405 Method Not Allowed (real POST-only route, exact JSON {"status":405,"error":"Method Not 
- LEARN: REJECTED OTHER @ www.ozoon.eu/api/v1 + services.ozoon.eu/services: passive GET breadth (oauth/authorize/connect/password-forgot/reset/verify-email/session on ww
