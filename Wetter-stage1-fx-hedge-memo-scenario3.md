# Memorandum: EUR/USD Foreign Exchange Exposure & Hedging Recommendation

**To:** Chief Financial Officer
**From:** [Your Name]
**Date:** April 1, 2026
**Re:** $12.5M EUR Receivable – FX Risk & Proposed Hedge Strategy
**Version:** 1.0 | **LLM Used:** Claude (Anthropic)

---

## Executive Summary

We are expecting a euro-denominated payment worth approximately **$12,500,000 USD** from a European client, due in **April 2027**. Because we invoice in EUR but report in USD, any weakening of the euro between now and settlement reduces our realized revenue — dollar-for-dollar.

EUR/USD is currently trading at **1.1608** (April 1, 2026). With meaningful macro uncertainty ahead — a Fed leadership transition, potential ECB rate hikes, and ongoing Middle East risk — leaving this exposure unhedged is not a passive choice; it is an active bet on euro stability. This memo outlines the exposure, quantifies the downside, and proposes three hedging options for your consideration. Detailed modeling will follow in Stages 2–4.

---

## The Exposure

| Detail | Value |
|---|---|
| Receivable (USD equivalent) | $12,500,000 |
| Receivable (EUR notional) | €10,776,574 |
| EUR/USD Spot Rate (Apr 1, 2026) | 1.1608 |
| 1-Year Forward Rate | 1.0910 |
| Settlement Date | April 2027 |
| USD Interest Rate | 3.75% |
| EUR Interest Rate | ~2.50% |

**What's at risk.** EUR/USD has already swung from 1.1417 to 1.2019 in 2026 alone — a range of over 600 pips. At those extremes, our USD proceeds on this receivable would vary by roughly **$650,000**. A more adverse move toward 1.08 — within historical range — would cost us over **$870,000** in lost revenue relative to today's spot. For a receivable of this size, that is a material earnings risk.

---

## Why This Matters Now

Three factors make the next twelve months particularly uncertain for EUR/USD:

1. **Fed policy transition.** Jerome Powell's term expires in May 2026. Markets expect the incoming chair to hold rates steady through mid-year before potentially cutting — creating near-term USD strength.
2. **ECB repricing.** The ECB cut rates three times in 2025, but surging energy costs have reversed that narrative. Markets are now pricing in two or more ECB hikes in 2026, which could support the euro — but timing and magnitude remain unclear.
3. **Geopolitical risk premium.** Escalating Middle East tensions have increased safe-haven USD demand, adding directional pressure on the pair that is difficult to model.

In short: rate differentials and geopolitics are pulling in opposite directions. That is precisely the environment where locking in certainty has the most value.

---

## Three Hedging Options

### 1. Forward Contract
Agree today to sell our EUR receivable at a fixed rate of **1.0910** in April 2027, guaranteeing **~$11,759,000** in USD proceeds regardless of where spot lands.

| | |
|---|---|
| **Advantage** | Complete certainty. No model risk, no ongoing decisions. |
| **Trade-off** | We forgo any upside if EUR strengthens above 1.0910. Locked in unconditionally. |

---

### 2. EUR Put Option (Floor Strategy)
Purchase the right to sell EUR at the current spot rate of **1.1608** for a premium of **$0.017/EUR** (~$183,000 upfront). This sets a worst-case floor on proceeds while preserving full upside.

| | |
|---|---|
| **Advantage** | Downside is capped; we benefit fully if EUR appreciates. Asymmetric protection. |
| **Trade-off** | The premium is a sunk cost — paid regardless of outcome. Reduces net proceeds by ~$183K at the floor. |

---

### 3. Money Market Hedge
Borrow EUR today at the EUR rate (~2.50%), convert to USD at spot, and invest those dollars at the USD rate (3.75%). Repay the EUR loan from the receivable at maturity. This replicates the forward synthetically without a derivatives contract.

| | |
|---|---|
| **Advantage** | No derivatives required; achieves a similar economic outcome to the forward. |
| **Trade-off** | Increases gross debt on the balance sheet; operationally more complex to execute and administer. |

---

## Next Steps

Stage 1 identifies the exposure and the options. The following stages will sharpen the analysis into a final recommendation:

- **Stage 2 – Excel Model:** Build a working model that computes net USD proceeds under each strategy across a range of EUR/USD outcomes, compared against the unhedged baseline.
- **Stage 3 – Technical Spec:** Document the model and design an improved version with sensitivity analysis and options pricing — precise enough to rebuild or hand off.
- **Stage 4 – Recommendation:** Select a strategy using model results, structure an AI-assisted prompt for hedge advisory support, and present a final CFO briefing.

We will return with a full recommendation and proposed execution timeline upon completion of the model.

---

## References

- Yahoo Finance. EUR/USD Live Rate (EURUSD=X), April 1, 2026. https://finance.yahoo.com/quote/EURUSD=X/
- U.S. Bank Asset Management. *Federal Reserve holds rates steady*, March 18, 2026. https://www.usbank.com/investing/financial-perspectives/market-news/federal-reserve-interest-rate.html
- Trading Economics. *Euro US Dollar Exchange Rate – EUR/USD*, 2026. https://tradingeconomics.com/euro-area/currency
- Course scenario data: Scenario 3 – U.S. Tech Services Firm (provided).
