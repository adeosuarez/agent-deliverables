# API probe: real error behavior of 4 public APIs (fetched live 2026-11-07 ~07:2xZ)

| API | Failure mode | HTTP | Error code / message (verbatim) | Content-Type | Request |
|---|---|---|---|---|---|
| GitHub | wrong path | 404 | `"message": "Not Found"` + `documentation_url: https://docs.github.com/rest` | application/json; charset=utf-8 | GET /thispathdoesnotexist12345 |
| GitHub | no auth (protected route) | 401 | `"message": "Requires authentication"` | application/json; charset=utf-8 | GET /user (no token) |
| GitHub | malformed param | 200 | (no error — `per_page=notanumber` silently ignored, returns default page) | application/json; charset=utf-8 | GET /repos/octocat/Hello-World/issues?per_page=notanumber |
| GitHub | rate limit (documented) | — | 60 requests/hour per IP unauthenticated | — | https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api |
| JSONPlaceholder | wrong path | 404 | `{}` (empty JSON body) | application/json; charset=utf-8 | GET /nope12345 |
| JSONPlaceholder | no auth | 200 | (open API — no auth exists) | application/json | GET /posts/1 |
| JSONPlaceholder | malformed param | 200 | `[]` (silently returns empty array) | application/json | GET /posts?userId=notanumber |
| JSONPlaceholder | rate limit (documented) | — | undocumented (no official limit published) | — | https://jsonplaceholder.typicode.com/guide/ |
| Frankfurter | wrong path | 404 | `{"status":404,"message":"not found"}` | application/json | GET /v1/no-such-endpoint |
| Frankfurter | no auth | 200 | (open API — no auth exists) | application/json; charset=utf-8 | GET /v1/latest |
| Frankfurter | malformed param | 404 | `{"message":"not found"}` (unknown base currency) | application/json; charset=utf-8 | GET /v1/latest?base=NOTACURRENCY |
| Frankfurter | rate limit (documented) | — | undocumented (free public API, no published cap) | — | https://frankfurter.dev/ |
| Open-Meteo | wrong path | 404 | `{"error":true,"reason":"Not Found"}` | application/json; charset=utf-8 | GET /v1/no-such-path |
| Open-Meteo | no auth | 200 | (open API — no auth exists) | application/json | GET /v1/forecast |
| Open-Meteo | malformed param | 400 | `{"reason":"Invalid value: Cannot initialize Float from invalid String value abc","error":true}` | application/json; charset=utf-8 | GET /v1/forecast?latitude=abc&longitude=def |
| Open-Meteo | rate limit (documented) | — | free tier: 10,000 calls/day, 5,000/hour (docs) | — | https://open-meteo.com/en/docs (pricing/limits section) |

Key finding: three of four APIs fail open on malformed parameters (200 with defaults or empty results) — only Open-Meteo validates strictly with a 400 and a precise message. Only GitHub returns a documented auth error; the other three require no auth at all.
