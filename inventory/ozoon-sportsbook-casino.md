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
