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
