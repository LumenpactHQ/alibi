# Case file contract

This is the one JSON shape the agents produce and the interface reads. Keshinro builds the agents to output it. Excellence builds the screens against it using the example below as fake data. If you want to change it, tell the other two first.

## Rules

- `verdict` is one of: `likely_fraud`, `likely_legitimate`, `needs_human`
- Every item in `claims` must list at least one `evidence_ids`. A claim with no evidence is invalid and the Devil's Advocate must reject it.
- Every item in `evidence` carries the exact SQL that produced it and the rows returned.
- `trace` records every agent step, including rejections and retries.
- All data is synthetic.

## Example

```json
{
  "case_id": "CASE-0001",
  "alert_id": "ALERT-1042",
  "generated_at": "2026-10-23T10:15:00Z",
  "verdict": "likely_fraud",
  "confidence": 0.82,
  "recommended_action": "Freeze account ACC-778 and contact the customer by phone",
  "summary": "Login from a new device 6 minutes before a transfer 9x the customer's average.",
  "claims": [
    {
      "claim_id": "C1",
      "text": "The transfer happened minutes after a login from a never-seen device.",
      "evidence_ids": ["E1", "E2"],
      "status": "accepted"
    }
  ],
  "evidence": [
    {
      "evidence_id": "E1",
      "source": "device_logs",
      "sql": "SELECT ... (the exact query that was run)",
      "rows": [{ "device_id": "DEV-991", "first_seen": "2026-10-23T09:55:00Z" }],
      "retrieved_by": "evidence_retriever"
    }
  ],
  "alibi_attempts": [
    {
      "alibi": "Customer may be travelling and using a new phone",
      "result": "rejected",
      "reason": "Customer profile shows no travel flag, and the login IP country matches home country.",
      "evidence_ids": ["E3"]
    }
  ],
  "timeline": [
    { "time": "2026-10-23T09:55:00Z", "event": "Login from new device DEV-991", "evidence_ids": ["E1"] },
    { "time": "2026-10-23T10:01:00Z", "event": "Transfer of 450,000 NGN to ACC-310", "evidence_ids": ["E2"] }
  ],
  "trace": [
    { "step": 1, "agent": "planner", "action": "listed 3 hypotheses", "output_summary": "account_takeover, mule_account, card_testing" },
    { "step": 2, "agent": "devils_advocate", "action": "rejected claim C2", "output_summary": "no evidence_ids" },
    { "step": 3, "agent": "evidence_retriever", "action": "ran 1 query", "output_summary": "E3 added" }
  ]
}
```

## Alert list shape (for the queue screen)

```json
[
  { "alert_id": "ALERT-1042", "amount": 450000, "currency": "NGN", "flagged_at": "2026-10-23T10:02:00Z", "reason": "amount above customer average", "status": "new" }
]
```
