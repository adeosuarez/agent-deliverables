# Mini API contract audit: 4 documented claims vs live responses (fetched live 2026-11-07)

| # | Claim (documented) | Doc URL | Live test (today) | Actual result | Verdict |
|---|---|---|---|---|---|
| 1 | GitHub REST API: unauthenticated requests are limited to 60 per hour per IP | https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api | GET https://api.github.com/zen with no token; read response headers | Header `x-ratelimit-limit: 60` present on the response | CONFIRMED |
| 2 | JSONPlaceholder: `/posts` resource contains 100 posts (6 resources x fixed counts) | https://jsonplaceholder.typicode.com/guide/ | GET https://jsonplaceholder.typicode.com/posts; count array items | Returned exactly 100 items | CONFIRMED |
| 3 | Open-Meteo: `current` weather uses a 15-minute update interval (`interval=900` seconds) | https://open-meteo.com/en/docs | GET https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m | `current.interval: 900` (seconds), `current_units.interval: seconds` | CONFIRMED |
| 4 | Frankfurter: any supported currency can be set as `base`, e.g. `base=USD` | https://frankfurter.dev/ | GET https://api.frankfurter.dev/v1/latest?base=USD | `base: "USD"` echoed with full rate table (e.g. IDR 18100) | CONFIRMED |

All four documented claims held against live responses fetched today. No discrepancies found; no claim required hedging.
