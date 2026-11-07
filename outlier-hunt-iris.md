# Mini outlier hunt: Iris dataset, sepal_width z-scores (fetched live 2026-11-07)

Dataset URL (verified live, HTTP 200 at fetch): https://raw.githubusercontent.com/uiuc-cse/data-fa14/gh-pages/data/iris.csv (150 rows)

Column choice: **sepal_width** — it is the most approximately normal of the four numeric columns (petal_length is bimodal by species, so a global z-score there would just re-detect species clustering, not outliers).

Mean = 3.054 cm, SD = 0.434 cm (n=150, population of the four numeric columns; sample SD).

Top 5 outliers by |z-score|:

| Rank | z | sepal_width (cm) | Species | Judgment |
|---|---|---|---|---|
| 1 | +3.104 | 4.4 | setosa | Genuine extreme case — real measured flower, plausible value within botanical range |
| 2 | +2.643 | 4.2 | setosa | Genuine extreme case — same wild-setosa wide-sepal tendency |
| 3 | -2.431 | 2.0 | versicolor | Genuine extreme case — narrow low end; not a placeholder (2.0 is physically possible) |
| 4 | +2.412 | 4.1 | setosa | Genuine extreme case |
| 5 | +2.182 | 4.0 | setosa | Genuine extreme case |

Judgment summary: all five are genuine extreme measurements, not data-quality artifacts — there are no sentinel values (no 9999/0/-1), no unit mismatches (all centimeters), and every value stays inside the plausible botanical range for Iris. The setosa-heavy top of the list is a real biological property (setosa has wider sepals relative to the global mean), not noise.
