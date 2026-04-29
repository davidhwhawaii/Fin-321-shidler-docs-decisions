# EUR/USD FX Hedging Analysis — Executive Memo
**Stage 4: Final Analysis, Prompt Engineering & Recommendation**

**To:** Chief Financial Officer
**From:** David Wetter, Treasury / Risk Management
**Date:** April 29, 2026
**Re:** EUR €10,776,574 Receivable — FX Hedge Strategy Recommendation
**Horizon:** Settlement April 2027 (365-day horizon)

---

## A. Exposure Summary

Our company is set to receive **€10,776,574** from an international counterparty in April 2027. This amount was derived from a **$12,500,000 USD revenue target** at the April 1, 2026 EUR/USD spot rate of **1.1608**. As a USD-functional-currency entity that is long EUR, we are fully exposed to EUR/USD translation risk between now and settlement.

The core risk is straightforward: if the EUR weakens against the USD over the next 12 months, our realized dollar proceeds will fall short of the $12,509,447 currently implied by the spot rate. Under a 5% EUR depreciation scenario (EUR/USD falling to approximately 1.1028), unhedged USD proceeds drop to roughly **$11,883,975** — a **$625,472 shortfall** relative to today's spot conversion. For a single receivable of this size, that represents material cash flow risk that warrants active management.

The treasury function has evaluated four hedging strategies — forward hedge, money market hedge, EUR put option, and EUR call option — to determine the optimal balance of cost, certainty, and upside participation.

---

## B. Summary of Hedge Outcomes

All figures are drawn from the Stage 2 quantitative model. The baseline unhedged proceeds at current spot are **$12,509,447**.

| Strategy | Locked-In / Floor Proceeds | vs. Unhedged Baseline | Key Characteristic |
|---|---|---|---|
| **Forward Hedge** | $12,662,001 | +$152,554 | Full certainty; zero upside |
| **Money Market Hedge** | $12,662,001 | +$152,554 | Synthetic forward; balance-sheet impact |
| **EUR Put Option (floor)** | $12,319,375 (worst case) | −$190,072 at floor | Floor protection; full upside preserved |
| **EUR Call Option** | Not applicable | Reference only | Wrong direction for a receivable holder |
| **No Hedge** | Market-dependent | Varies by ±$625K+ | Full upside and full downside |

### Forward Hedge
The forward hedge locks in **$12,662,001** regardless of where EUR/USD trades at maturity. At today's CIP-derived 1-year forward rate of **1.1750**, this represents a **$152,554 gain** over current spot conversion and eliminates all FX uncertainty. The trade-off is complete: the company sacrifices any benefit from EUR appreciation in exchange for total cash flow certainty.

### Money Market Hedge
The money market hedge replicates the forward synthetically by borrowing the present value of the EUR receivable (€10,513,731) today, converting it to USD at spot ($12,204,339), and investing in USD instruments for 12 months. The USD investment compounds to **$12,662,001** at maturity — exactly equal to the forward hedge, confirming covered interest parity (difference = $0). The primary distinction is operational: the money market hedge increases gross debt on the balance sheet and requires immediate USD investment, which has liquidity and leverage implications. CFO review of balance-sheet treatment is advised before selecting this route.

### EUR Put Option (Floor Strategy)
The ATM EUR put (strike: 1.1608) costs **$0.017/EUR**, or a total upfront premium of **$183,202** (future-valued to $190,072 at maturity). In exchange, the company receives a **worst-case floor of $12,319,375** if EUR weakens below the strike, while retaining full upside participation if EUR strengthens. At current spot (unchanged EUR/USD), net proceeds after premium are $12,319,375 — below the forward and below the unhedged baseline. The option only outperforms the forward hedge when EUR/USD exceeds approximately **1.1784** at maturity (the break-even point against the forward).

### EUR Call Option (Reference Only)
A EUR call option — the right to *buy* EUR — is directionally incorrect for a company that needs to *sell* EUR at maturity. This strategy is documented for analytical completeness but is not considered a primary hedge candidate. At current rates, selling a call would expose the company to uncapped losses on EUR appreciation while providing a modest credit of $134,168 — an asymmetric and inappropriate risk profile for this exposure.

---

## C. Sensitivity Interpretation

The sensitivity table spans EUR/USD at maturity (S_T) from **1.1028 to 1.2188** — a ±5% range around the April 1 spot of 1.1608 — across 11 scenarios.

**EUR Depreciation Scenarios (S_T < 1.1608):**
Under depreciation, the forward hedge and money market hedge both outperform the unhedged position by a growing margin — up to **$778,027** at the 5% depreciation extreme (S_T = 1.1028). The put option's floor kicks in, delivering a fixed $12,319,375 in all depreciation scenarios, which is better than the unhedged outcome but below the forward. In other words: if EUR weakens materially, the forward provides the best outcome, while the put provides meaningful but not complete protection.

**EUR Appreciation Scenarios (S_T > 1.1608):**
Here the picture reverses. The forward hedge caps proceeds at $12,662,001 — the company cannot benefit from EUR strength. The unhedged position and the put option, by contrast, both capture upside. At the 5% appreciation extreme (S_T = 1.2188), unhedged proceeds reach **$13,134,919** and put-protected proceeds reach **$12,944,848** (net of premium) — outperforming the forward by **$472,918** and **$282,847** respectively. This illustrates the fundamental trade-off: certainty versus optionality.

**Key Insight — Break-Even Analysis:**
The EUR put strategy equals forward proceeds at approximately **S_T ≈ 1.1784**. Above this level, the put strategy outperforms the forward; below it, the forward wins. Since the 1-year forward rate is itself 1.1750, the put must overcome only a **0.0034 appreciation move** beyond the forward to justify its premium — a relatively modest hurdle, but one the EUR must actually achieve for the premium to pay off.

---

## D. Strategic Recommendation

**Recommended Strategy: EUR Put Option (with a secondary consideration of the Forward Hedge)**

For this exposure, the EUR put option represents the most strategically appropriate hedge. The rationale is as follows:

The EUR/USD forward rate of 1.1750 already prices in a **$152,554 premium** over current spot — meaning the market expects modest EUR appreciation over the next 12 months. Locking in that rate with a forward hedge captures a known gain, but eliminates participation in any further EUR strength. Given the current macro environment — where the USD rate differential (3.75% vs. 2.50% EUR) continues to compress — there is a credible scenario in which EUR/USD moves toward or above 1.18–1.20 by April 2027, a range that would generate materially better outcomes under the put strategy.

The put premium of $190,072 (future-valued) represents approximately **1.5% of the EUR notional**, which is a reasonable cost for 12-month downside protection. The resulting **$12,319,375 floor** ensures the company never receives less than 98.5% of spot-equivalent proceeds, regardless of EUR movement.

If cash flow certainty is the overriding priority — for example, if the $12.5M USD target is a hard budget commitment — the **forward hedge** is the appropriate fallback. It eliminates all variability and delivers a confirmed $12,662,001 with no premium cost.

A **combination strategy** (partial forward + partial put) could also be considered to balance certainty and optionality — for example, hedging 50–60% via forward and buying puts on the remainder — though this adds execution complexity.

---

## E. Executive Justification

**Cash Flow Stability:** The put option provides a guaranteed minimum USD inflow of $12,319,375, preventing any budget shortfall driven by EUR depreciation. The $190,072 premium is a known, fixed cost that can be budgeted precisely.

**Budget Certainty:** The floor of $12,319,375 is within 1.5% of the current spot-equivalent, which is well within typical treasury risk tolerance for a single 12-month receivable. If the CFO requires a hard minimum, the forward hedge at $12,662,001 provides an even tighter lock.

**Liquidity Impact:** The put premium ($183,202) is paid upfront. This is the primary liquidity consideration — a moderate cash outflow at inception. The forward hedge, by contrast, requires no upfront cash and may be preferable if current liquidity is constrained.

**Optionality Value:** Unlike the forward, the put preserves unlimited participation in EUR appreciation. Given the CIP-implied forward of 1.1750 and the potential for further EUR strength, forgoing upside via a forward hedge carries real opportunity cost. The put monetizes this optionality.

**Accounting Implications (Note):** Hedge accounting treatment under ASC 815 or IFRS 9 has not been analyzed in this model. If the company wishes to designate either hedge as a qualifying cash flow hedge (with changes reported through OCI rather than P&L), formal effectiveness testing and documentation would be required. This is flagged for accounting team review.

---

## F. Structured AI Prompt

### Appendix: AI Prompt — EUR/USD FX Hedging Spreadsheet

---

```
# GOAL

Build a professional, audit-ready EUR/USD FX hedging workbook in Excel (.xlsx) modeling four
strategies: Forward Hedge, Money Market Hedge, EUR Put Option, and EUR Call Option (reference only).
Include a sensitivity table, CFO dashboard, parity verification, and a line chart. Follow all
specifications below exactly.

---

# INPUT VARIABLES

All values are explicitly defined. Do not infer or estimate any missing data.

| Named Range  | Value         | Units       | Description                                        |
|--------------|---------------|-------------|--------------------------------------------------- |
| FC_AMT       | 10,776,574    | EUR         | EUR-denominated receivable                         |
| S0_in        | 1.1608        | USD/EUR     | EUR/USD spot rate at inception (April 1, 2026)     |
| F0_in        | [CIP formula] | USD/EUR     | 1-year forward rate — derive as S0_in*(1+R_USD)/(1+R_FC); do NOT hardcode |
| R_USD        | 0.0375        | Annual      | USD 1-year interest rate (Fed funds / T-bill proxy)|
| R_FC         | 0.0250        | Annual      | EUR 1-year interest rate (ECB deposit rate proxy)  |
| T_DAYS       | 365           | Days        | Time to settlement                                 |
| K_PUT        | 1.1608        | USD/EUR     | EUR put strike (ATM = S0_in)                       |
| PREM_PUT     | 0.0170        | USD/EUR     | EUR put premium per unit                           |
| K_CALL       | 1.1608        | USD/EUR     | EUR call strike (ATM = S0_in; reference only)      |
| PREM_CALL    | 0.0120        | USD/EUR     | EUR call premium per unit (assumed; reference only)|
| CREDIT_SPREAD| 0.0000        | Annual      | Add-on to R_FC for money market hedge borrowing (set to 0 for base case; expose as editable input) |

---

# MODEL LOGIC

## Sheet 1: "FX Hedge Model"

### Section 1 — Inputs & Assumptions
- Place all named ranges in a clearly labeled input table (columns: Parameter, Value, Units, Named Range, Source/Note)
- F0_in must be a live formula: =S0_in*(1+R_USD)/(1+R_FC)
- Color code: Yellow background for editable inputs (FC_AMT, S0_in, R_USD, R_FC, T_DAYS, K_PUT, PREM_PUT, K_CALL, PREM_CALL, CREDIT_SPREAD)
- Blue background for derived/assumption cells (F0_in)
- Add a "Date Sourced" column next to S0_in, R_USD, and R_FC for auditability

### Section 2 — Forward Hedge
- Step [a]: EUR notional = FC_AMT
- Step [b]: Forward rate = F0_in
- Step [c]: USD_forward = FC_AMT * F0_in
- Label result: "Locked-In USD Proceeds (Forward)"
- Format cell as Green (formula output)

### Section 3 — Money Market Hedge
- Step [a]: BORROW_AMT = FC_AMT / (1 + R_FC + CREDIT_SPREAD) — expose BORROW_AMT as a named, labeled cell
- Step [b]: INVEST_AMT = BORROW_AMT * S0_in — expose as named, labeled cell
- Step [c]: USD_mm = INVEST_AMT * (1 + R_USD)
- Parity check row: USD_mm - USD_forward (label: "Difference — must equal $0")
- Note balance-sheet impact in an adjacent comment cell

### Section 4 — EUR Put Option (Floor Strategy)
- Step [a]: Total premium upfront = PREM_PUT * FC_AMT
- Step [b]: FV_PUT_PREM = [a] * (1 + R_USD) — future-value premium to maturity
- Step [c]: USD_put_floor = K_PUT * FC_AMT - FV_PUT_PREM
- Step [d]: For sensitivity table — IF(S_T < K_PUT, K_PUT * FC_AMT - FV_PUT_PREM, S_T * FC_AMT - FV_PUT_PREM)
- Label floor result: "Worst-Case USD Floor (Put Option)"

### Section 5 — EUR Call Option (Reference Only)
- Step [a]: Total call premium = PREM_CALL * FC_AMT
- Step [b]: FV_CALL_PREM = [a] * (1 + R_USD)
- Step [c]: Net proceeds if S_T > K_CALL: K_CALL * FC_AMT - FV_CALL_PREM (capped upside)
- Step [d]: Net proceeds if S_T <= K_CALL: S_T * FC_AMT - FV_CALL_PREM
- Label clearly: "EUR Call — Reference Only. Not recommended for a receivable hedge."

### Section 6 — Sensitivity Table
- Vary S_T from 0.95*S0_in to 1.05*S0_in in increments of 0.01*S0_in (11 rows)
- ALSO include a ±10% stress extension (S_T from 0.90*S0_in to 1.10*S0_in, 21 rows total — place in adjacent columns)
- Columns: S_T | No Hedge | Forward | Money Market | Put Option (net) | Call Option (ref)
- Highlight the S_T = S0_in row (current spot) in bold
- Format all USD columns as $#,##0

### Section 7 — Parity & Verification Checks
- Confirm USD_forward = USD_mm (display as: "Parity Confirmed: $X" or "PARITY ERROR")
- Confirm F0_in = S0_in * (1+R_USD) / (1+R_FC) (display formula and result)
- Flag any non-zero parity difference in red

### Section 8 — CFO Dashboard (Gray outputs)
- Summary table: Strategy | USD Proceeds | vs. No Hedge @ S0 | Notes
- Include: Unhedged (at S0), Forward, Money Market, Put Floor, Put at S0 (net of premium)
- Leave a "Hedge Recommendation" row as a text placeholder (to be completed by analyst)
- Gray background for all output cells

---

# CHART REQUIREMENTS

- Create a line chart titled: "USD Proceeds by Strategy vs. EUR/USD at Maturity"
- X-axis: S_T values (11 scenarios, ±5% range)
- Y-axis: USD Proceeds ($)
- Series: No Hedge | Forward Hedge | Money Market | Put Option (net)
- Add a data label or annotation marking the break-even point where Put = Forward
- Format gridlines, axis labels, and legend professionally

---

# FORMATTING

- Color coding:
  - Yellow background: Editable inputs
  - Blue background: Derived assumptions (e.g., F0_in)
  - Green font/background: Formula outputs
  - Gray background: Summary/output cells
- Font: Arial 10pt for data; Arial 11pt bold for section headers
- Column widths: Parameter column ≥ 35 chars wide; number columns formatted to $#,##0 or 0.0000
- Freeze top rows (row 1–3) and first column
- Add print area covering Sections 1–8 and chart

---

# SHEET 2: "Notes & Assumptions"
- Document all rate sources with dates (Bloomberg, ECB, Fed)
- List all simplifying assumptions (no bid-ask, flat yield curve, European-style options, no credit spreads in base case, no tax)
- Explain money market hedge balance-sheet flag
- Note: PREM_CALL is assumed, not market-sourced; recommend Black-Scholes pricing for production use
- Note: T_DAYS = 365 assumes ACT/365 simple interest; flag ACT/360 as an alternative for bank money markets

---

# VERIFICATION

Before delivering the model, confirm:
1. USD_forward = USD_mm (parity check passes, difference = $0)
2. F0_in formula is live (changes when R_USD or R_FC changes)
3. Sensitivity table uses IF() formulas, not hardcoded values
4. All named ranges are defined in Excel Name Manager
5. No formula errors (#REF!, #DIV/0!, #VALUE!, #NAME?)
6. Chart updates automatically when S0_in changes

---

# EXPORT

- Save as: Wetter-david-stage4-model-final.xlsx
- Include both sheets: "FX Hedge Model" and "Notes & Assumptions"
- All formulas must be live — do not paste-values-only
```

---

## Extra Credit: Areas for Further Study & Improvement

### 1. AI Skills & Automation

This project's workflow — from inputs to model to memo — is well-suited for AI automation at each stage. Tools like Claude with Code Interpreter could be configured to pull live EUR/USD spot and forward rates from a Bloomberg or Refinitiv API at model open, automatically updating `S0_in`, `R_USD`, and `R_FC` without manual entry. This would eliminate a common source of error (stale rate inputs) and make the model production-ready rather than point-in-time. Beyond live data, Monte Carlo simulation could replace the static 11-scenario sensitivity table with a full distribution of outcomes — sampling from historical or implied EUR/USD volatility to generate probability-weighted proceeds for each strategy. This would allow the CFO to understand not just what happens at 5% depreciation, but the likelihood of each outcome. A Claude Skill configured with this prompt and the Stage 3 spec could regenerate the entire workbook on demand by retrieving current market data, running the model, and producing an updated memo — a fully automated treasury reporting pipeline triggered by a single prompt.

### 2. Multi-File Reasoning & GitHub Version Control

The three-stage artifact set — Stage 2 spreadsheet, Stage 3 specification, Stage 4 prompt — forms a coherent, reproducible package when committed to GitHub. Version control allows any future analyst (or AI agent) to trace the exact model logic, inputs, and decisions at each stage, providing a clear audit trail. An AI system with access to all three files simultaneously could perform consistency checks automatically: verifying that named ranges in the spreadsheet match the Stage 3 spec, that the Stage 4 prompt encodes the correct formulas, and that any change to the spec propagates correctly into the prompt and model rebuild. This multi-file reasoning capability is directly analogous to how Big 4 audit teams use version-controlled workpapers — every assumption, calculation, and recommendation is tied to a specific document version, enabling reproducibility and defensibility. GitHub also supports branching for scenario variants (e.g., a stress-test branch with ±10% sensitivity) while preserving the baseline as an auditable reference, satisfying documentation requirements under ASC 815 and IFRS 9 hedge accounting standards.

---

*Prepared by: David Wetter | Treasury / Risk Management | April 2026*
*Model: Stage 2 EUR/USD FX Hedge Workbook | Spec: Stage 3 Technical Specification v1.0*
