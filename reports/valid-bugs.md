# Validated findings (running count 0)

- 5 lead(s) marked VALID at 2026-09-06 22:19:41 UTC
  - **Verdict: HOLD** — Valid high-impact finding but requires program authorization for account creation to prove. Cannot submit without differential evidence (own 200 vs foreign 401/403 or 200+data).
  - **Verdict: HOLD** — Valid critical-severity finding but `isMockProviderEnabled:false` in SSR provides mild contrary evidence. Needs authenticated differential test.
  - **Verdict: HOLD** — Valid medium-high severity finding; also the gateway probe enabling Leads 1 and 2. Needs program authorization for account creation.
  - | Q6 Not always-rejected | **YES** — SSRF is valid class |
  - **Bottom line:** Leads 1-3 are the strongest findings on a real-money platform. All three are gated behind `account_creation:restricted` — the hunt bot cannot produce VALID verdicts without program au

- 1 lead(s) marked VALID at 2026-09-11 06:38:29 UTC
  - | Q7 Reasonable triager? | **No** — triager would classify as informational/informative at best, not a valid bug |

- 4 lead(s) marked VALID at 2026-09-17 05:01:21 UTC
  - | Q6 Not always-rejected? | **Yes** — hardcoded creds is a valid class |
  - | Q4 Provable? | **Yes** — already proven active via live API call (non-invasive GET returning valid photo data) |
  - | Q6 Not always-rejected? | **Borderline** — credential in public repo is valid, but Unsplash is a third-party service, not Ozoon infra. Program scope says "all infrastructure...operated by Ozoon." Th
  - | Q4 Provable? | **No** — all probes return 401 unauthenticated; cannot confirm BOLA without valid session |
