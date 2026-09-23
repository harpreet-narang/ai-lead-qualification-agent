# Architecture

## Processing path

1. **Webhook intake** receives a lead.
2. **Normalize Lead** maps fields to a predictable schema and derives helper values.
3. **Validate Lead** rejects malformed submissions.
4. **Deterministic Score** applies visible ICP rules.
5. **AI Fit Assessment** interprets the lead's free-text message and returns a score from 0–20.
6. **Combine Score + Tier** calculates the final 0–100 score.
7. **Follow-up gate** creates a draft only for Hot/Warm leads.
8. **Human approval status** is set to pending rather than automatically sending the generated message.
9. **Optional Supabase persistence** stores the lead and score breakdown.
10. **Webhook response** returns the qualification result.

## Why this design?

The workflow avoids a common pattern where an LLM receives a lead and simply returns "qualified" or "not qualified."

That makes decisions difficult to audit and sensitive to prompt/model changes.

Here, deterministic rules remain explicit while AI is limited to semantic interpretation.

## Human-in-the-loop boundary

The portfolio version intentionally stops at a **pending human approval** state for high-priority follow-up.

During live testing, this can be connected to Telegram, Slack, email, or an n8n Wait/approval step without changing the scoring system.
