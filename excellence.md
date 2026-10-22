# Excellence: interface

**Your job:** build the screens the judges will see in the demo: an alert queue and a case file view that makes the reasoning easy to follow.

**Before you start:** read `README.md` and `docs/CASE_FILE_CONTRACT.md`. Build against the example JSON in the contract as fake data. You do not need the agents or Zetaris to be ready.

## Paste this into Cursor (Agent mode)

```
You are helping me build the interface of "Alibi", a fraud-investigation agent for the Open Agent Hackathon 2026. Judges score Product and UX, and Demo, so clarity matters most.

Read README.md and docs/CASE_FILE_CONTRACT.md first. The case file JSON is the data shape. Build in /web.

Use React with Vite and plain CSS (or Tailwind if you prefer). Load data from local JSON files in /web/public/sample (copy the example from the contract and create 3 more samples: one likely_legitimate, one needs_human, one likely_fraud with a rejected alibi). Later we will swap this for a real API call, so put all data loading in one file (api.js) with a function getAlerts() and getCase(alertId).

Build two screens:

1. Alert queue: a list of alerts (alert_id, amount with currency, flagged_at, reason, status). Clicking one opens the case. Include a button "Investigate" that will call the agent later (for now it loads the sample case).

2. Case file view, in this order from top to bottom:
   - Verdict banner: verdict, confidence, and recommended action. Use colour and a text label (never colour alone). Show clearly when the verdict is "needs_human" and why.
   - Summary in two lines.
   - Timeline: a vertical timeline of events. Clicking an event highlights its evidence.
   - Claims: each claim with its status. Each claim expands to show its evidence: the source, the exact SQL, and the returned rows in a small table.
   - Alibi attempts: a section titled "What could clear this customer?" showing each alibi the Devil's Advocate tried, and whether it was rejected, with the reason. This is our key differentiator, so make it prominent.
   - Agent trace: a collapsible step-by-step log of which agent did what, including rejections and retries.

Requirements: works at laptop and phone widths, readable text sizes, keyboard accessible, a loading state and an error state. Show a visible note that all data is synthetic and verdicts are recommendations for human review.

Do not include any API keys. Explain each file briefly after you create it.
```

## Rules to remember
- Build against the contract. If you want to change a field name, tell Joel and Keshinro first.
- Do the "What could clear this customer?" section well. It is what makes Alibi different.
- Work on your own branch (`excellence/web`) and open a pull request to `main`.
