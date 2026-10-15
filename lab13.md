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

## Code understanding quiz — pending student response
In this line, what does `issuers.get(issuer, 0)` do, and why do we add `weight` to it?

```python
issuers[issuer] = issuers.get(issuer, 0) + weight
```

**Student answer:** [Pending. Answer before requesting the explanation.]

## Candidate D — AI-proposed attack
D holds 100% Cash and 0% of all other holdings. The checker confirms that D passes all five checks.

**AI-proposed objection:** S2 requires CPI + 4% net of costs over rolling five-year periods. Passing static constraints does not establish that cash can meet this objective. S10 also leaves the pilot's optimization criterion undefined. This is an objection about suitability and missing evidence, not proof that cash must miss the target.

### Independent judgment — pending
With AI closed, write two sentences judging whether the objection holds and identify the deciding statement.

**Student judgment:** [Pending.]

### Skeptical-colleague role — pending
This is an individual lab; play the skeptical colleague as directed rather than claiming a partner meeting occurred.

**Statement cited against D and reason:** [Pending student response.]

## One question for the CIO — pending
Write with AI closed.

**One question that blocks optimization:** [Pending student response.]  
**An answer that would change the portfolio, and how:** [Pending student response.]

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
