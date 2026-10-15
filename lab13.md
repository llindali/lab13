# Lab 13 — Portfolio Mandate Defense
**Student:** Linda Li  
**Client:** Meridian Foundation Reserve (fictional)  
**Status:** Checker verified; student responses listed below remain pending.

## Decision question
Which portfolios does Meridian's mandate permit, and what must the CIO answer before anyone optimizes?

## S1–S10 classifications
Student-supplied classifications, with wording clarified after review.

| Statement | Type | Reason |
|---|---|---|
| S1 | Hard constraint | Establishes a seven-year horizon and annual rebalancing requirement. |
| S2 | Objective | Targets CPI + 4% annually, net of costs, over rolling five-year periods. |
| S3 | Hard constraint | Requires four quarterly payouts of $0.75 million in reserve-eligible holdings: $3 million, or 3% of $100 million. |
| S4 | Objective | Establishes the 60/40 performance benchmark; it does not require exact 60/40 holdings or specify a tracking-error limit. |
| S5 | Hard constraint | The client mandate restricts the portfolio to long-only positions and bans borrowing, short sales, and derivatives. |
| S6 | Hard constraint | Caps combined debt and equity exposure to a single corporate issuer at 8%, with the stated exemptions. |
| S7 | Hard constraint | Caps annual turnover at 25% unless the committee approves more. |
| S8 | Hard constraint | Requires at least 90% redeemable within five business days. |
| S9 | Unverifiable promise | The loss measure and breach response are unspecified; a holdings checker cannot guarantee future calendar-year losses. |
| S10 | Missing | The optimization criterion and tradeoffs have not been specified; no sector, ESG, tax, or home-bias preference is expressed. |

## Checker and verification
Run with Python 3, using the standard library only:

```bash
python mandate.py
```

Reserve minimum = 4 × 0.75 / 100 × 100 = **3.0%**.

| Check | A | B | C | D |
|---|---:|---:|---:|---:|
| Total weights (%) | 100.0 pass | 100.0 pass | 100.0 pass | 100.0 pass |
| Lowest weight (%) | 0.0 pass | 0.0 pass | 0.0 pass | 0.0 pass |
| Reserve-eligible (%) | 14.0 pass | 1.0 FAIL | 2.0 FAIL | 100.0 pass |
| Redeemable within five days (%) | 100.0 pass | 85.0 FAIL | 100.0 pass | 100.0 pass |
| Largest single company (%) | 0.0 pass | 9.0 FAIL | 8.0 pass | 0.0 pass |
| Failed checkable rules | 0 | 3 | 1 | 0 |

For every candidate, calendar-year loss **requires CIO clarification**. A passes every checkable rule; that is not full investment approval. Acme bonds and stock are aggregated, so B's corporate exposure is 9%. The Treasury 10-year note is reserve-eligible under the supplied table.

### Negative-weight probe
Tested a copy of A with Cash = -2 and Global equity index fund = 64.
Results: total 100.0 pass; lowest weight -2.0 FAIL; reserve 10.0 pass; five-day liquidity 100.0 pass; largest company 0.0 pass.
Only long-only fails. The saved candidate A retains Cash = 2 and equities = 60.

### Scope of the checker
This static weight checker tests the five rules requested in the assignment. It does not establish the return target, future losses, annual rebalancing, historical turnover, or the absence of derivatives/borrowing beyond the supplied holdings representation. Those require additional evidence or clarification.

## Code understanding quiz — completed
In this line, what does `issuers.get(issuer, 0)` do, and why do we add `weight` to it?

```python
issuers[issuer] = issuers.get(issuer, 0) + weight
```

**Student answer:** This line accumulates the total portfolio weight for each unique issuer in the `issuers` dictionary. `issuers.get(issuer, 0)` returns the accumulated weight if the issuer already exists, or 0 if it does not, avoiding a `KeyError`. Adding the current holding's weight updates that issuer's total.

Acme bonds and stock share the same `Acme Corp` key, so their weights are combined. In candidate B, 5% bonds plus 4% stock equals 9.0%. Each holding individually is below 8%, but their combined issuer exposure exceeds the limit and correctly produces FAIL.

**Feedback after the student's answer:** Correct. The limit applies to aggregate corporate issuer exposure, not each security separately.

## Candidate D — AI-proposed attack
D holds 100% Cash and 0% of all other holdings. The checker confirms that D passes all five checks.

**AI-proposed objection:** S2 requires CPI + 4% net of costs over rolling five-year periods. Passing static constraints does not establish that cash can meet this objective. S10 also leaves the pilot's optimization criterion undefined. This is an objection about suitability and missing evidence, not proof that cash must miss the target.

### Independent judgment — student response and review
With AI closed, write two sentences judging whether the objection holds and identify the deciding statement.

**Student judgment:** The objection holds because holding 100% cash guarantees a real purchasing power shortfall that fails the foundation's intertemporal growth requirements. Statement S2 decides it, as cash yields cannot mathematically achieve an annualized return of CPI + 4% net of costs over rolling five-year periods.

**Review note:** S2 is the relevant statement, but a guaranteed shortfall or mathematical impossibility is not established by the supplied case. Cash yields, inflation, and costs are not given. The supported objection is that passing the checker does not demonstrate that 100% cash can meet S2; the CIO could reasonably reject D for lacking a credible basis for meeting that objective.

### Skeptical-colleague role — pending
This is an individual lab; play the skeptical colleague as directed rather than claiming a partner meeting occurred.

**Statement cited against D and reason:** [Pending student response.]

## One question for the CIO — pending
Write with AI closed.

**Student-supplied design consequences:**
1. If the CIO chooses tracking error minimization against the benchmark, the optimizer will anchor tightly around 60% global equities and approximately 37% core bonds, leaving 3% in reserve-eligible cash/short Treasuries, regardless of whether that mix meets CPI + 4% in low-return environments.
2. If the CIO defines S9 as a hard tail-risk budget, such as 95% one-year VaR at most 10%, while maximizing return toward CPI + 4%, the optimizer will de-risk away from broad equities toward short-to-intermediate fixed income and cash equivalents, accepting greater benchmark tracking error.

**Review notes:** These are conditional design ideas, not computed allocations. Exact minimum-tracking-error weights require a specified risk model, and a reserve allocation need not be exclusively cash/short Treasuries under the supplied table. A 95% VaR limit is a probabilistic model constraint: it does not prevent all losses above 10%, and the resulting allocation depends on the model and feasibility. The available non-fund Treasury holdings are bills and a 10-year note; a new intermediate-maturity holding is outside the lab's table.

**One question that blocks optimization:** [Still pending student wording.]  
**Answer that changes the design:** The alternatives above provide the student's proposed consequences, subject to the review notes.

## Investment policy statement — learning notes
An investment policy statement (IPS) records a client's investment objectives, constraints, decision responsibilities, and monitoring framework. It guides portfolio construction and provides a reference for evaluating results and handling exceptions.

Here, S2 states the return objective and S4 supplies its performance reference. S1 and S3 define horizon, rebalancing, and payout needs. S5–S8 constrain implementation, concentration, turnover, and liquidity. S9 expresses a downside concern requiring clarification before it can become an operational policy. S10 identifies missing optimization direction and the absence of additional preferences. The classification table maps each statement individually.

## Reflection — pending student responses
1. **Why can a portfolio pass every checkable rule and still be wrong for this client?** [Pending.]
2. **Which S1–S10 classification were you least sure of, and why?** [Pending.]

## Project 2 client mandate — pending client selection
AI assistance is permitted for this section. No client choice has yet been supplied.

- **Client:** [Pending.]
- **Objective:** [Pending.]
- **Hard constraints:** [Pending.]
- **One question for the client:** [Pending.]

## Submission
Submit individual GitHub links to this Markdown file and `mandate.py` through the Lab 13 quiz after completing the pending responses.

Source: [Lab 13 instructions](https://github.com/CinderZhang/FIN43900-Fall2026/blob/main/lessons/week-08/lab-13-portfolio-mandate.md).
