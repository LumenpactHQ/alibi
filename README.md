# Alibi

**The fraud analyst that argues against itself.**

Team **NovaStack** · Open Agent Hackathon 2026 · **Track 2: The Agent That Can Explain Why**

A fraud analyst gives Alibi a flagged transaction. Alibi pulls evidence from several data sources through Zetaris, builds a case, and then a Devil's Advocate agent tries to find an *alibi*, proof that the transaction is legitimate. If no alibi survives, the case stands. The output is a case file: verdict, timeline, and evidence behind every claim.

> All data is synthetic. Verdicts are recommendations for human review, not final decisions.

## Team

| Person | Owns |
|---|---|
| Joel (leader) | Zetaris, synthetic data, repo, submission |
| Keshinro | Agent loop, prompts, model connection |
| Excellence | Interface (alert queue and case file view) |

## The problem in five lines

1. **User:** a fraud analyst at a Nigerian payments company
2. **Problem:** investigating one flagged transaction takes hours across separate systems, and most flags are false alarms
3. **Goes in:** an alert ID
4. **Comes out:** a case file with verdict (`likely_fraud`, `likely_legitimate`, `needs_human`), timeline, evidence, and a recommended action
5. **Why an agent:** it must form hypotheses, query for each, and survive a challenge before it is trusted

## Agents

| Agent | Job |
|---|---|
| Planner | Reads the alert, lists hypotheses (account takeover, mule account, card testing...), assigns queries |
| Evidence Retriever | Writes and runs Zetaris SQL, returns rows with the query that produced them |
| Pattern Analyst | Connects the evidence into a case with a confidence score |
| Devil's Advocate | Argues the transaction is legitimate and tries to find an alibi. Rejects claims with no evidence |
| Reporter | Produces the final case file |

The loop that judges should see: Analyst builds a case, Devil's Advocate rejects a weak claim, Retriever fetches more evidence, Analyst revises, Devil's Advocate accepts or escalates to `needs_human`. Every step goes in the trace.

## Sponsor technology

| Sponsor | Role in Alibi |
|---|---|
| Zetaris | Single access point over the transactions DB, device logs, customer CSV and watchlist. Agents query it over REST |
| Meterless | TODO: fill in after reading the Meterless folder in HackOS Resources |
| Cursor | Development environment for the whole team |
| NVIDIA | TODO: only if it genuinely contributes (for example the model route) |

## Repo layout (target)

```
alibi/
  README.md
  .env.example
  docs/
    CASE_FILE_CONTRACT.md   # the JSON shape everyone builds against
    prompts/                # one Cursor prompt per person
  agents/                   # Keshinro
  data/                     # synthetic data and generator (Joel)
  web/                      # interface (Excellence)
  traces/                   # saved agent traces for the demo
```

## Setup

1. Copy `.env.example` to `.env` and fill in your own values. **Never commit `.env`.**
2. Start Zetaris locally (see the Participant Setup Guide on HackOS). Zetaris keeps its own `.env` inside its install folder. Never copy that into this repo.
3. Dependencies and run commands: TODO, filled in as the code lands.

Run with `SAMPLE_MODE=true` to use cached sample results if Zetaris or the model service is unavailable.

## Rules we follow (from the judging guide)

- No API keys anywhere in the repo, README, screenshots or logs
- Every claim in a case file links to the query and rows behind it
- No hardcoded demo path: the agent must work on all scenarios in `data/`
- Logs and traces saved for every run
- Declare any pre-existing code in the HackOS submission form

## Submission checklist (deadline Tue 27 Oct, 23:45 UTC = Wed 28 Oct, 00:45 Lagos)

- [ ] Demo video, 1 to 4 minutes, shows Zetaris and Meterless integration
- [ ] Slide deck
- [ ] Repo with README covering setup, usage, dependencies
- [ ] Sponsor technology explanation (Zetaris, Meterless, Cursor, NVIDIA)
- [ ] Pre-existing components declared
- [ ] No committed secrets
- [ ] Clean-setup test passed
- [ ] Submitted on HackOS (aim for the 27th, daytime)
