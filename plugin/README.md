# Research Toolkit

A research plugin for Claude Code built on Exa. You ask in plain language ("research X", "what are my options for Y", "monitor Z") or type `/research`, and one skill picks the right mode, runs it, and writes a dated Markdown note with sources.

The point is discipline, not more searching. The toolkit stops and hands control back to you at the two moments where research usually goes wrong: when the criteria are set, and when the shortlist is cut. Finalists are scored against your weights, never picked by eye.

## Six modes

| You want | Mode | Stops for you? |
| --- | --- | --- |
| A fact, one entity, one page read | Lookup | No |
| The landscape: who and what is out there | Sweep | Yes: you set the criteria and weights |
| Every candidate scored against those criteria | Cut | Yes: you approve the cut line |
| The truth about the survivors | Verify | No |
| A structured list of a whole universe (companies, people, filings) | Enumerate | No |
| A researched answer in one call, with per-field confidence | Deep | No |
| What is new on a topic, on a schedule | Watch | No |

Sweep, Cut and Verify are one pipeline: wide before deep, with your judgement at the joints.

## What is in the box

- `skills/research/SKILL.md`: the router and the method.
- `agents/searcher.md`: a small subagent that runs one search territory and returns deduplicated rows, used when the sweep script can't reach the Exa API.
- `scripts/exa_extras.py`: a standard-library-only CLI for `sweep`, `deep`, and Exa monitors.
- `references/`: the Exa feature map, what Exa indexes beyond web pages, a source map for non-Exa tools (PubMed, Wolfram, the browser and so on), query patterns, mode procedures, and the note template.

The Cut step uses the decision matrix from the companion [reasoning-toolkit](https://github.com/dougkeiller/reasoning-toolkit). Install both for the full pipeline.

## Install

```bash
claude plugin marketplace add dougkeiller/research-toolkit
claude plugin install research-toolkit@research-toolkit
```

## Setup

You need your own Exa account.

1. Install Exa's MCP connector (for example Exa's own Claude Code plugin). This toolkit's `.mcp.json` declares Exa as a dependency but ships with no URL or key.
2. The installed plugin needs no API key: it uses the Exa connector. (Only the optional script in a clone of the GitHub repo needs a key; see that repo's root README.)
3. Monitors (Watch mode) also need a webhook URL in `EXA_MONITOR_WEBHOOK`.

Scheduled monitors have a recurring Exa cost; the skill states it before creating one. In cloud sessions the scripts need network egress to `api.exa.ai`.

## Version

1.4.0 (2026-11-10). Highlights: a three-line frame (question, what counts as a match, done when) before any search; a stop budget per mode; a source-mix check that adds a pass when the evidence is mostly vendor-written; every claim written from a single source's own words.

## Credit

`references/query-patterns.md` adapts material from Exa's MIT-licensed search skill.

## Running without the script

Anthropic's plugin directory accepts script files only from verified publishers, so the plugin carries no scripts. Sweeps and Deep Search run through the Exa connector's tools instead. Exa monitors (Watch mode) need `scripts/exa_extras.py`, which is in this repository but not in the installed plugin: to use monitors, clone the repo and run the script directly with your own Exa API key in `EXA_API_KEY` (`python3 scripts/exa_extras.py --help`).
