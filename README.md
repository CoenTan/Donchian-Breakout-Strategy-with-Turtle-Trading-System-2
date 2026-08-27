# Donchian-Breakout-Strategy-with-Turtle-Trading-System-2


Cross- Asset Volatility- Adjusted Donchian Breakout: A Turtle-Style Time-Series Momentum Strategy.
A brief report for this strategy. (All graphs are found within the code)
Introduction
Time-series momentum is the tendency of an asset’s own past returns to positively predict its future returns over a short to medium horizon. Moskowitz, Ooi and Pedersen (2012) document this effect across 58 liquid futures instruments, spanning equity indices, currencies, commodities and bonds. This project implements a specific time-series momentum strategy, a volatility-adjusted Donchian Channel breakout, which is structurally identical to the Turtle Trading System.
The strategy originates from one core observation where if an asset makes a new high relative to its recent trading range, that means that new buying pressure has entered the market, and the pressure often persists for a period rather than reversing immediately. The same logic applies in reverse for new lows too. It is a “Higher highs, lower lows” concept which follows a strict mechanical rule set.

Strategy Description
This strategy is a dual-moving extreme breakout system with volatility scaled position sizing. Each instrument is traded in 3 states at any instant, where it can be long, short, or flat (no trade).
Entry rules: Let N be the entry lookback. When the position is flat, if on day P the close exceeds the max close of the prior N days, a long position is opened. If the close is below the minimum close of prior N days, a short position is opened
Exit rules: Let M be the exit lookback, M is smaller than N as well. While long, the position closes if today’s close falls below the minimum close of the prior M days. While short, the position closes if today’s close rises above the maximum close of the prior M days.
M is shorter than N. A symmetric system using the same windows for entry and exits will only exit a long position on a full N-day reversal. This means a large portion of the gains will be erased before exiting. A shorter exit window reacts earlier, closing the trade sooner and preserves more of the gains.
Position sizing. Position size is scaled inversely to each instrument's volatility. This is measured by the Average True Range ATR over a 20 day rolling window. Position size on day t = target daily volatility / ATR% as of day t-1. We cap the leverage at 5x to prevent extreme position sizes on quiet days
No look-ahead bias. All quantities used to make day t’s decision, is computed with data up to t-1.
Parameters and Why No Tuning was Performed
The strategy has 3 free parameters: entry lookback N, exit lookback M and the ATR. The project uses fixed values taken directly from the published Turtle Trading System Two. N = 55 , M = 20 , ATR window = 20. None of the parameters were adjusted with the projects data included.
N controls how large a price move must be relative to recent history. Smaller N triggers more often and catches trends earlier at the cost of more false signals. A larger N triggers less often but each signal carries more weight.
M controls how quickly the strategy reacts to signs a trend is stalling. Smaller M means faster exits, but risking premature exits on temporary pullbacks.
The ATR window controls how quickly the volatility estimate adapts, a shorter window means faster reaction to a volatility spike but is noisier. A longer window is more stable but slower to reduce position size after a shock begins.


Why did I use fixed parameters?
The 55/20/20 configuration comes from a documented trading system developed on different instruments. All the results represent a genuine test of whether an unmodified, published rule holds up on this specific basket of futures, not whether a tuned parameter can get good results on a specific data set. This also satisfies the Occam’s Razor requirement.
Papers referenced and how each was incorporated.
Moskowitz, Ooi and Pedersen (2012) Time Series Momentum, Journal of Financial Economics.
The paper used 58 futures instruments and shows that a diversified portfolio of TSM strategies across asset classes delivers returns with little exposure to standard asset pricing factors, performing the best during extreme markets.
- We took the theoretical basis for expecting trend persistence in futures markets.
- We also took the simple 12 month benchmark and used it. The benchmark exists to test whether the added complexity of the Donchian breakout and ATR sizing makes sense to be added.
Baltas and Kosowski (2013) Momentum Strategies in Futures markets and Trend-Following Funds.
This paper studies TSM through the lens of commodity trading advisors, noting that the main driver of many managed futures strategies actually follows this style of trend-following. What was taken from this paper is less a specific formula and more institutional grounding, evidence that this approach is a simplified version of how real, professionally managed capital is deployed. It also reinforces the idea of volatility-based position sizing as standard practice.
Turtle Trading System, Dennis and Eckhardt (1983)
This is more of a trading system that has been documented. It supplied the exact operational rules in this project. System Two, 55 day entry, and 20 day exit, is used as the fixed
parameter set in this project. The concept of a shorter exit and a longer entry window is also taken from this system.

Findings
⅝ instruments , CL, GC, 6E, SI, 6J, showed an equal or improved Sharpe ratio out-of-sample compared to in-sample. Two, ES and ZN showed deterioration.For ZN, the sharpe fell from 0.282 to -0.824, and the annualised return fell from 2.12% to -7.00%, the max drawdown was also -32.81%. This is consistent with the known weakness of breakout systems where they may generate false entries.
Win rates were surprising as they clustered around 50-53% across every instrument and period. However, this was consistent with the theory of letting winning trades run longer than losing trades.
Combined Portfolio Findings
The combined portfolio’s Sharpe declined out of sample compared to in-sample from 0.531 to 0.209, and the annualised return declined from 2.67% to 0.81%.
Despite this, the max drawdown was -7.5% which was shallower than every instrument. This showed consistency with the Moskowitz, Ooi and Pedersen report.

Limitations & Optimisations 
Fixed, untuned parameters means the strategy is not adapted to the specific volatility or trending character of any individual instrument, a version tuned per instrument using only pre-2010 data might perform differently.
The combined portfolio averages all eight instruments equally, regardless of each instrument's own volatility. A low-conviction instrument like ZN carries the same weight as a strong performer like 6J. A risk-parity or Sharpe-weighted construction could improve the portfolio's risk-adjusted return, but was not implemented, equal weighting keeps the diversification result attributable to the strategy and asset mix itself rather than to a weighting scheme chosen after seeing the results.
Key Takeaways A fixed Turtle Trading System Two configuration, applied without modification across eight futures markets, produced a positive out-of-sample Sharpe ratio (0.209) and positive out-of-sample annualised return (0.81%) as a combined portfolio, despite individual results ranging from strongly positive (6E at 0.553) to strongly negative (ZN at -0.824) in the same period.
The diversification benefit predicted by Moskowitz, Ooi and Pedersen is directly observable in this project's own results. The combined portfolio's out-of-sample max drawdown (-7.50%) was smaller than every individual instrument's own out-of-sample drawdown, because instrument-specific losing periods did not coincide in time with losses in other instruments.
Performance broadly weakened out-of-sample relative to in-sample at the portfolio level (Sharpe 0.531 to 0.209), though not uniformly, five of eight instruments held up or improved.
This points toward a broader environment less favourable to trend-following generally in this specific window, rather than a failure specific to this implementation.
Win rates near 50% across every instrument and period confirm the strategy's returns, where positive, come from letting winning trades run longer than losing trades, not from high prediction accuracy. This is exactly how the strategy was designed to work, not an unexpected finding.


Overall, the results support the core thesis crafted by a simple, published, unmodified breakout rule, combined with volatility-based sizing and applied across a diversified basket, produces a more consistent risk profile than any single instrument on its own, even in a period where the underlying edge weakened.
Papers Cited
1. Moskowitz, T. J., Ooi, Y. H., and Pedersen, L. H. (2012). Time Series Momentum. Journal of Financial Economics, 104(2), 228-250.
2. Baltas, A.-N., and Kosowski, R. (2013). Momentum Strategies in Futures Markets and Trend-Following Funds.
3. Dennis, R., and Eckhardt, W. (1983). The Turtle Trading System.
