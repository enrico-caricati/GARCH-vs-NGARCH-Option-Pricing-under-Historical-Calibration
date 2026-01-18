# GARCH vs NGARCH Option Pricing under Historical Calibration

This project implements and compares GARCH(1,1) and NGARCH volatility models for Monte Carlo pricing of European options. The focus is on understanding how asymmetric volatility dynamics affect the risk-neutral distribution of terminal prices and option payoffs, rather than on forecasting realised outcomes.

Models are calibrated using historical returns only and priced under a risk-neutral martingale constraint, without using market option prices.

# Data and Setup

- Underlying: SPY (daily closes)

- Risk-free rate: FRED (SOFR, with 3-month Treasury fallback pre-2018)

- Calibration: Rolling MLE on daily log-returns

- Pricing: Monte Carlo under the risk-neutral measure

- Options: European calls

- No option-chain data used

The market price of risk parameter is set to zero ($𝜆_𝑄 = 0$), as it is not identifiable without option prices

# Models
**GARCH(1,1):**
$ℎ_𝑡 = 𝜔 + 𝛼 𝜀_{t-1}^2 + 𝛽h_{t-1}$​

**NGARCH:**
$ℎ_𝑡 = 𝜔 + 𝛽ℎ_{𝑡−1} + 𝛼ℎ_{𝑡−1}(𝑧_{𝑡−1} − 𝜃)^2$

NGARCH introduces the parameter 𝜃, allowing volatility to respond asymmetrically to positive and negative shocks (“leverage effect”)

# Experimental Design

To highlight when NGARCH does and does not matter, four scenarios are evaluated on two different dates:

# Dates

**2019-06-03** — low-volatility, calm regime

**2022-06-13** — stressed regime with sustained negative shocks

# Contracts

1. ATM, 21 trading days

2. OTM (K = 1.05 × S₀), 126 trading days

**All simulations:**

- use the same pricing date for GARCH and NGARCH

- enforce the martingale condition

- are evaluated via terminal distributions and discounted payoffs

# Results

**1. 2019-06-03 (Calm Regime)**
**ATM, 21 days**

- GARCH and NGARCH terminal distributions are nearly identical

- Both models are tightly centred on the forward

- Discounted payoff distributions overlap almost completely

- Estimated NGARCH asymmetry parameter 𝜃 is small

**Interpretation:**
In a low-volatility regime with weak asymmetry, NGARCH collapses effectively to GARCH. No meaningful distributional advantage is expected or observed

**OTM, 126 days**

- Terminal price and payoff distributions remain very similar

- Both models produce comparable tail probabilities

- Differences are dominated by Monte Carlo noise rather than structural effects

**Interpretation:**
Even with longer maturity and tail-sensitive strikes, asymmetric volatility dynamics do not materially affect pricing when recent history lacks strong leverage effects

**2. 2022-06-13 (Stressed Regime)
ATM, 21 days**
- Both models remain centred on the forward (martingale holds)

- NGARCH produces slightly wider conditional variance paths

- Differences are visible but modest at short horizons

**Interpretation:**
Short maturity limits the time for asymmetric volatility dynamics to propagate

**OTM, 126 days**

- NGARCH exhibits a visibly wider right tail in the terminal log returns distribution

- Higher dispersion and fatter tails relative to GARCH

- Discounted payoff distribution shows greater mass in extreme outcomes

- Realised outcome lies deeper in the NGARCH distribution than under GARCH

**Interpretation:**
In stressed regimes, asymmetric volatility dynamics accumulate over longer horizons, materially affecting tail-sensitive option payoffs. This is the regime where NGARCH provides clear additional flexibility over GARCH

# Key Takeaways

- NGARCH does not systematically outperform GARCH in calm regimes

- Differences between models are regime-dependent

- NGARCH’s advantage emerges when recent returns contain sustained negative shocks, and the contract is sensitive to tail behaviour (OTM, longer maturity)

- Without option price data, improvements appear in distributional shape, not in payoff-based RMSE or MAE

- These findings are consistent with theoretical expectations

# Limitations

- No calibration to market option prices

- No estimation of variance risk premia

- Gaussian innovations only

- European options only

# Extensions

- Joint calibration to option chains

- Estimation of $𝜆_𝑄$

- Implied volatility surface comparison

- Alternative asymmetric volatility models (EGARCH, GJR-GARCH)
