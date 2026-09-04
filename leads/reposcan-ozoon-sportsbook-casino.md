## REPOSCAN 2026-09-03 16:33:17 UTC
[HYP] Hardcoded MySQL Credentials - hyper repo
class: SECRET
asset: oZoon/hyper/core/config.php:17
confidence: 85
reasoning: Hardcoded MySQL credentials: user='hyper', password='12345', database='hyper'. Also documented in mysql.txt with same weak password and a test user (hyper01/123456789).
impact: Medium - If deployed to production, database compromise. Credentials are weak and trivially guessable.
verify_steps: 1. Check if any ozoon.com subdomain runs this PHP code 2. Test if MySQL port 3306 is exposed on ozoon.com hosts 3. Attempt login with discovered credentials if service is live
[HYP] Hardcoded MySQL Credentials - mas-film repo
class: SECRET
asset: oZoon/mas-film/core/config.php:13
confidence: 80
reasoning: Hardcoded MySQL credentials: user='masha', password='12345', database='masha'. Same weak password pattern as hyper repo.
impact: Medium - If deployed, database compromise via weak credentials.
verify_steps: 1. Check if any ozoon.com subdomain hosts this film database app 2. Scan for MySQL services on discovered hosts
[HYP] Exposed Unsplash API Keys - diploma-try repo
class: SECRET
asset: oZoon/diploma-try/src/js/lib/constants.js:13-14
confidence: 90
reasoning: Hardcoded Unsplash API Access Key and Secret committed to public repo: ACCESS_KEY='KVx67XvmzAv0NWFzGhl02RT3YJ0kXfNhhffCmc6V2Vk', SECRET='NEbVoZN0xAL1MJkl9GCIfHmud75H71MjACB2fo0UdiU'. These are real API credentials, not placeholders.
impact: Low - Unsplash API abuse, rate limit exhaustion, potential for unauthorized photo operations if key has write permissions.
verify_steps: 1. Test if API key is still active by making a read request to Unsplash API 2. Check if key has write permissions 3. Verify these keys aren't reused in any Ozoon production systems
[HYP] Weak Hardcoded Auth Credentials - php-learn-5 repo
class: SECRET
asset: oZoon/php-learn-5/include/helpers.php:46-52
confidence: 70
reasoning: Hardcoded authentication array with trivial passwords: 1@bk.ru/1, 2@bk.ru/2, etc. Passwords match the numeric portion of email addresses.
impact: Low - Test/learning credentials only, but pattern indicates weak security practices.
verify_steps: 1. Check if this app is deployed anywhere 2. Test if any ozoon.com subdomain uses similar auth patterns
[HYP] SQL Injection Vulnerabilities - multiple PHP repos
class: OTHER
asset: oZoon/secure-query-string/settings.php, oZoon/hyper/core/functions.php
confidence: 85
reasoning: All SQL queries use string concatenation with user input without parameterized queries. Examples: settings.php:44, functions.php:142,165,180,373,402,434,484
impact: Medium - SQL injection could lead to data exfiltration, authentication bypass, or remote code execution if deployed.
verify_steps: 1. Identify if any ozoon.com endpoints run this code 2. Test for SQL injection on any discovered PHP endpoints
[HYP] Debug Mode Enabled in Production Config - hyper, mas-film repos
class: MISCONFIG
asset: oZoon/hyper/core/config.php:4-5, oZoon/mas-film/core/config.php:4-5
confidence: 75
reasoning: Both repos have error_reporting(E_ALL) and display_errors=on, which would leak sensitive information in production.
impact: Low-Medium - Information disclosure aiding further attacks.
verify_steps: 1. Check if any ozoon.com endpoints return PHP error messages 2. Trigger errors to see if stack traces are exposed
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-03 19:25:07 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-03 21:52:51 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-03 23:43:53 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 02:18:46 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 07:18:00 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 12:15:49 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 16:24:54 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
