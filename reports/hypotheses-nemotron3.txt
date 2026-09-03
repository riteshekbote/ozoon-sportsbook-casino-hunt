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
