# Keshinro: agent loop

**Your job:** build the brain of Alibi. Five agents, one loop, one case file as output.

**Before you start:** read `README.md` and `docs/CASE_FILE_CONTRACT.md`. Your output must match the contract exactly. Open the repo in Cursor.

## Paste this into Cursor (Agent mode)

```
You are helping me build the agent core of "Alibi", a fraud-investigation agent for the Open Agent Hackathon 2026, Track 2 (The Agent That Can Explain Why).

Read README.md and docs/CASE_FILE_CONTRACT.md first. The case file JSON in the contract is the single output format. Do not change it.

Build everything inside /agents. Use Python 3.11. Keep it simple and readable.

1. A config module that loads settings from environment variables (see .env.example). Never hardcode keys.
2. A model client with one function: call_model(system_prompt, user_prompt) -> str. Make the provider configurable through env vars, because the model route (Meterless / NVIDIA) will be filled in later. Include a SAMPLE_MODE=true path that returns canned responses so we can develop offline.
3. A Zetaris client with one function: run_sql(sql) -> list[dict]. It POSTs to {ZETARIS_REST_URL}/sql-editor/sqls/run with headers X-Org-ID and X-Request-ID, and the API key as configured. Only allow SELECT statements. Reject anything else. In SAMPLE_MODE return rows from a local JSON file.
4. Five agents, each a small function with its own system prompt in a separate prompts file:
   - planner: reads an alert, lists hypotheses (account takeover, mule account, card testing, etc.), and assigns one query goal per hypothesis.
   - evidence_retriever: turns a query goal into one SELECT, runs it, and returns an evidence item (evidence_id, source, sql, rows).
   - pattern_analyst: builds claims from evidence. Every claim MUST list evidence_ids and a confidence.
   - devils_advocate: argues the transaction is legitimate and tries to find an alibi. It MUST reject any claim without evidence_ids and may request more evidence.
   - reporter: assembles the final case file in the contract format.
5. An orchestrator: planner -> retriever -> analyst -> devils_advocate. If the devils_advocate rejects a claim or asks for more evidence, loop back to the retriever and analyst, maximum 3 rounds. If the case is still weak after 3 rounds, the verdict is "needs_human".
6. A trace: every agent step is appended to the trace list (step, agent, action, output_summary), including rejections and retries. Save each run to /traces/<case_id>.json.
7. A command line entry point: python -m agents.run --alert ALERT-1042 prints the case file JSON.
8. Basic error handling: if a query fails, record it in the trace and continue. If the model returns invalid JSON, retry once.
9. A few unit tests for the SELECT-only guard and the "claim without evidence is rejected" rule.

Do not invent data. Do not hardcode any alert, account or verdict. Explain each file briefly after you create it.
```

## Rules to remember
- Every claim needs evidence. No exceptions.
- No keys in code. Use `.env`.
- Work on your own branch (`keshinro/agents`) and open a pull request to `main`.
- Tell Joel the moment the model route (Meterless / NVIDIA) details are ready so you can plug them in.
