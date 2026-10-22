# Joel: Zetaris, data, repo, submission

**Your job:** get Zetaris running, create the synthetic fraud data, keep the repo clean, and own the submission.

## Order of work
1. Finish the Zetaris install (pull, up -d, verify, sign in, SELECT 1).
2. Create your own API key in Zetaris and put it in your local `.env`. Never share it.
3. Read the Meterless / NVIDIA setup section and tell the team the model route details (not the key in chat).
4. Build the synthetic data (prompt below).
5. Load the data into Zetaris and confirm a query works.
6. Keep the README checklist up to date. Record the demo video. Submit on HackOS on the 27th.

## Paste this into Cursor (Agent mode): synthetic data

```
You are helping me create synthetic data for "Alibi", a fraud-investigation agent for the Open Agent Hackathon 2026. Read README.md and docs/CASE_FILE_CONTRACT.md first. Work inside /data.

All data must be fully synthetic. No real names, phone numbers, or account numbers.

Write a Python script data/generate.py (standard library only, fixed random seed) that creates:

1. customers.csv: customer_id, name (fake), home_country, account_age_days, avg_transfer_amount, travel_flag.
2. transactions.csv: txn_id, account_id, customer_id, amount, currency (NGN), counterparty_account, channel, timestamp, device_id, ip_country.
3. device_logs.csv: device_id, customer_id, first_seen, last_seen, login_events.
4. watchlist.csv: account_id, reason, added_on (a handful of known mule accounts).
5. alerts.csv: alert_id, txn_id, flagged_at, reason.

Plant exactly 12 labelled scenarios, saved in data/scenarios.json with fields: alert_id, true_label (fraud or legitimate), pattern, and the planted evidence. Include:
- 4 account takeover cases (new device, then large transfer shortly after)
- 2 mule account cases (counterparty on watchlist or many small inbound transfers)
- 2 card testing cases (many tiny transactions then a large one)
- 2 legitimate false alarms with a valid alibi (customer travelling, known new phone)
- 2 ambiguous cases where the correct answer is "needs_human"

Add 500 or more normal background transactions so the agent has to find the signal.

Also write data/README.md explaining each file and column, and data/load_into_zetaris.md with simple steps to register the CSVs as tables in Zetaris (leave placeholders where I need to fill in exact Zetaris steps).

Finally write eval/run_eval.py that runs the agent on every alert in scenarios.json and reports how many verdicts match true_label, plus how many needed a human. We will report the measured numbers in the demo.

Explain each file briefly after you create it.
```

## Submission reminders
- Deadline: Tue 27 Oct, 23:45 UTC (Wed 28 Oct, 00:45 Lagos). Submit by the afternoon of the 27th.
- Demo video 1 to 4 minutes, shows Zetaris and Meterless doing real work.
- Declare any pre-existing components.
- Search the repo for secrets before the final push.
- Test a clean setup from the README on a fresh clone.
