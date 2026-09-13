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
## REPOSCAN 2026-09-04 19:07:15 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 21:32:03 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-04 23:15:44 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 01:00:05 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 05:29:11 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 09:18:55 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 12:48:30 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 15:44:58 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 17:50:58 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 19:46:15 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 21:51:20 UTC
[HYP] Hardcoded MySQL Credentials — hyper repo
class: SECRET
asset: oZoon/hyper/core/config.php:17
confidence: 85
reasoning: Hardcoded MySQL creds: user='hyper', password='12345', database='hyper'. Also mysql.txt:51-54 documents test user hyper01/123456789 with identical weak password. Config also has error_reporting(E_ALL) + display_errors=on (debug leak).
impact: Medium — If any Ozoon host deploys this PHP app, DB compromise via trivial creds. Weak passwords are trivially guessable.
verify_steps: 1. Check if any ozoon.com/ozoon.eu subdomain runs this PHP codebase (server header PWS/8.3.1.0.8 suggests shared hosting) 2. Test MySQL 3306 exposure on discovered hosts 3. Attempt login with creds if service is live
[HYP] Hardcoded MySQL Credentials — mas-film repo
class: SECRET
asset: oZoon/mas-film/core/config.php:13
confidence: 80
reasoning: Hardcoded MySQL creds: user='masha', password='12345', database='masha'. Same weak password pattern as hyper. debug mode also enabled.
impact: Medium — Database compromise if deployed.
verify_steps: 1. Check if any ozoon subdomain hosts this film database app 2. Scan for MySQL 3306 on discovered hosts
[HYP] Hardcoded MySQL Credentials — secure-query-string repo
class: SECRET
asset: oZoon/secure-query-string/settings.php:7
confidence: 75
reasoning: Hardcoded MySQL creds: user='aaa', password='aaa', database='aaa'. Identical user/pass/db pattern suggests test/dev default. Also has SQL injection (line 44, 50-51, 129) via direct string concatenation of user input.
impact: Medium — DB compromise if deployed; SQLi amplifies impact.
verify_steps: 1. Check if any ozoon subdomain runs this URL-shortener app 2. Scan for MySQL exposure
[HYP] Active Unsplash API Key + Secret — diploma-try repo
class: SECRET
asset: oZoon/diploma-try/src/js/lib/constants.js:13-14
confidence: 95
reasoning: Hardcoded Unsplash ACCESS_KEY='KVx67XvmzAv0NWFzGhl02RT3YJ0kXfNhhffCmc6V2Vk' and SECRET='NEbVoZN0xAL1MJkl9GCIfHmud75H71MjACB2fo0UdiU'. API key CONFIRMED ACTIVE via live Unsplash API call (returned valid photo data). Same keys also in diploma repo (oZoon/diploma/src/lib/constants.js:11-12).
impact: Low-Medium — Unsplash API abuse (rate limit exhaustion, unauthorized photo operations if key has write scope). If reused on any Ozoon production system, broader credential compromise.
verify_steps: 1. Test key scope: try write operations (like/unlike photo) 2. Check rate limit status 3. Verify keys aren't reused in any ozoon.eu/ozoon.com production systems
[HYP] Hardcoded Auth Credentials — php-learn-5 repo
class: SECRET
asset: oZoon/php-learn-5/include/helpers.php:46-52
confidence: 70
reasoning: Hardcoded auth array: 1@bk.ru/1, 2@bk.ru/2, 3@bk.ru/3, 4@bk.ru/4, 5@bk.ru/5. Passwords are single digits matching email numeric prefix. Learning project only.
impact: Low — Test/learning credentials; pattern indicates weak security practices.
verify_steps: 1. Check if this app is deployed anywhere 2. Test if any ozoon subdomain uses similar auth patterns
[HYP] SQL Injection — hyper repo functions.php
class: OTHER
asset: oZoon/hyper/core/functions.php:142,165,180,373,402,434,484
confidence: 85
reasoning: All SQL queries use string concatenation with user input (e.g. line 142: 'SELECT userId FROM tokens WHERE token = \'' . $state['qs']['token'] . '\''). No parameterized queries. The allowSymbols check provides minimal input validation but doesn't prevent injection in all paths.
impact: Medium — SQLi could lead to data exfiltration, auth bypass, or RCE if deployed with MySQL FILE privilege.
verify_steps: 1. Identify if any ozoon endpoints run this code 2. Test SQL injection on any discovered PHP endpoints
[HYP] SQL Injection — mas-film repo functions.php
class: OTHER
asset: oZoon/mas-film/core/functions.php:92,96,100,104,120,127,144-145,165-166,188,213,233-234,254-256,278,298-299,319-321,346,367-368,389-390
confidence: 85
reasoning: Pervasive SQL injection via string concatenation across all CRUD operations. checkStr() provides minimal alphanumeric filtering but doesn't prevent injection in filter/sort parameters.
impact: Medium — Full DB compromise via SQLi if deployed.
verify_steps: 1. Check if any ozoon subdomain runs this app 2. Test injection on filter/sort parameters
[HYP] SQL Injection — secure-query-string repo settings.php
class: OTHER
asset: oZoon/secure-query-string/settings.php:44,50-51,129
confidence: 80
reasoning: SQL queries built via string concatenation. Line 44: 'SELECT encode FROM aSecure WHERE decode = \''.$decode.'\'' — direct user input in WHERE clause. Line 129: similar pattern for encode lookup.
impact: Medium — SQLi on a URL shortener could leak all shortened URLs and their destinations.
verify_steps: 1. Check if deployed 2. Test injection on decode parameter
[HYP] Debug Mode Enabled — hyper, mas-film repos
class: MISCONFIG
asset: oZoon/hyper/core/config.php:4-5, oZoon/mas-film/core/config.php:4-5
confidence: 75
reasoning: Both repos have error_reporting(E_ALL) + display_errors=on. Leaks PHP error messages, stack traces, and potentially sensitive variable values in production.
impact: Low-Medium — Information disclosure aiding further attacks (file paths, DB structure, PHP version).
verify_steps: 1. Trigger errors on any ozoon PHP endpoints 2. Check for PHP error messages in HTTP responses
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-05 23:28:46 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 01:18:33 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 06:03:02 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 11:01:33 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 14:14:52 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 17:06:31 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 19:14:18 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 21:23:53 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-06 23:06:30 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 01:04:12 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 06:09:31 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 12:39:40 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 17:55:25 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 20:57:38 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-07 23:17:29 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 01:14:49 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 06:00:41 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 10:57:31 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 14:52:57 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 18:14:09 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 21:12:35 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-08 23:25:51 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 01:29:31 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 06:39:01 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 11:49:59 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 15:42:30 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 18:59:43 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 21:33:34 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-09 23:31:42 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 01:25:26 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 06:39:04 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 11:49:43 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 15:44:40 UTC
[HYP] Active Unsplash API Key + Secret — diploma-try repo
class: SECRET
asset: oZoon/diploma-try/src/js/lib/constants.js:13-14
confidence: 95
reasoning: Hardcoded Unsplash ACCESS_KEY='KVx67XvmzAv0NWFzGhl02RT3YJ0kXfNhhffCmc6V2Vk' and SECRET='NEbVoZN0xAL1MJkl9GCIfHmud75H71MjACB2fo0UdiU'. API key CONFIRMED ACTIVE via live Unsplash API call (returned valid photo data). Same keys also present in oZoon/diploma/src/lib/constants.js:11-12. Exposed in a public GitHub repo.
impact: Medium — Active API key abuse (rate-limit exhaustion, unauthorized read/write operations if scope permits). If reused in any Ozoon production system, broader credential compromise. Abusable by any anonymous visitor to the public repo.
verify_steps: 1. Test key scope: attempt write operations (like/unlike a photo) to determine if key has write permissions 2. Check Unsplash rate-limit headers to see if key has elevated limits 3. Cross-reference whether these keys appear in any ozoon.eu/ozoon.com production bundles or environment configs 4. Revoke key via Unsplash developer dashboard if scope allows
[HYP] Hardcoded MySQL Credentials — hyper repo
class: SECRET
asset: oZoon/hyper/core/config.php:17
confidence: 85
reasoning: Hardcoded MySQL creds: user='hyper', password='12345', database='hyper'. Also mysql.txt:51-54 documents test user hyper01/123456789 with identical weak password. Config also has error_reporting(E_ALL) + display_errors=on (debug leak). Weak passwords trivially guessable. All committed to a public GitHub repo.
impact: Medium — If any Ozoon host deploys this PHP app, database compromise via trivial creds. The password '12345' is the 3rd most common password globally.
verify_steps: 1. Check if any ozoon.com/ozoon.eu subdomain runs this PHP codebase (server header PWS/8.3.1.0.8 suggests shared hosting) 2. Test MySQL 3306 exposure on discovered hosts 3. Attempt login with creds if service is live
[HYP] Hardcoded MySQL Credentials — mas-film repo
class: SECRET
asset: oZoon/mas-film/core/config.php:13
confidence: 80
reasoning: Hardcoded MySQL creds: user='masha', password='12345', database='masha'. Same weak password pattern as hyper. Debug mode also enabled (error_reporting(E_ALL) + display_errors=on).
impact: Medium — Database compromise if deployed.
verify_steps: 1. Check if any ozoon subdomain hosts this film database app 2. Scan for MySQL 3306 on discovered hosts
[HYP] Hardcoded MySQL Credentials — secure-query-string repo
class: SECRET
asset: oZoon/secure-query-string/settings.php:7
confidence: 75
reasoning: Hardcoded MySQL creds: user='aaa', password='aaa', database='aaa'. Identical user/pass/db pattern suggests test/dev default but committed to public GitHub.
impact: Medium — DB compromise if deployed; SQLi amplifies impact.
verify_steps: 1. Check if any ozoon subdomain runs this URL-shortener app 2. Scan for MySQL exposure
[HYP] SQL Injection — hyper repo functions.php
class: OTHER
asset: oZoon/hyper/core/functions.php:142,165,180,373,402,434,484
confidence: 85
reasoning: All SQL queries use string concatenation with user input (e.g. line 142: 'SELECT userId FROM tokens WHERE token = \'' . $state['qs']['token'] . '\''). No parameterized queries. The allowSymbols check provides minimal input validation but doesn't prevent injection in all paths.
impact: Medium — SQLi could lead to data exfiltration, auth bypass, or RCE if deployed with MySQL FILE privilege.
verify_steps: 1. Identify if any ozoon endpoints run this code 2. Test SQL injection on any discovered PHP endpoints
[HYP] SQL Injection — mas-film repo functions.php
class: OTHER
asset: oZoon/mas-film/core/functions.php:92,96,100,104,120,127,144-145,165-166,188,213,233-234,254-256,278,298-299,319-321,346,367-368,389-390
confidence: 85
reasoning: Pervasive SQL injection via string concatenation across all CRUD operations. checkStr() provides minimal alphanumeric filtering but doesn't prevent injection in filter/sort parameters.
impact: Medium — Full DB compromise via SQLi if deployed.
verify_steps: 1. Check if any ozoon subdomain runs this app 2. Test injection on filter/sort parameters
[HYP] SQL Injection — secure-query-string repo settings.php
class: OTHER
asset: oZoon/secure-query-string/settings.php:44,50-51,129
confidence: 80
reasoning: SQL queries built via string concatenation. Line 44: 'SELECT encode FROM aSecure WHERE decode = \''.$decode.'\'' — direct user input in WHERE clause. Line 129: similar pattern for encode lookup.
impact: Medium — SQLi on a URL shortener could leak all shortened URLs and their destinations.
verify_steps: 1. Check if deployed 2. Test injection on decode parameter
[HYP] Hardcoded Auth Credentials — php-learn-5 repo
class: SECRET
asset: oZoon/php-learn-5/include/helpers.php:46-52
confidence: 70
reasoning: Hardcoded auth array: 1@bk.ru/1, 2@bk.ru/2, 3@bk.ru/3, 4@bk.ru/4, 5@bk.ru/5. Passwords are single digits matching email numeric prefix. Learning project only.
impact: Low — Test/learning credentials; pattern indicates weak security practices.
verify_steps: 1. Check if this app is deployed anywhere 2. Test if any ozoon subdomain uses similar auth patterns
[HYP] Debug Mode Enabled in Production Config — hyper, mas-film repos
class: MISCONFIG
asset: oZoon/hyper/core/config.php:4-5, oZoon/mas-film/core/config.php:4-5
confidence: 75
reasoning: Both repos have error_reporting(E_ALL) + display_errors=on. Leaks PHP error messages, stack traces, and potentially sensitive variable values in production.
impact: Low-Medium — Information disclosure aiding further attacks (file paths, DB structure, PHP version).
verify_steps: 1. Trigger errors on any ozoon PHP endpoints 2. Check for PHP error messages in HTTP responses
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 18:41:05 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 21:13:19 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-10 23:13:08 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 01:08:23 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 06:03:08 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 11:29:25 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 15:10:55 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 18:36:16 UTC
[HYP] <title>
class: SECRET|MISCONFIG|IDOR|SSRF|OTHER
asset: <repo/path>
confidence: <0-100>
reasoning: <facts>
impact: <severity>
verify_steps: <how to confirm it is live/in-scope, passive>
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 21:15:48 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-11 23:18:12 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 01:15:52 UTC
[HYP] Hardcoded MySQL Credentials in hyper PHP Image Service
class: SECRET
asset: oZoon/hyper/core/config.php:17
confidence: 15
reasoning: Hardcoded DB credentials in source: mysqli('localhost', 'hyper', '12345', 'hyper'). Password '12345' is trivially guessable. mysql.txt also documents the full setup including INSERT of user 'hyper01' with password '123456789'. These are localhost dev credentials for a small PHP image hosting demo.
impact: low
verify_steps: |
[HYP] SQL Injection via String Concatenation in hyper PHP App
class: OTHER
asset: oZoon/hyper/core/functions.php:142,402,406,434,484
confidence: 15
reasoning: All SQL queries are built via direct string concatenation of user-controlled input ($state['qs']['*']) into raw SQL. The 'checkSymbols()' whitelist provides some defense (alphanumeric only), but the pattern is fundamentally unsafe — no parameterized queries or prepared statements. If the whitelist is ever bypassed or expanded, SQL injection is immediate.
impact: low
verify_steps: |
[HYP] Hardcoded Unsplash API Keys (ACCESS_KEY + SECRET)
class: SECRET
asset: oZoon/diploma/src/lib/constants.js:11-12
confidence: 10
reasoning: Hardcoded Unsplash API credentials: ACCESS_KEY='KVx67XvmzAv0NWFzGhl02RT3YJ0kXfNhhffCmc6V2Vk' and SECRET='NEbVoZN0xAL1MJkl9GCIfHmud75H71MjACB2fo0UdiU'. These are used in a diploma project (Unsplash photo browser). Unsplash demo keys have low value but could be abused for rate-limit evasion or logged in the Unsplash developer dashboard.
impact: low
verify_steps: |
[HYP] Firebase Config Exposed via .env.example Pattern (restaurant app)
class: MISCONFIG
asset: oZoon/restaurant/.env.example + src/constants/env.ts + src/dal/firebase/index.ts
confidence: 10
reasoning: The restaurant app uses Firebase (initializeApp with apiKey, authDomain, projectId, storageBucket, etc.) loaded from environment variables. The .env.example shows the structure but uses 'your_data' placeholders. The Firebase config pattern is visible in source; if the actual .env were ever committed, full Firebase credentials would be exposed. No actual secrets are in the repo.
impact: informational
verify_steps: |
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 05:50:36 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 09:53:54 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 13:20:24 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 16:21:56 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 18:43:46 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 21:09:06 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-12 23:19:05 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-13 01:03:38 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
## REPOSCAN 2026-09-13 06:14:17 UTC
TARGET_ORG not configured for ozoon-sportsbook-casino; skipping public-org deep scan.
