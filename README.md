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

## What runs

Two workflows run every day at 14:15 UTC (10:15am US Eastern). You can also start either one from the Actions tab.

| Workflow | What it does |
| --- | --- |
| Nanotech class alerts | No AI. Files an issue for newly published applications and new grants in CPC `B82` or USPC class `977`, and assigns you. |
| Nanotech patent watch | Copilot reads the remaining title matches (nanoparticles, graphene, quantum dots, and similar) and drops brand-name and measurement false alarms. |

## One thing you still have to add

USPTO requires a free API key. This repository does not contain one.

1. Sign in and create a key: [Open Data Portal getting started](https://data.uspto.gov/apis/getting-started).
2. In this repository, open **Settings → Secrets and variables → Actions → New repository secret**.
3. Name it exactly `USPTO_API_KEY` and paste the key.
4. Open **Actions → Nanotech class alerts → Run workflow**.

Until that secret exists, the class-alert workflow files a single setup issue instead of guessing.

The agentic review also needs GitHub Copilot on this account. The first runs called Copilot and it rejected every model (`claude-sonnet-5`, `gpt-5.4-mini`, `claude-haiku-4.5`) as unavailable for the current plan. Enable Copilot, or a plan that includes `gpt-5-mini`, under **GitHub Settings → Copilot**, then run **Nanotech patent watch** again. Official `B82` / `977` alerts do not wait on that.

## Files

- [`.github/workflows/nanotech-class-alerts.yml`](.github/workflows/nanotech-class-alerts.yml) is the no-AI alert for official nanotech classes.
- [`.github/workflows/nanotech-patent-watch.md`](.github/workflows/nanotech-patent-watch.md) is the agent's instructions. Edit this in plain language if you want a tighter or looser definition of nanotechnology.
- [`.github/workflows/nanotech-patent-watch.lock.yml`](.github/workflows/nanotech-patent-watch.lock.yml) is the compiled Actions workflow. Regenerate it with `gh aw compile` after frontmatter changes. Instruction-only edits do not need a recompile.
- [`scripts/uspto_recent.py`](scripts/uspto_recent.py) fetches the candidate list. The agent does not get to invent patents that are not in that list.
