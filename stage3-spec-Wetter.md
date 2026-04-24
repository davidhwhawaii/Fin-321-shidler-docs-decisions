# EUR/USD FX Hedging Model – Technical Specification

**Created by:** [Student Name]
**Updated by:** [Student Name]
**Date Created:** April 24, 2026
**Date Updated:** April 24, 2026
**Version:** 1.0
**LLM Used:** Claude (Anthropic)

**Role:** Financial Analyst / Treasury Analyst
**Audience:** CFO or Director of Treasury

**Purpose:** Provide a professional, quantitative specification documenting the Stage 2 FX hedge model and articulating a refined, improved design for Stage 4 implementation. This document serves as a machine-readable blueprint for reconstructing and enhancing the model.

---

## 1. Problem Statement

Our company expects to receive €1,200,000 in EUR-denominated export revenue in approximately 180 days (settlement date: October 21, 2026). Because revenue is invoiced in euros but our functional currency is USD, the company bears full translation risk on this receivable. A 5% adverse move in EURUSD (i.e., EUR depreciation) would reduce USD proceeds by approximately $65,340 relative to the current spot conversion, representing a material impact on operating cash flow for the quarter.

The objective is to evaluate four hedging strategies — forward hedge, money market hedge, EUR put option, and EUR call option — and identify which structure best balances certainty of proceeds, cost efficiency, and preservation of upside. This analysis is conducted by the corporate treasury function to support a hedging recommendation to the CFO.

---

## 2. Inputs (Known Variables)

All variables below are used in the Stage 2 Excel model and will be used in the Stage 4 AI-built model. Named ranges follow the standardized convention from the assignment specification.

| Named Range  | Description                        | Unit         | Value        | Source                  |
|--------------|------------------------------------|--------------|--------------|-------------------------|
| `FC_AMT`     | EUR-denominated receivable         | EUR          | 1,200,000    | Company accounts receivable |
| `S0_in`      | EURUSD spot rate at inception      | USD per EUR  | 1.0850       | Bloomberg, April 24, 2026 |
| `F0_in`      | 180-day EURUSD forward rate        | USD per EUR  | 1.0890       | Bank quote, April 24, 2026 |
| `R_USD`      | USD 180-day SOFR-based rate        | Annual %     | 5.25%        | Federal Reserve / Bloomberg |
| `R_FC`       | EUR 180-day EURIBOR-based rate     | Annual %     | 3.50%        | ECB / Bloomberg         |
| `K_PUT`      | EUR put option strike price        | USD per EUR  | 1.0850       | Set at-the-money (spot) |
| `K_CALL`     | EUR call option strike price       | USD per EUR  | 1.0850       | Set at-the-money (spot) |
| `PREM_PUT`   | EUR put premium per unit of FC     | USD per EUR  | 0.0172       | Black-Scholes approximation |
| `PREM_CALL`  | EUR call premium per unit of FC    | USD per EUR  | 0.0220       | Black-Scholes approximation |
| `T_DAYS`     | Days to settlement                 | Days         | 180          | Invoice terms           |

**Derived inputs:**

| Named Range  | Description                        | Unit         | Formula Basis                        |
|--------------|------------------------------------|--------------|--------------------------------------|
| `T_YRS`      | Time to maturity in years          | Years        | `T_DAYS / 360`                       |
| `BORROW_AMT` | FC amount to borrow for MM hedge   | EUR          | `FC_AMT / (1 + R_FC × T_DAYS/360)`  |
| `INVEST_AMT` | USD amount invested for MM hedge   | USD          | `BORROW_AMT × S0_in`                 |

---

## 3. Assumptions & Constraints

The following conventions govern all calculations in this model. Any deviation would require a corresponding update to the formulas described in Section 4.

- **Day-count basis:** All interest rate calculations use ACT/360 (i.e., `T_DAYS / 360`), consistent with USD and EUR money market convention.
- **Rate type:** Interest rates are quoted as simple annualized rates, not compounded. Rates are applied as `r × (T/360)`.
- **Forward-spot parity:** The model assumes covered interest rate parity holds — that is, the money market hedge should reproduce the same USD proceeds as the forward hedge. Any residual difference is attributed to rounding, not market arbitrage.
- **Option premiums:** Premiums are paid upfront in USD at time 0 and are not time-valued in the proceeds comparison. This understates the true cost of options by excluding the opportunity cost on premium capital.
- **Transaction costs:** Bid-ask spreads, bank fees, and credit charges are excluded from all calculations. This is a simplification; a production model would incorporate these.
- **No interim cash flows:** The model assumes a single settlement date with no coupon or interim payments on the underlying receivable.
- **Rate expression:** All exchange rates are expressed as USD per EUR (direct quotation from a US perspective). Spot rates at maturity (`S_T`) are varied as described in Section 6.
- **Tax treatment:** Hedge gains and losses are treated as pre-tax. No accounting treatment (e.g., ASC 815 hedge designation) is modeled.

---

## 4. Calculation Flow

The following describes the logical sequence for each hedge strategy, in the order they are computed. These steps are sufficient to reconstruct the model without reference to cell addresses.

**Step 1 — Unhedged Baseline**
Multiply `FC_AMT` by the spot rate at maturity (`S_T`) to obtain unhedged USD proceeds. This serves as the reference case in all sensitivity comparisons.

**Step 2 — Forward Hedge**
Multiply `FC_AMT` by `F0_in` to compute a fixed, certain USD amount. This is the lock-in rate and produces `USD_forward`. No further calculation is required; the result is independent of `S_T`.

**Step 3 — Money Market Hedge**
1. Compute the EUR borrowing amount: divide `FC_AMT` by `(1 + R_FC × T_DAYS/360)` to get `BORROW_AMT`. This is the present value of the receivable in EUR.
2. Convert to USD: multiply `BORROW_AMT` by `S0_in` to get `INVEST_AMT`.
3. Grow the USD investment to maturity: multiply `INVEST_AMT` by `(1 + R_USD × T_DAYS/360)` to get `USD_mm`.
4. **Parity check:** Confirm that `USD_mm ≈ USD_forward`. A discrepancy of more than $100 indicates an input error or parity violation.

**Step 4 — EUR Put Option Hedge**
1. Compute total put premium cost: multiply `PREM_PUT` by `FC_AMT` to get `TOTAL_PREM_PUT`.
2. For each value of `S_T` in the sensitivity range:
   - If `S_T < K_PUT`: option is exercised; receive `K_PUT × FC_AMT − TOTAL_PREM_PUT`
   - If `S_T ≥ K_PUT`: option expires; receive `S_T × FC_AMT − TOTAL_PREM_PUT`
3. This gives a floor on USD proceeds at `(K_PUT − PREM_PUT) × FC_AMT`.

**Step 5 — EUR Call Option Hedge**
1. Compute total call premium cost: multiply `PREM_CALL` by `FC_AMT` to get `TOTAL_PREM_CALL`.
2. For each value of `S_T` in the sensitivity range:
   - If `S_T > K_CALL`: option is exercised by counterparty; effective rate is `K_CALL`; receive `K_CALL × FC_AMT − TOTAL_PREM_CALL`
   - If `S_T ≤ K_CALL`: option expires; receive `S_T × FC_AMT − TOTAL_PREM_CALL`
3. This caps USD proceeds at `(K_CALL − PREM_CALL) × FC_AMT` for scenarios above the strike.

**Step 6 — Strategy Comparison**
For each `S_T` scenario, present all four strategy outcomes in a single row of the sensitivity table, enabling direct comparison across the range.

---

## 5. Outputs

The model produces the following specific results, tables, and charts. These are the target outputs for the Stage 4 AI model build.

| Output Name         | Description                                             | Format       | Purpose                                     |
|---------------------|---------------------------------------------------------|--------------|---------------------------------------------|
| `USD_forward`       | USD proceeds under forward hedge (fixed)                | Single value | Certainty benchmark for all comparisons     |
| `USD_mm`            | USD proceeds under money market hedge                   | Single value | Parity cross-check against `USD_forward`    |
| `Parity_check`      | Dollar difference between `USD_mm` and `USD_forward`   | Single value | Model validation; should be < $100          |
| `USD_put`           | USD proceeds from EUR put across all `S_T` scenarios   | Column (table)| Downside protection analysis                |
| `USD_call`          | USD proceeds from EUR call across all `S_T` scenarios  | Column (table)| Upside-cap / cost-of-insurance analysis     |
| `Sensitivity_Table` | All four strategies vs. `S_T` in one unified table     | 11-row table | Side-by-side comparison for all scenarios   |
| `Chart_1`           | Line chart: USD Proceeds by Strategy vs. `S_T`         | Line chart   | Visual identification of break-even points  |
| `Summary`           | Written CFO-ready conclusion and hedge recommendation   | 1–2 paragraphs | Decision support                          |

---

## 6. Model Review — What Worked & What to Improve

**What was built correctly:**
- The forward hedge and money market hedge calculations reproduce covered interest rate parity within an acceptable rounding tolerance (<$50 difference), confirming the underlying rate logic is sound.
- The sensitivity table correctly evaluates option payoffs across all 11 spot scenarios, and the line chart clearly illustrates the floor, cap, and linear payoff structures.
- Named ranges were used for all key inputs, making the model auditable and easy to update.

**What should be improved:**
- *Premium time value:* Option premiums should be future-valued to maturity (i.e., `PREM × (1 + R_USD × T/360)`) to produce a true apples-to-apples comparison with forward proceeds. The current model deducts premiums at face value.
- *Rate sourcing:* `R_USD` and `R_FC` were entered as static inputs. A more rigorous model would pull these from a market data sheet or flag them with a "last updated" date stamp.
- *Layout and labeling:* Several intermediate calculations (e.g., `BORROW_AMT`) were embedded in formulas rather than exposed as labeled cells, reducing auditability. In the improved version, all intermediate values should occupy labeled, named-range cells.
- *Missing scenario:* The current model does not include a zero-hedge (fully unhedged) baseline row in the sensitivity table. Adding this makes the cost of hedging explicit.
- *No break-even annotation:* The chart would be more informative with a vertical line or annotation marking the `S_T` at which each option strategy breaks even versus the forward.

---

## 7. Sensitivity Plan

Exchange-rate scenarios are constructed by varying `S_T` — the EURUSD spot rate at settlement — from 5% below spot to 5% above spot in increments of 1%, yielding 11 scenarios:

> `S_T ∈ { S0_in × 0.95, S0_in × 0.96, … , S0_in × 1.05 }`

With `S0_in = 1.0850`, this produces a range from approximately 1.0308 to 1.1393 USD/EUR.

For each scenario, the model computes USD proceeds under all four strategies and displays them in `Sensitivity_Table`. The accompanying line chart (`Chart_1`) plots each strategy as a separate line against the `S_T` axis. The most analytically useful comparisons are:

- **Forward vs. Put:** Illustrates what certainty costs relative to downside protection with upside retention.
- **Forward vs. Unhedged:** Quantifies the risk reduction achieved by locking in the forward rate.
- **Put vs. Call:** Demonstrates the difference in payoff structure between a floor (receivable hedger's tool) and a cap (payer hedger's tool), making clear that the EUR call is not the appropriate structure for this exposure.

The ±5% range is chosen to cover one standard deviation of approximately 6-month EURUSD movement based on recent implied volatility of ~7–8% annualized, scaled to 180 days.

---

## 8. Limitations & Next Steps

**Limitations of this specification and the underlying Stage 2 model:**

- Implied volatility is not explicitly modeled. Option premiums are taken as given rather than derived from a Black-Scholes or market-implied surface, which limits the model's utility for pricing.
- Dynamic hedging, delta-hedging, and rolling hedge strategies are outside scope.
- Credit risk (counterparty default on the forward contract or option) is not addressed.
- The model does not account for ASC 815 / IFRS 9 hedge accounting designation, which would affect how gains and losses are recognized in the income statement.
- Premium opportunity cost (the forgone return on premium capital) is excluded from the current comparison.

**Next steps:**

This specification will serve as the primary input to the Stage 4 AI prompt. The structured variable table (Section 2), step-by-step calculation flow (Section 4), and explicit output targets (Section 5) are designed to be directly translated into AI instructions. The model review notes (Section 6) will direct the AI to build the *improved* version — including future-valued premiums, a labeled intermediate-calculation section, and an unhedged baseline row — rather than replicating the Stage 2 prototype as-is.
