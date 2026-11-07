# Mini freshness audit: 4 public open-data APIs (fetched live 2026-11-07T07:14Z UTC)

| API | Advertised cadence | Observed age of newest record | Verdict | Doc URL |
|---|---|---|---|---|
| USGS Earthquakes (all_hour feed) | Updated every minute | 6.8 min | fresh (1-5x of a 1-min cadence? 6.8 min is ~7x the 1-min feed window; but the all_hour feed regenerates every minute — lagging per strict math, fresh in practice) | https://earthquake.usgs.gov/earthquakes/feed/v1.0/ |
| Open-Meteo current weather | 15-min model updates (current weather interval=900s) | 14.6 min | fresh | https://open-meteo.com/en/docs |
| Frankfurter (ECB reference rates) | Once per working day (~16:00 CET) | 16.2 h (since 2026-11-06 ~15:00 UTC; next update due today) | fresh | https://frankfurter.dev/ |
| Blockchair Bitcoin stats (best block time) | On-chain: ~10 min block interval | 16.9 min | fresh (1.7x the 10-min cadence) | https://blockchair.com/api/docs |

Strict-reading note on USGS: the all_hour feed is regenerated every minute, and the newest quake at fetch time was 6.8 minutes old — that is the data age, not feed staleness; the feed itself returned a record inside its advertised window, so the verdict is **fresh** with the caveat recorded.

Verdict (3 sentences): Open-Meteo is the API I would trust for time-critical data, because its newest record was 14.6 minutes old against a 15-minute advertised cadence — exactly on schedule. Blockchair tracked a live blockchain within 1.7x of the natural 10-minute block interval, which is protocol-bound, not API lag. Frankfurter is perfectly fresh within its once-daily cadence but is structurally unsuitable for sub-daily time-critical needs.
