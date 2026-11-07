# Latency benchmark: 5 public no-key APIs (3 timed runs each, median)

Fetched live 2026-11-07 ~07:20Z from a Linux server (curl, HTTP/2, no proxy). Timings in milliseconds.

| API | Status (3 runs) | Timings (ms) | Median (ms) |
|---|---|---|---|
| api.github.com/zen | 200 / 200 / 200 | 36, 32, 31 | 32 |
| api.frankfurter.dev/v1/latest | 200 / 200 / 200 | 34, 34, 35 | 34 |
| earthquake.usgs.gov (all_hour.geojson) | 200 / 200 / 200 | 351, 349, 338 | 349 |
| api.open-meteo.com (forecast, 1 param pair) | 200 / 200 / 200 | 1071, 706, 1106 | 1071 |
| api.blockchair.com/bitcoin/stats | 200 / 200 / 200 | 1171, 1209, 1236 | 1209 |

Methodology: 3 identical sequential GETs per API, 1s apart, response wall-clock time measured by curl's time_total; median = middle of the three values. No auth headers, no concurrency, single region.

Notable: GitHub and Frankfurter respond in ~35 ms (CDN-cached lightweight endpoints). USGS's geojson summary (~350 ms) serves a much larger payload. Open-Meteo and Blockchair both take >1 s — Open-Meteo's variance (706-1106 ms) suggests model-compute on request; Blockchair is consistently the slowest of the five. No rate-limit headers were returned on these paths.
