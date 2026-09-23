# Scoring Model

The workflow deliberately separates **business rules** from **AI interpretation**.

## Deterministic score — max 80

### Role seniority — max 15
- Founder / Owner / C-level / VP / Head / Director: 15
- Manager / Lead: 10
- Other known role: 5
- Missing: 0

### Company size — max 15
- 20–500 employees: 15
- 5–19 or 501–1,000: 8
- Other / missing: 0

### Email type — max 10
- Business domain: 10
- Common personal domain: 0

### Budget — max 15
- 5,000+ in the submitted budget range: 15
- 2,500–4,999: 10
- 1,000–2,499: 5
- Lower / unknown: 0

### Timeline — max 15
- 0–30 days: 15
- 31–90 days: 12
- 91–180 days: 6
- Longer / unknown: 0

### Source / intent signal — max 10
- demo / consultation / contact-sales: 10
- website / inbound form: 6
- other: 2

## AI fit score — max 20

The LLM is asked to assess only signals that benefit from language understanding:

- clarity of the business problem
- urgency
- fit for an automation/AI service
- strength of buying intent

The model must return a score from 0 to 20 plus a short explanation.

## Combined score

`lead_score = deterministic_score + ai_fit_score`

The score is capped at 100.

## Routing

| Combined score | Tier | Default action |
|---:|---|---|
| 80–100 | HOT | Prepare follow-up draft; require human approval |
| 60–79 | WARM | Prepare follow-up draft; require human approval |
| 40–59 | NURTURE | Add to nurture queue |
| 0–39 | LOW | Log for later review |

The model does **not** control the routing thresholds.
