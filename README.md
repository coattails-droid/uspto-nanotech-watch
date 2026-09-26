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

**Nanotech class alerts** runs every day at 14:15 UTC (10:15am US Eastern). It needs no AI. It files one issue for newly published applications and new grants in CPC `B82` or USPC class `977`, and assigns you.

**Nanotech patent watch** is the Copilot review of borderline title matches. It is manual only. This account's Copilot plan rejects every model the agent can request, so a daily schedule was only producing failed runs. After Copilot includes `gpt-5-mini`, start it from the Actions tab.

The USPTO key is stored as the Actions secret `USPTO_API_KEY`. Class alerts read it. The agent does not.

The agent still cannot start: Copilot returned "model not supported" for `claude-sonnet-5`, `gpt-5.4-mini`, and `claude-haiku-4.5`. That is the plan on this GitHub account, not a bad patent query. Enable a Copilot plan that includes `gpt-5-mini`, then run **Nanotech patent watch** from Actions.

## Files

- [`.github/workflows/nanotech-class-alerts.yml`](.github/workflows/nanotech-class-alerts.yml) is the no-AI alert for official nanotech classes.
- [`.github/workflows/nanotech-patent-watch.md`](.github/workflows/nanotech-patent-watch.md) is the agent's instructions. Edit this in plain language if you want a tighter or looser definition of nanotechnology.
- [`.github/workflows/nanotech-patent-watch.lock.yml`](.github/workflows/nanotech-patent-watch.lock.yml) is the compiled Actions workflow. Regenerate it with `gh aw compile` after frontmatter changes. Instruction-only edits do not need a recompile.
- [`scripts/uspto_recent.py`](scripts/uspto_recent.py) fetches the candidate list. The agent does not get to invent patents that are not in that list.
