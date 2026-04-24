# EUR/USD FX Hedging Model – Technical Specification

**Created by:** David Wetter
**Updated by:** David Wetter
**Date Created:** April 1, 2026
**Date Updated:** April 24, 2026
**Version:** 1.0
**LLM Used:** Claude (Anthropic)

**Role:** Financial Analyst / Treasury Analyst
**Audience:** CFO or Director of Treasury

**Purpose:** Provide a professional, quantitative specification documenting the Stage 2 EUR/USD FX hedge model and articulating a refined, improved design for Stage 4 implementation.

---

## 1. Problem Statement

> Our company expects to receive €10,776,574 in EUR-denominated export revenue, settlement April 2027 (365-day horizon), exposing us to full translation risk on the EUR/USD rate. This receivable was derived from a $12,500,000 USD target divided by the April 1, 2026 spot rate of 1.1608. A 5% adverse EUR depreciation (to approximately 1.1028) would reduce USD proceeds by approximately $625,472 relative to current spot conversion — a material cash flow impact. This specification documents the analytical framework built in Stage 2 for quantifying, comparing, and evaluating four hedging strategies: forward hedge, money market hedge, EUR put option, and EUR call option (reference only).

Include:
- **Exposure type:** EUR receivable (long EUR; USD functional currency)
- **Amount and horizon:** €10,776,574 due April 2027 (365 days from April 1, 2026)
- **Objective:** Protect USD value of receivable; evaluate certainty vs. optionality trade-off
- **Decision context:** Corporate treasury / Risk Management; CFO memo support

> *A strong statement demonstrates clear understanding of both financial context and business implications.*

---

## 2. Inputs (Known Variables)

| Variable     | Description                          | Unit        | Value          | Source                                    |
|--------------|--------------------------------------|-------------|----------------|-------------------------------------------|
| `FC_AMT`     | EUR-denominated receivable           | EUR         | 10,776,574     | Stage 1 memo: $12,500,000 ÷ 1.1608       |
| `S0_in`      | EUR/USD spot rate at inception       | USD per EUR | 1.1608         | Bloomberg, April 1, 2026                  |
| `F0_in`      | 1-year EUR/USD forward rate          | USD per EUR | 1.1750         | CIP-derived: S₀ × (1+R_USD)/(1+R_FC)    |
| `R_USD`      | USD 1-year interest rate             | Annual %    | 3.75%          | Fed funds upper bound / 1-yr T-bill proxy |
| `R_FC`       | EUR 1-year interest rate             | Annual %    | 2.50%          | ECB deposit rate proxy                    |
| `T_DAYS`     | Days to settlement                   | Days        | 365            | Settlement April 2027                     |
| `K_PUT`      | EUR put option strike                | USD per EUR | 1.1608         | ATM — set equal to `S0_in`               |
| `K_CALL`     | EUR call option strike               | USD per EUR | 1.1608         | ATM — set equal to `S0_in` (reference)   |
| `PREM_PUT`   | Put premium per unit of FC           | USD per EUR | 0.0170         | Stage 1 memo; total upfront ≈ $183,202   |
| `PREM_CALL`  | Call premium per unit of FC          | USD per EUR | 0.0120         | Conservative estimate; not memo-stated    |

> *Tip:* All named ranges are defined directly in the Stage 2 workbook and used in formulas throughout. `F0_in` is CIP-derived and live — it recalculates automatically if `S0_in`, `R_USD`, or `R_FC` changes. All FX rates are expressed as USD per EUR (direct quotation, US perspective).

---

## 3. Assumptions & Constraints

State all conventions used. Clarity here ensures full reproducibility.

- Interest rates are quoted on a **simple annual basis**; applied as `r × (T_DAYS / 365)` given the 365-day horizon. No ACT/360 adjustment was made — a simplification relative to standard money market convention.
- The **forward rate is CIP-derived**, not sourced from a bank quote: `F0_in = S0_in × (1 + R_USD) / (1 + R_FC)`. This ensures exact parity with the money market hedge by construction.
- **Options are assumed European-style** — exercise at expiry only. Standard for OTC FX options.
- **Put premium is paid upfront in USD** at inception. The Stage 2 model future-values the premium to maturity using `R_USD` before netting against proceeds, enabling a fair comparison with the forward.
- **Call option included for reference only** — it is not the appropriate hedge for a EUR receivable and is not recommended as a primary strategy.
- **No bid-ask spreads, transaction costs, or credit spreads** are modeled. All rates are mid-market.
- **Flat yield curve assumed** — a single rate per currency; no term structure.
- **EUR borrowing rate equals the EUR risk-free rate** (2.50%) for the money market hedge. In practice, a corporate credit spread would apply, increasing the cost.
- **No margin, collateral, or balance-sheet effects** are modeled. The money market hedge increases gross debt — flagged for CFO review but not quantified here.
- **No tax treatment** — gains and losses are pre-tax throughout.

> *Write assumptions so another treasury analyst could replicate your results exactly.*

---

## 4. Calculation Flow

Describes the logical sequence of the model — as if briefing a junior analyst or AI model builder. Focus on **order of operations**, not cell references.

**Step 1 — Unhedged Baseline**
Multiply `FC_AMT` by spot rate at maturity (`S_T`) to compute unhedged USD proceeds. At current spot, this equals $12,509,447. This serves as the reference case in all sensitivity comparisons.

**Step 2 — Forward Hedge**
Multiply `FC_AMT` by `F0_in` to produce fixed USD proceeds (`USD_forward` = $12,662,001). The result is certain and independent of `S_T`. This is the primary certainty benchmark.

**Step 3 — Money Market Hedge**
1. Compute EUR borrowing amount: divide `FC_AMT` by `(1 + R_FC)` → `BORROW_AMT` = €10,513,731
2. Convert to USD at spot: multiply `BORROW_AMT` by `S0_in` → `INVEST_AMT` = $12,204,339
3. Grow USD investment to maturity: multiply `INVEST_AMT` by `(1 + R_USD)` → `USD_mm` = $12,662,001
4. **Parity check:** confirm `USD_mm − USD_forward = $0`. In the Stage 2 model, parity is exact because `F0_in` is CIP-derived — difference = $0 ✓

**Step 4 — EUR Put Option (Floor Strategy)**
1. Total upfront premium: `PREM_PUT × FC_AMT` = $183,202
2. Future-value the premium to maturity: `$183,202 × (1 + R_USD)` = $190,072
3. For each `S_T`: if `S_T < K_PUT`, exercise — net proceeds = `K_PUT × FC_AMT − $190,072`; if `S_T ≥ K_PUT`, let expire — net proceeds = `S_T × FC_AMT − $190,072`
4. Worst-case floor = $12,319,375 (when EUR/USD ≤ 1.1608)

**Step 5 — EUR Call Option (Reference Only)**
1. Total upfront premium: `PREM_CALL × FC_AMT` = $129,319; FV = $134,168
2. For each `S_T`: if `S_T > K_CALL`, counterparty exercises — proceeds capped at `K_CALL × FC_AMT − $134,168`; if `S_T ≤ K_CALL`, expires — proceeds = `S_T × FC_AMT − $134,168`
3. This strategy is directionally wrong for a EUR receivable holder and is documented for analytical completeness only.

**Step 6 — Strategy Comparison**
For each `S_T` scenario, all four strategy results are collected into a unified sensitivity table for side-by-side comparison across the ±5% range.

> *Your goal: anyone reading this section should know exactly how to implement this logic in Excel or code — without additional explanation.*

---

## 5. Outputs

| Output              | Description                                            | Format         | Purpose                                         |
|---------------------|--------------------------------------------------------|----------------|-------------------------------------------------|
| `USD_forward`       | Locked-in USD proceeds under forward hedge             | Single value   | Certainty benchmark: $12,662,001                |
| `USD_mm`            | USD proceeds under money market hedge                  | Single value   | Parity cross-check: $12,662,001                 |
| `Parity_check`      | Difference: `USD_mm − USD_forward`                     | Single value   | Validation output: $0 confirms CIP consistency  |
| `USD_put_floor`     | Worst-case net USD floor under EUR put                 | Single value   | Downside protection benchmark: $12,319,375      |
| `Sensitivity_Table` | All strategies vs. `S_T` across 11 scenarios          | 11-row table   | Side-by-side comparison across rate scenarios   |
| `Chart_1`           | Line chart: USD Proceeds by Strategy vs. `S_T`         | Line chart     | Visual identification of break-even and floor   |
| `CFO_Dashboard`     | Summary table: each strategy, USD amount, vs. baseline | Summary table  | Executive-ready decision support                |
| `Summary`           | Hedge recommendation narrative (Stage 4 placeholder)   | 1–2 paragraphs | CFO memo conclusion — to be completed Stage 4   |

> *Outputs should read like a professional financial dashboard — clear, repeatable, and decision-focused.*

---

## 6. Model Review — What Worked & What to Improve

**What was built correctly and operates as intended:**
- Forward and money market hedges produce exact parity ($0 difference) because `F0_in` is CIP-derived as a live formula rather than a hardcoded input — a deliberate and correct design choice.
- The put option premium is future-valued to maturity before netting, enabling an apples-to-apples comparison against forward proceeds. This is more rigorous than a face-value deduction.
- All 10 key inputs have named ranges defined in the workbook (`FC_AMT`, `S0_in`, `F0_in`, `R_USD`, `R_FC`, `T_DAYS`, `K_PUT`, `PREM_PUT`, `K_CALL`, `PREM_CALL`), making the model auditable and AI-readable.
- The CFO Dashboard (Section 6) provides a clean summary table with proceeds, delta vs. baseline, and strategy notes.

**What should be replaced in a more rigorous version:**
- *Day-count convention:* The model uses `T_DAYS / 365` implicitly but applies simple annual rates directly (i.e., multiplies by `1 + r`), which is equivalent to `T_DAYS = 365`. A more flexible model should apply `r × T_DAYS / 360` (ACT/360) or `r × T_DAYS / 365` (ACT/365) explicitly so the formula generalizes to non-annual horizons.
- *Call premium source:* `PREM_CALL = $0.012` is an assumed value not stated in the Stage 1 memo. The improved model should derive premiums from a Black-Scholes pricer or require a sourced market input.
- *EUR borrowing rate:* The money market hedge assumes borrowing at the risk-free ECB rate (2.50%). A credit spread should be added as a separate named input (`CREDIT_SPREAD`) to reflect realistic borrowing costs.
- *Call option labeling:* The current model includes the call option in the sensitivity table without clearly flagging it as reference-only. The improved version should label it explicitly and exclude it from the primary recommendation row.

**Naming, layout, and auditability improvements:**
- Intermediate money market values (`BORROW_AMT`, `INVEST_AMT`) should be exposed as labeled, named-range cells rather than embedded in chained formulas.
- A "last updated" date stamp should be added alongside each market-sourced input (`S0_in`, `R_USD`, `R_FC`).
- A break-even annotation should be added to `Chart_1` showing the `S_T` at which the put option net proceeds equal the forward — currently visible but unlabeled.

**Additional scenarios worth including:**
- A ±10% stress scenario extending the sensitivity table to capture tail risk.
- A premium sensitivity row showing how the put floor shifts if implied volatility — and thus `PREM_PUT` — changes by ±20%.

---

## 7. Sensitivity Plan

> Vary EUR/USD spot at maturity (`S_T`) from `0.95 × S0_in` to `1.05 × S0_in` in increments of `0.01 × S0_in`, producing 11 scenarios. With `S0_in = 1.1608`, the range spans **1.1028 to 1.2188 USD/EUR**. For each value, compute USD proceeds under all four strategies and the unhedged baseline. Present results as `Sensitivity_Table` and `Chart_1`.

The ±5% range covers approximately one standard deviation of 12-month EUR/USD movement based on implied volatility of ~7–8% annualized. The most analytically useful comparisons in `Chart_1` are:

- **Forward vs. Unhedged** — quantifies the $152,554 certainty gain at current spot, and the growing opportunity cost if EUR appreciates significantly
- **Forward vs. Put** — illustrates what the $190,072 premium buys: a $12,319,375 floor with unlimited upside above 1.1608
- **Put break-even vs. Forward** — the EUR put matches forward proceeds at approximately S_T ≈ 1.1784; above this level, the put strategy outperforms the forward

> *Professional analysts always test sensitivity — it shows how robust their recommendations are.*

---

## 8. Limitations & Next Steps

> This specification does not incorporate implied volatility modeling, Black-Scholes option pricing, dynamic or delta-hedging strategies, counterparty credit risk, or ASC 815 / IFRS 9 hedge accounting treatment. The call premium ($0.012/EUR) is assumed, not sourced. The money market hedge excludes the corporate credit spread on EUR borrowings and ignores balance-sheet leverage implications. Transaction costs and bid-ask spreads are excluded from all strategies.

The next phase is Stage 4: the named variable table (Section 2), step-by-step calculation flow (Section 4), explicit output definitions (Section 5), and model improvement notes (Section 6) will be translated directly into a structured AI prompt. The AI will build the *improved* version of the model — incorporating sourced option premiums, an explicit credit spread input, ACT/360 day-count flexibility, labeled intermediate cells, and a ±10% stress extension — rather than replicating the Stage 2 prototype as-is.

> *A well-written spec means the AI uses standardized variable names, generates correct auditable formulas, builds the improved version, and produces exactly the tables and charts specified above.*
