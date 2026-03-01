# Curve_analysis

This repo/notebook analyzes Curve 3pool behavior during the March 2023 USDC depeg (USDC traded down to ~0.88). The focus is on pool inventory dynamics, LP downside exposure (proxy), and mechanism sensitivity to the amplification parameter A.


## Data source
	•	Dune on-chain queries (Curve 3pool dashboard), pulled via Dune API into Colab.


## Methods 
### 1.	Post-mortem (inventory + imbalance)
    •	Plot pool composition (% shares) over time.
  	•	Define an imbalance metric: imbalance_L1 = Σ|share_i-1/3|
  	•	Identify the peak stress timestamp (pool becomes a “USDC warehouse”, USDT nearly depleted).
### 2.	LP downside proxy (static MTM stress)
  	•	Stress revaluation under static USDC repricing (e.g., USDC=$0.90 or $0.88): V(t) = USDT(t) + DAI(t)+USDC(t)*p
    •	Report normalized drawdowns V(t)/V(t0)-1
    •  not realized LP token PnL (no virtual_price, no time-varying USDC price path).
### 3. A sensitivity (mechanism-level)
    •	Approximate peak 3pool state as a USDC/USDT 2-coin StableSwap slice for numerical stability.
    •	Simulate USDC→USDT swaps across trade sizes for A in {20,100,500}.

## Key findings
	•	During the depeg window, 3pool becomes extremely one-sided (USDC share spikes; USDT liquidity is nearly drained), consistent with a bank-run dynamic.
	•	Under static USDC discounting scenarios, the MTM proxy shows large drawdowns when inventory is skewed toward the depegged asset.
	•	Higher A reduces slippage under imbalance (better UX), but can also lower the cost of draining scarce “good” liquidity (USDT) in stress regimes—suggesting A should be paired with imbalance-aware fees and emergency guards.

## Limitations
	•	MTM plots are static stress proxies, not realized LP token returns.
	•	A sensitivity uses a 2-coin slice approximation at peak stress for stability; it captures one-sided drainage economics but is not a full 3-coin on-chain quote replication.





