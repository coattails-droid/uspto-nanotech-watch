# USPTO nanotech watch

A GitHub agentic workflow that reviews newly **published** US patent applications and newly **granted** patents, then opens an issue here when one is actually about nanotechnology.

You are assigned on that issue, so GitHub notifies you.

## What it can and cannot see

The Patent Office does not make most new filings public on the day they are filed.

- Utility applications are usually published about 18 months after the filing date.
- An applicant can request earlier publication, or request that the application not be published at all.
- Provisional applications are never published on their own.
- Grants show up when the patent issues. Pre-grant publications show up on their publication date.

The watcher uses the official [USPTO Open Data Portal](https://data.uspto.gov/), which refreshes daily. Each run looks at grants and publications from the last 4 days, so a missed day is picked up on the next run.

Official nanotechnology classes are included automatically: CPC `B82B` / `B82Y`, and USPC class `977`. Other candidates (nanoparticles, nanotubes, graphene, quantum dots, and similar) are read by the agent, which drops brand-name and measurement false alarms.

## One thing you still have to add

USPTO requires a free API key. This repository does not contain one.

1. Sign in and create a key: [Open Data Portal getting started](https://data.uspto.gov/apis/getting-started).
2. In this repository, open **Settings → Secrets and variables → Actions → New repository secret**.
3. Name it exactly `USPTO_API_KEY` and paste the key.
4. Open **Actions → Nanotech patent watch → Run workflow**.

Until that secret exists, the workflow files a single setup issue instead of guessing.

The review itself runs on GitHub Copilot inside Actions (`engine: copilot`). Your GitHub account needs Copilot available to this repository, and Actions has to be allowed to run. The run is weekday-capable every day at 14:15 UTC (10:15am US Eastern).

## Files

- [`.github/workflows/nanotech-patent-watch.md`](.github/workflows/nanotech-patent-watch.md) is the agent's instructions. Edit this in plain language if you want a tighter or looser definition of nanotechnology.
- [`.github/workflows/nanotech-patent-watch.lock.yml`](.github/workflows/nanotech-patent-watch.lock.yml) is the compiled Actions workflow. Regenerate it with `gh aw compile` after frontmatter changes. Instruction-only edits do not need a recompile.
- [`scripts/uspto_recent.py`](scripts/uspto_recent.py) fetches the candidate list. The agent does not get to invent patents that are not in that list.
