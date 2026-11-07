# Mini digest freshness audit: 5 AI-agent release pages (fetched live 2026-11-07T07:3xZ UTC)

All data fetched live from official GitHub Releases pages today. Age = today (2026-11-07) minus newest release date.

| Tool (maintainer) | Newest release | Date | Age | Gap to 2nd newest | Verdict |
|---|---|---|---|---|---|
| openai-python (OpenAI) | v3.47.0 | 2026-11-05 | 2 days | same day (v3.46.0 also 2026-11-05) | active |
| anthropic-sdk-python (Anthropic) | v2.6.0 | 2026-11-05 | 2 days | same day | active |
| langchain (LangChain) | langchain-typesafe 0.0.2 | 2026-11-05 | 2 days | same day | active |
| browser-use (Browser Use) | 0.13.14 | 2026-10-26 | 12 days | 2 days before (0.13.13) | active |
| crewAI (CrewAI Inc.) | 1.15.37 | 2026-11-07 | 0 days | 1 day (1.15.36 on 2026-11-06) | active |

What changed (newest release, from the official page):
- openai-python v3.47.0: adds a `model` option for the Responses API.
- anthropic-sdk-python v2.6.0: new error codes for ramp-limit and External Key validation failures; organization member sign-out support.
- langchain typesafe 0.0.2: experimental middleware accepts a `classifier`.
- browser-use 0.13.14: adds OpenAI browser tools and a shared Bash helper; reliability improvements.
- crewAI 1.15.37: routine patch release published today (patch-level fixes per the release page).

Note: 5 tools requested, 3+ maintainers — OpenAI, Anthropic, LangChain, Browser Use, CrewAI Inc. are 5 distinct maintainers. A sixth candidate checked (microsoft/autogen) was excluded: its GitHub Releases page shows a newest release over 400 days old (stale) even though the repo is actively pushed — an example of release-page vs repo-activity divergence.
