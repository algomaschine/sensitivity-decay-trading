# Whitepaper: From Arousal Decay to Edge Decay — A Unified Formulation for Markets

**Version:** 0.4  
**Author:** Eduard Samokhvalov  
**License:** MIT  
**Date:** 2025-02-19  

**Tags:** #SensitivityDecay #BiologicalBarrier #RegimeRotation #ArousalEconomics #TradingPsychology

---

## Abstract

We derive a mathematical model of **sensitivity decay** and **ceiling effect** from a well-known intimate dynamic: with one fixed "partner" (one stimulus source), responsiveness fades over repeated exposure; "standard" stimulation no longer suffices, and no amount of technique within the same pair can sustainably restore peak response—a **biological barrier**. We then map this formulation one-to-one onto trading: the "partner" becomes the strategy–market pair, arousal becomes edge, and overload becomes regime change. The result is a formal framework for when to rotate strategies, how to size positions, and why diversification across regimes is necessary.

This document incorporates **cutting-edge research (2024–2026)**:

- **Power-law decay** (neural adaptation, primary visual cortex, synaptic vesicle cycle) as an alternative to exponential decay
- **Optimal information gain** at intermediate habituation (eLife 2025, zebrafish)
- **Statistical jump models (JM)** for regime persistence with explicit jump penalties (2024)
- **Continuous regime probability** instead of discrete states (2024)
- **Hybrid ML regime detection** (HMM + XGBoost, RegimeFolio, 2025)
- **Pre-registration with Pre-Analysis Plan (PAP)** for credible inference (2024–2025)
- **Competitive arousal hypothesis** in markets (overbidding, bubbles)

The document specifies the full **design of experiments**, including **double-blind** protocols and **PAP** requirements for both initial validation and live or backtested trading.

---

## 1. The Jessicka Formulation (Intimate Domain)

Call the subject **Jessicka**. Her responsiveness to a single repeated stimulus (one partner, one context) is observed to decay over time; after saturation, only a **regime change** (new context, new intensity, or recovery period) restores sensitivity.

### 1.1 State Variables

- **Stimulus source (partner):** \( P \) — fixed for the duration of the relationship.
- **Context / regime:** \( C_t \in \{C_1, \ldots, C_K\} \) — e.g. same routine vs new setting vs stress vs recovery.
- **Exposure count:** \( n_{P,C}(t) \) — number of exposures to \( P \) in context \( C \) up to time \( t \).
- **Sensitivity (arousal responsiveness):** \( \sigma_{P,C}(n) \in [0,1] \), with \( \sigma_{P,C}(0) = 1 \).
- **Peak response (ceiling):** \( \bar{r}_{P,C} \) — maximum sustainable response with \( (P,C) \); cannot be exceeded by "more technique" alone.
- **Effective response:** \( r_{P,C}(t) = \bar{r}_{P,C} \cdot \sigma_{P,C}(n_{P,C}(t)) \).

### 1.2 The Decay Law (Habituation)

With repeated exposure to the same \( (P, C) \), sensitivity decays. Two formulations are supported by recent neuroscience:

#### 1.2.1 Exponential Decay (Classical)

\[
\sigma_{P,C}(n) = e^{-\lambda_{P,C} \, n}
\]

\( \lambda_{P,C} > 0 \): **decay rate** for this partner–context pair.

#### 1.2.2 Power-Law Decay (2023–2025, Neural Adaptation)

Recent research shows that **power-law decay better describes neural adaptation** than exponential decay:

- **Primary visual cortex:** Magnitude of population adaptation follows a power law with stimulus probability ratios (Nature Communications 2023).
- **Presynaptic vesicle cycle:** Power-law adaptation describes multi-timescale recovery (100 ms to 10+ s) in synaptic terminals (Nature Communications 2025).
- **Multi-timescale dynamics:** Multiple recovery steps create power-law structure that enables temporal whitening of spike trains.

\[
\sigma_{P,C}(n) = (1 + \beta_{P,C} \, n)^{-\eta_{P,C}}
\]

\( \beta_{P,C} > 0 \), \( \eta_{P,C} > 0 \): **power-law parameters**. For \( n \gg 1 \), \( \sigma \sim n^{-\eta} \). This captures **longer tails** and **multi-timescale** habituation observed in biological systems.

**Unified form:** Use either exponential or power-law depending on empirical fit; the market formulation below supports both.

### 1.3 Optimal Information Gain (eLife 2025)

A 2025 study (eLife) shows that **intermediate habituation maximizes information gain**—a trade-off between encoding information and energy consumption. Validated in zebrafish neural responses to repeated looming stimuli. Implication: the optimal rotation threshold \( \theta \) may lie at an **intermediate** \( \sigma \), not at full decay. Over-rotating (too frequent regime switches) wastes energy; under-rotating (staying too long) loses information. The model suggests \( \theta \in [0.3, 0.6] \) as a theoretically motivated range.

### 1.4 The Ceiling (Biological Barrier)

For a fixed \( (P,C) \), response is bounded:

\[
r_{P,C}(t) \leq \bar{r}_{P,C}
\]

No amount of technique can sustainably exceed \( \bar{r}_{P,C} \) without changing \( P \) or \( C \). That is the **biological barrier**.

### 1.5 Overload (Intensity Threshold)

When baseline stimulation is high, a stronger signal is needed to reach the same perceived intensity:

\[
\tau_t = \tau_0 \cdot (1 + \alpha \cdot B_t / \bar{B})
\]

\( B_t \): current baseline arousal; \( \bar{B} \): long-run average; \( \tau_0 \): base threshold. Only "overload" stimuli (above \( \tau_t \)) produce the desired response once habituated.

### 1.6 Recovery and Rotation

Sensitivity recovers when there is no exposure to \( (P,C) \) for a period \( \Delta t \):

\[
\sigma_{P,C}(n^+) \approx \sigma_{P,C}(n^-) + \gamma_{P,C} \cdot (1 - \sigma_{P,C}(n^-)) \cdot \Delta t
\]

\( \gamma_{P,C} \in (0,1) \): **recovery rate**. **Jessicka rule:** Rotate when \( \sigma_{P,C}(n) < \theta \) (e.g. \( \theta \in [0.3, 0.6] \) from optimal information gain).

---

## 2. Mapping to Markets (One-to-One)

| Intimate (Jessicka) | Trading |
|---------------------|---------|
| Partner \( P \) | Strategy \( S \) |
| Context \( C \) | Regime \( R \) |
| Sensitivity \( \sigma \) | Edge responsiveness |
| Peak response \( \bar{r} \) | Ceiling edge \( \bar\mu_{S,R} \) |
| Effective response \( r \) | Effective edge \( \mu_{S,R}(t) \) |
| Exposure count \( n \) | Number of trades/signals in \( (S,R) \) |
| Overload threshold \( \tau \) | Signal threshold |
| Rotation | Switch to \( (S', R') \) when \( \sigma_{S,R} < \theta \) |

---

## 3. Market Formulation

### 3.1 Decay Models

- **Exponential:** \( \sigma_{S,R}(n) = e^{-\lambda_{S,R} n} \), \( \sigma_{S,R}(0) = 1 \).
- **Power-law (2024+):** \( \sigma_{S,R}(n) = (1 + \beta_{S,R} n)^{-\eta_{S,R}} \).

### 3.2 Core Relations

- **Effective edge:** \( \mu_{S,R}(t) = \bar\mu_{S,R} \cdot \sigma_{S,R}(n_{S,R}(t)) \).
- **Ceiling:** \( \mu_{S,R}(t) \leq \bar\mu_{S,R} \).
- **Overload threshold:** \( \tau_t = \tau_0 \cdot (1 + \alpha \cdot V_t / \bar{V}) \); position size \( w_{S,R}(t) \propto \sigma_{S,R}(n_{S,R}(t)) \).
- **Rotation:** If \( \sigma_{S,R}(n_{S,R}(t)) < \theta \), switch to \( (S', R') \).
- **Allocation:** \( \pi_k(t) \propto \sigma_{S_k,R_k}(n_k(t)) \cdot \bar\mu_{S_k,R_k} \) (normalized).

### 3.3 Regime Detection (2024–2026)

#### 3.3.1 Statistical Jump Model (JM)

Traditional Markov-switching treats regime shifts as noise. **Statistical jump models (2024)** add an explicit **jump penalty** at each state transition, enhancing regime persistence. JM-guided strategies outperform HMM and buy-and-hold on volatility, drawdown, and Sharpe (US, Germany, Japan, 1990–2023).

#### 3.3.2 Continuous Regime Probability

Extended JM frameworks (2024) generalize discrete states to **continuous probability vectors** over regimes. This provides smoother transitions and robustness when regimes are imbalanced or data is limited—common in financial markets.

#### 3.3.3 Hybrid ML (2025)

- **RegimeFolio:** VIX-based regime classifier + Random Forest / Gradient Boosting ensemble. 137% cumulative returns, 1.17 Sharpe (2020–2024).
- **HMM + XGBoost / BaggingClassifier:** Hybrid voting for bull/bear/neutral. Macro + technical indicators.
- **Generative-discriminative:** HMM + kernel machines (SVM/MKL) for high-frequency regime classification.

**Recommendation:** Use JM or hybrid ML for regime classification; train only on data up to calibration end \( t_2 \); no retraining during holdout/live.

### 3.4 Competitive Arousal (2024)

Markets exacerbate emotional arousal when winning bids lead to substantial earnings, causing **overbidding and bubbles** (competitive arousal hypothesis). Implication: overload threshold \( \tau_t \) should account for **market-wide arousal** (e.g. VIX, volume, sentiment) to avoid overexposure during euphoric regimes.

---

## 4. Design of Experiments

All tests follow a **double-blind** protocol. **Pre-registration alone is insufficient** (2024): a detailed **Pre-Analysis Plan (PAP)** is required to reduce p-hacking and publication bias. Pre-registration with PAP shows evidence of reduced bias in empirical economics.

### 4.1 Pre-Registration and Pre-Analysis Plan (PAP)

**Before** opening the holdout window, the following are fixed and timestamped in a public registry (e.g. OSF, AEARCTR):

1. **Decay model choice:** Exponential vs power-law; if power-law, grid for \( \eta \).
2. **Thresholds:** \( \theta \), \( \alpha \) (or pre-registered grid).
3. **Exposure definition:** One trade, one signal, or one bar.
4. **Regime classifier:** Architecture, training period, no retraining during holdout.
5. **Primary metrics:** Sharpe, max drawdown, hit rate of rotation.
6. **Secondary metrics:** Volatility, turnover, regime persistence.
7. **Stopping rules:** When to halt the experiment.

No change to these is allowed after holdout data are observed.

### 4.2 Initial Validation (Double-Blind)

**4.2.1 Data split**

- **Training:** \( [t_0, t_1] \) — regime classifier, strategy–regime pairs. No performance stats for \( \theta \), \( \alpha \).
- **Calibration:** \( [t_1, t_2] \) — estimate \( \lambda \) (or \( \beta, \eta \)), \( \gamma \), \( \bar\mu \). Sealed.
- **Holdout:** \( [t_2, t_3] \) — single, pre-registered evaluation only.

**4.2.2 Blinding**

- **Blinded analyst:** Anonymous labels (Strategy A, B, C; Regime 1, 2, 3). Mapping sealed until after holdout evaluation.
- **Blinded estimation:** Second analyst receives only exposure counts and outcome series under anonymous labels. No identity of strategies/regimes or holdout period.
- **Unblinding:** After metrics are computed and recorded.

### 4.3 Trading Implementation (Double-Blind)

- **Strategy–regime anonymization:** Codes only; no human-readable names or historical performance to the engine.
- **Regime classifier:** Out-of-sample only; no retraining during holdout/live.
- **No look-ahead:** Exposure counts and \( \sigma \) use only information at \( t \).
- **Evaluator blinded:** No breakdown by strategy–regime until period locked.

### 4.4 Summary Table

| Phase | Who is blinded | To what |
|-------|----------------|--------|
| Initial validation | Second analyst | Strategy/regime identity; holdout data |
| Parameter estimation | Both analysts | Holdout window; code → name mapping |
| Trading / backtest | Trading engine | Strategy/regime names; future data |
| Regime classifier | — | Any data after \( t_2 \) |
| Evaluation | Evaluator | Breakdown until period locked |

---

## 5. Summary: The Sex–Market Formula

| Concept | Jessicka | Markets |
|---------|----------|---------|
| Decay (exp) | \( \sigma = e^{-\lambda n} \) | Same |
| Decay (power-law) | \( \sigma = (1+\beta n)^{-\eta} \) | Same |
| Ceiling | \( r \leq \bar{r} \) | \( \mu \leq \bar\mu \) |
| Effective payoff | \( r = \bar{r} \cdot \sigma \) | \( \mu = \bar\mu \cdot \sigma \) |
| Overload threshold | \( \tau_t = \tau_0(1 + \alpha B_t/\bar{B}) \) | \( \tau_t = \tau_0(1 + \alpha V_t/\bar{V}) \) |
| Rotation | when \( \sigma < \theta \) | when \( \sigma < \theta \) |
| Size | — | \( w \propto \sigma \) |

---

## 6. Limitations

- Parameters must be estimated; regimes are unobserved. This is research/backtesting material, not investment or personal advice.

---

## References (2023–2026)

1. **Power-law adaptation:** Nature Communications (2023) — primary visual cortex; Nature Communications (2025) — presynaptic vesicle cycle.
2. **Optimal information gain:** eLife (2025) — habituation in zebrafish.
3. **Statistical jump model:** arXiv/SSRN (2024) — regime-switching with jump penalty; Springer (2024) — continuous regime probability.
4. **RegimeFolio / hybrid ML:** arXiv (2025) — RegimeFolio; Springer (2025) — HMM + XGBoost.
5. **Pre-registration + PAP:** Journal of Political Economy (2024) — 15,992 test statistics; IZA (2024) — experimental economics.
6. **Competitive arousal:** HAL (2024) — emotional markets, overbidding, bubbles.
7. **Habituation mechanisms:** Nature Scientific Reports (2025) — anticipatory and reactive; PAIN (2024) — pain habituation.

---

**Repository:** [github.com/your-username/sensitivity-decay-trading](https://github.com)  
**Contact:** [Your LinkedIn / email]
