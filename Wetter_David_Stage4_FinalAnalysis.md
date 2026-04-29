# Stage 4 – Final Analysis & Hedge Recommendation
## EUR/USD FX Hedging Model — Scenario 3: U.S. Tech Services Firm

**Prepared by:** David Wetter  
**Date:** April 29, 2026  
**Model Version:** Stage 4 (Improved)  
**Audience:** CFO / Director of Treasury  
**LLM Used:** Claude (Anthropic), claude-sonnet-4-6  

---

## A. Exposure Summary

A U.S.-based technology services firm has invoiced a European client for services rendered, generating a **EUR 10,687,414.50 receivable** due in approximately 12 months (April 2027). This EUR amount was derived by dividing the $12,500,000 USD revenue target by the EUR/USD spot rate of **1.1696** as of April 29, 2026.

As a USD-functional-currency entity that is *long EUR*, the firm is fully exposed to **EUR/USD translation risk**. If the EUR depreciates against the dollar by the time the receivable settles, USD proceeds will be materially lower than the $12,500,000 originally targeted. At the current spot rate, converting the receivable yields **$12,500,000 exactly**. A 5% EUR depreciation (spot falling to ~1.1111) would reduce proceeds to approximately **$11,875,000** — a shortfall of **$625,000** relative to the current spot conversion. This is a material cash flow impact that directly affects earnings, budgeting, and liquidity planning.

The treasury function has been tasked with evaluating four hedging strategies to protect USD proceeds while identifying the optimal trade-off between cost certainty, downside protection, and upside participation.

**Key market data as of April 29, 2026:**

| Variable | Value | Source |
|----------|-------|--------|
| EUR/USD Spot (S₀) | 1.1696 | Bloomberg / Trading Economics |
| 1-Year Forward (bank-quoted) | 1.0910 | Assignment scenario |
| USD 1-Year Rate (R_USD) | 3.75% | FOMC (held steady April 29, 2026) |
| EUR 1-Year Rate (R_EUR) | 2.00% | ECB deposit facility (held March 2026) |
| CIP-Implied Forward | 1.1897 | Derived: 1.1696 × 1.0375/1.0200 |

> **Important note on the forward rate:** The bank-quoted forward of 1.0910 is significantly below both the current spot (1.1696) and the CIP-implied forward (1.1897). In standard covered interest parity, a USD rate of 3.75% and EUR rate of 2.00% would imply EUR/USD appreciates slightly to ~1.1897 over 12 months (reflecting the USD interest rate advantage). The 1.0910 bank quote likely reflects a specific counterparty pricing scenario, embedded risk premium, or a scenario assumption built into this exercise. Both rates are disclosed transparently, and the model uses the bank-quoted forward for the forward hedge strategy.

---

## B. Summary of Hedge Outcomes

The model computes four hedging strategies against the unhedged baseline. All proceeds are expressed in USD.

| Strategy | USD Proceeds | vs. Unhedged @ S₀ | Key Feature |
|----------|-------------|-------------------|-------------|
| **No Hedge** | $12,500,000 (at S₀) | — | Fully exposed to EUR/USD movement |
| **Forward Hedge** | $11,659,969 | −$840,031 | Certain; no upside; uses 1.0910 bank rate |
| **Money Market Hedge** | $12,714,461 | +$214,461 | CIP-consistent; synthetic forward at 1.1897 |
| **EUR Put Option** | $12,311,501 floor | −$188,499 vs S₀ | Floor + unlimited upside above 1.1696 |
| **EUR Call Option** | Reference only | — | Wrong direction for long-EUR receivable |

**Forward Hedge:** Locking in the bank-quoted forward rate of 1.0910 produces **$11,659,969** in USD proceeds — $840,031 *below* the current spot conversion. This strategy eliminates all uncertainty but at the steep cost of giving up the current spot value entirely. The bank-quoted forward implies a large EUR depreciation relative to current rates; entering this contract would crystallize a significant loss relative to the current spot. Unless the CFO is highly confident EUR will fall sharply, this forward appears unattractive relative to current spot.

**Money Market Hedge:** Borrowing EUR at 2.00%, converting at spot 1.1696, and investing in USD at 3.75% yields **$12,714,461** — the CIP-consistent result. This is $214,461 above the unhedged spot conversion, and is the theoretically superior certainty play. However, it requires increasing gross EUR debt on the balance sheet, adding leverage and liquidity considerations. Importantly, this result differs from the bank-quoted forward because the bank's 1.0910 rate is not CIP-derived from current market rates.

**EUR Put Option:** Buying a 1-year EUR put at strike 1.1696 (ATM) for $0.017/EUR establishes a worst-case **floor of $12,311,501** after future-valuing the $181,686 premium at R_USD. If EUR appreciates above 1.1696, the option expires worthless and the firm captures full upside — every cent of EUR appreciation above 1.1696 flows through to higher USD proceeds. The break-even versus the forward hedge occurs at approximately **S_T ≈ 1.1086**: above this level, the put strategy generates more value than the bank-quoted forward.

**EUR Call Option:** A call on EUR grants the right to *buy* EUR at 1.1696. For a firm that already *owns* EUR (via the receivable), buying a call provides no additional hedging benefit — it is the wrong instrument for this exposure. Documented for completeness only.

---

## C. Sensitivity Interpretation

The sensitivity table spans EUR/USD from **1.1111 to 1.2281** (±5% from current spot), covering approximately one standard deviation of 12-month EUR/USD movement at ~7–8% implied volatility.

**EUR Depreciation Scenarios (S_T < 1.1696):**  
The unhedged position suffers directly — at S_T = 1.1111, USD proceeds fall to $11,875,000, a $625,000 shortfall. The money market hedge holds firm at $12,714,461. The put option activates at strike and floors proceeds at $12,311,501. The bank-quoted forward, counterintuitively, *outperforms* the unhedged position in depreciation scenarios because it locks in 1.0910 — but only in scenarios where spot falls *below* 1.0910 (outside the ±5% range modeled).

**EUR Appreciation Scenarios (S_T > 1.1696):**  
The unhedged position benefits directly — every 1% EUR appreciation above spot adds approximately $125,000 in USD proceeds. The forward hedge sacrifices all of this upside, locked at $11,659,969. The money market hedge similarly caps at $12,714,461. The put option uniquely participates: as S_T rises above 1.1696, the option expires and net proceeds equal the unhedged value minus the future-valued premium (~$188,499). By S_T = 1.2281, the put generates $12,936,501 versus the forward's $11,659,969 — a $1.28M advantage.

**Key break-even:** The put option matches the bank-quoted forward at approximately **S_T ≈ 1.1086**. Only if EUR/USD falls below 1.1086 does the bank-quoted forward prove superior. Given current spot of 1.1696, this would require a ~5.2% EUR depreciation — a meaningful threshold.

---

## D. Strategic Recommendation

**Recommended Strategy: EUR Put Option (ATM, 1-year, strike 1.1696)**

The EUR put option is the optimal hedge for this firm's exposure. It provides:
1. A **hard floor** of $12,311,501 — ensuring minimum USD proceeds regardless of EUR movement
2. **Full upside participation** if EUR appreciates beyond 1.1696
3. A known, bounded cost of **$181,686 upfront** ($188,499 future-valued)

The bank-quoted forward at 1.0910 is the weakest alternative. It locks in proceeds $840,031 below the current spot conversion — effectively pre-accepting a large FX loss relative to today's rate, with no ability to recover if EUR holds or appreciates. Unless the treasurer has a strong directional view that EUR will fall below 1.1086 over the next year, this forward destroys value relative to both the put and the unhedged position across most of the ±5% scenario range.

The money market hedge produces the best *certainty* outcome at $12,714,461, but its balance-sheet implications (increasing gross EUR debt) and the absence of any upside participation make it a secondary choice. If the CFO prioritizes absolute certainty and has flexibility on leverage, the money market hedge is a compelling alternative.

---

## E. Executive Justification

**Cash Flow Stability:** The put option guarantees minimum USD proceeds of $12,311,501 — within $188,499 of the original $12,500,000 USD revenue target. This is a manageable insurance cost of approximately 1.5% of the receivable notional, providing the finance team a reliable budgeting floor.

**Budget Certainty:** The forward hedge offers certainty but at 1.0910 it crystallizes a $840,031 shortfall against the revenue target — making budget conversations with leadership materially more difficult. The put option preserves the $12,500,000 target as a realistic outcome if EUR holds at current levels.

**Liquidity Impact:** The put option requires an upfront premium of $181,686 — a manageable cash outflow relative to the $12.5M exposure. The money market hedge requires establishing a EUR credit facility, increasing gross debt and potentially consuming credit capacity needed elsewhere in the business.

**Optionality Value:** In the current macro environment, the EUR/USD rate faces genuine two-sided risk. The Middle East conflict has driven inflation higher in Europe, creating ECB tightening pressure that could support EUR. Simultaneously, the USD remains supported by the Fed's hold at 3.75%. The put option preserves the firm's ability to benefit from EUR strength — something neither the forward nor the money market hedge permits.

**Premium Cost vs. Protection:** The $0.017/EUR premium is the cost of asymmetric protection. Relative to the $0.079/EUR advantage the money market hedge offers over the bank-quoted forward ($12,714,461 vs. $11,659,969 ÷ EUR notional), the put premium is clearly efficient.

**Accounting Implications (Optional):** Under ASC 815 (U.S. GAAP) or IFRS 9, the put option may qualify for cash flow hedge accounting if properly designated and documented. This would allow mark-to-market changes to flow through OCI rather than P&L — reducing earnings volatility. The forward hedge is also eligible; the money market hedge is not a recognized derivative and is treated as a synthetic position. Hedge accounting documentation should be completed at inception.

---

## F. Structured AI Prompt

---

### AI Prompt: EUR/USD FX Hedge Model — Scenario 3

---

# GOAL

Build a professional, auditable EUR/USD FX hedging spreadsheet for a U.S. technology services firm. The model should analyze four hedging strategies — forward hedge, money market hedge, EUR put option, and EUR call option — for a EUR receivable due in 12 months. Use the exact variable values below and produce all sections specified.

---

# INPUT VARIABLES

Use these exact values. Define each as a named range in Excel. Do not infer, estimate, or substitute values.

| Named Range | Value | Description |
|-------------|-------|-------------|
| `FC_AMT` | 10,687,414.50 | EUR receivable = $12,500,000 ÷ 1.1696 |
| `S0_in` | 1.1696 | EUR/USD spot rate (April 29, 2026) |
| `F0_in` | 1.0910 | 1-year bank-quoted forward (given) |
| `R_USD` | 0.0375 | USD 1-year interest rate (3.75% — Fed funds upper bound, April 29, 2026) |
| `R_FC` | 0.0200 | EUR 1-year interest rate (2.00% — ECB deposit facility, March 2026) |
| `T_DAYS` | 365 | Days to settlement |
| `K_PUT` | 1.1696 | EUR put strike = ATM = S0_in |
| `PREM_PUT` | 0.0170 | Put premium per EUR (USD) |
| `K_CALL` | 1.1696 | EUR call strike = ATM = S0_in |
| `PREM_CALL` | 0.0220 | Call premium per EUR (USD) |

Additionally compute and display (but do not use as the primary forward rate):
- `F0_CIP` = `S0_in × (1 + R_USD) / (1 + R_FC)` — CIP-implied forward for reference and the money market hedge

---

# SPREADSHEET REQUIREMENTS

## Color Coding
Apply these consistently throughout:
- **Yellow fill** (RGB 255,255,0): All editable input cells (hardcoded values users will change)
- **Blue text** (RGB 0,0,255): Hardcoded numbers in input rows
- **Green fill** (RGB 226,239,218): Formula output cells and key results
- **Gray fill** (RGB 217,217,217): CFO dashboard summary outputs
- **White fill**: Intermediate calculation rows
- **Dark navy header** (RGB 31,56,100): Section headers (white text)

## Named Ranges
Define all 10 named ranges listed in the Input Variables table. Use them in all formulas — never hardcode values directly into calculations.

## Sheet Structure
Create two sheets:
1. **FX Hedge Model** — main model
2. **Notes & Assumptions** — all assumptions, sources, and limitations documented

---

# MODEL LOGIC

## Section 1 — Inputs & Assumptions
Display all 10 named range inputs in a table with columns: Parameter | Value | Units | Named Range | Source/Note. Include a row for `F0_CIP` as a derived reference value (not an input).

## Section 2 — Forward Hedge
```
[a] FC_AMT
[b] F0_in (bank-quoted forward)
[c] USD_forward = FC_AMT × F0_in  ← Primary output; certain regardless of S_T
```
Label [c] as the locked-in forward proceeds. Note that this uses the bank-quoted rate.

## Section 3 — Money Market Hedge
```
[a] BORROW_AMT = FC_AMT ÷ (1 + R_FC)          ← Expose as labeled named cell
[b] INVEST_AMT = BORROW_AMT × S0_in            ← Expose as labeled named cell
[c] USD_mm     = INVEST_AMT × (1 + R_USD)      ← Primary output
[d] F0_CIP     = S0_in × (1+R_USD)/(1+R_FC)    ← Display for reference
[e] Parity gap = USD_mm − USD_forward           ← Note: gap exists because F0_in ≠ F0_CIP
```
Include a note explaining that parity does not hold because the bank-quoted forward (1.0910) is not CIP-derived from current rates. The money market hedge uses the CIP rate, not the bank quote.

## Section 4 — EUR Put Option (Floor Strategy)
```
[a] PUT_PREM_TOTAL = PREM_PUT × FC_AMT         ← Upfront USD cost
[b] PUT_PREM_FV    = PUT_PREM_TOTAL × (1+R_USD) ← Future-valued to maturity
[c] USD_put_floor  = K_PUT × FC_AMT − PUT_PREM_FV ← WORST-CASE FLOOR (when S_T ≤ K_PUT)
[d] CALL_PREM_TOTAL = PREM_CALL × FC_AMT       ← Reference only
[e] CALL_PREM_FV    = CALL_PREM_TOTAL × (1+R_USD)
```
Label [c] clearly as the worst-case floor. Net put proceeds for any S_T:
- If S_T < K_PUT: exercise → net = K_PUT × FC_AMT − PUT_PREM_FV
- If S_T ≥ K_PUT: expire → net = S_T × FC_AMT − PUT_PREM_FV

## Section 5 — Sensitivity Table
Generate 11 scenarios: `S_T = S0_in × (0.95 + 0.01 × i)` for i = 0 to 10.
For each S_T, compute columns:
1. No Hedge (USD) = S_T × FC_AMT
2. Forward Hedge (USD) = USD_forward (constant)
3. Money Market (USD) = USD_mm (constant)
4. Put Option Net (USD) = IF(S_T < K_PUT, K_PUT × FC_AMT − PUT_PREM_FV, S_T × FC_AMT − PUT_PREM_FV)

Highlight the row where S_T ≈ S0_in (current spot) in light blue. Add annotation noting this is the current spot row.

## Section 6 — CFO Dashboard
Summary table with rows:
- Unhedged proceeds at S₀
- Forward Hedge locked-in proceeds + delta vs. unhedged
- Money Market locked-in proceeds + delta vs. unhedged
- EUR Put Option worst-case floor + delta vs. unhedged
- Hedge Recommendation (text: "EUR PUT OPTION")

---

# VERIFICATION

1. **CIP Check:** Display `F0_CIP = S0_in × (1+R_USD)/(1+R_FC)` and confirm it ≠ F0_in (no parity expected — this is correct and should be disclosed)
2. **Premium future-value check:** Confirm PUT_PREM_FV = PUT_PREM_TOTAL × (1+R_USD) before using in floor calculation
3. **Sensitivity check:** At S_T = K_PUT, put net proceeds should equal `K_PUT × FC_AMT − PUT_PREM_FV` (floor value)
4. **Named range audit:** All formulas must reference named ranges; no hardcoded rates in formula cells

---

# CHART

Produce a line chart titled "USD Proceeds by Strategy vs. EUR/USD at Maturity" with:
- X-axis: S_T values from sensitivity table
- Y-axis: USD Proceeds ($)
- Four series: No Hedge (red), Forward Hedge (blue), Money Market (green), Put Option (orange)
- Y-axis minimum: $10,500,000

---

# EXPORT

Save as `.xlsx`. Ensure all formulas recalculate correctly. Deliver with zero formula errors (#REF!, #DIV/0!, #VALUE!, #N/A, #NAME?).

---

*End of AI Prompt*

---

## Appendix: Extra Credit — Areas for Further Study

### 1. AI Skills & Automation

This project demonstrates a clear path to AI-augmented treasury workflows. A Claude Skill could be configured with the Stage 3 specification as its system prompt, enabling treasury analysts to regenerate the model on demand simply by providing updated market inputs — spot rate, interest rates, and option premiums. More powerfully, Claude's Code Interpreter capability could be connected to live Bloomberg or Refinitiv APIs to pull `S0_in`, `R_USD`, and `R_FC` automatically at model-open, eliminating manual data entry and reducing the risk of stale inputs. A Monte Carlo simulation layer could then replace the deterministic ±5% sensitivity table with a probabilistic distribution of outcomes, outputting not just point estimates but confidence intervals for each hedging strategy's payoff — a far more rigorous basis for CFO decision-making.

### 2. Multi-File Reasoning

The three-stage project structure (Stage 1 memo → Stage 3 spec → Stage 4 model) is precisely the kind of multi-document architecture where AI multi-file reasoning adds measurable value. An AI system given simultaneous access to the `.md` specification, the `.xlsx` model, and the Stage 1 memo could cross-validate that named ranges match the spec, that input values are consistent with the memo, and that sensitivity ranges cover the scenarios discussed in the narrative. This creates a closed feedback loop: any change to the spec automatically triggers a model consistency check, and any model output that contradicts the spec generates a flag for human review. In practice, this would allow a single treasury analyst to maintain a self-auditing model ecosystem that would previously have required a model review team.

### 3. GitHub & Version Control

Committing each stage artifact — the memo, the specification `.md`, the Excel model, and the AI prompt — to a GitHub repository creates a complete, timestamped audit trail of the analytical decision. For hedge accounting under ASC 815 or IFRS 9, documentation of hedge designation must exist *at inception*, and the version control timestamp on the commit provides exactly this evidence. A reviewer or auditor can `git diff` between model versions to identify exactly which input assumptions changed and when — a level of traceability that emailed spreadsheets cannot provide. For recurring hedges (quarterly receivables, for example), branching the repository per hedge period and tagging the model at each FOMC or ECB decision date creates a structured, reproducible archive that is far more defensible in audit than a folder of identically-named Excel files.

---

*Deliverable prepared as part of FIN [Course] Stage 4 — Final Analysis, Prompt Engineering & Recommendation*  
*Model file: `Wetter_David_Stage4_FXHedge_Scenario3.xlsx`*
