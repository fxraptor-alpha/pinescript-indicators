# Fractal Model

`Fractal Model` is a TradingView tool for marking a multi-timeframe reversal sequence. It projects selected higher-timeframe candles beside the active chart, watches their highs and lows for a sweep, then maps the confirmation, entry zones and target levels that follow.

It is a visual-analysis tool. It does not place orders or guarantee a trade outcome.

## The Model In One View

```text
Higher-timeframe level -> sweep -> close back through the level -> C2 -> CISD -> C3 / C4 entry zones -> IC-CISD -> projections
```

The higher-timeframe high or low is the reference point. A sweep alone is not enough: the candle must close back through the swept level to create a reversal candidate. The script labels that candidate `C2`, then waits for a lower-timeframe change in delivery (`CISD`) before treating the setup as confirmed. After confirmation it keeps tracking the setup through `C3` and `C4`, and marks any later same-direction `IC-CISD`.

## Bullish Sequence

1. Price trades below a prior higher-timeframe low.
2. The active candle closes back above that low.
3. The script identifies the reversal candle as a bullish `C2` candidate.
4. A later close above the calculated CISD level confirms the move.
5. A C3 entry zone and, after C3 closes, a C4 entry zone are drawn.
6. Pullbacks that form a swing and break back up are marked as IC-CISD levels.
7. Optional liquidity lines, projection levels and SMT marks help frame the next upside objective.

## Bearish Sequence

1. Price trades above a prior higher-timeframe high.
2. The active candle closes back below that high.
3. The script identifies the reversal candle as a bearish `C2` candidate.
4. A later close below the calculated CISD level confirms the move.
5. A C3 entry zone and, after C3 closes, a C4 entry zone are drawn.
6. Pullbacks that form a swing and break back down are marked as IC-CISD levels.
7. Optional liquidity lines, projection levels and SMT marks help frame the next downside objective.

## What The Script Draws

### Higher-Timeframe Candle Projection

The panel on the right of the chart is a compact view of higher-timeframe candles. It gives the sweep logic a visible reference without requiring a separate chart window. Options add the HTF open line, vertical lines at each HTF candle open, high/low lines, the previous candle's EQ (50%), an HTF label with the remaining time of the current candle, and the interval name (for example the day of the week).

### Sweep And C2 Marking

When price sweeps a projected high or low and returns through it on close, the script marks the sweep and tracks the corresponding C2. With `Show live setup` on, a setup that is still forming is tracked in real time. A setup that later trades beyond its C2 extreme after the CISD is marked as `xC2`.

### CISD Level

The CISD line is the script's confirmation level. It is derived from the candles around the setup and acts as the boundary price must close through before the script considers the reversal confirmed.

### IC-CISD

After the main CISD confirms, the script keeps watching for a pullback against the move followed by a body close back through the open of the pullback's first opposing candle. That level is an IC-CISD. It is only marked when the pullback contains a three-candle swing, and the swing extreme is marked with a dot. Several IC-CISDs can form in a row. Tracking stops when the setup is invalidated or when C4 ends or is invalidated. IC-CISD lines use the same colors, width and style as the CISD.

### Entry Zones

- **C3 zone:** spans from the C2 midpoint to the middle of the CISD candle bodies when the C2 midpoint sits inside them, otherwise from the C2 midpoint to the C3 open. It grows with each lower-timeframe bar and locks when C3 ends.
- **C4 zone:** spans from the C3 midpoint to the C4 open. If price trades beyond the C3 extreme during C4, the zone is removed and the label changes to `xC4`.

### Projection Levels

After confirmation, the script can draw range-based projection levels from the setup, measured from the wick or the body. The default values are `0`, `1`, `-1`, `-2`, and `-2.5`, and any comma-separated list can be entered. Treat these as charting reference levels, not predicted prices.

### Formation Liquidity

The optional formation-liquidity lines mark selected C1/C0 levels and note when price reaches them.

### SMT Divergence

The script compares the chart with up to two correlated symbols. A bearish SMT is marked when the chart makes a higher high but a comparison symbol does not; a bullish SMT when the chart makes a lower low but a comparison symbol does not. Inverse correlations are handled. Lines are drawn at the chart symbol's own prices and labelled with the symbol that diverged. SMT is checked between C1 and C2, with an extra mark between C2 and C3 on a setup that is still valid.

- **Automatic:** picks the comparison symbols for the current chart.
- **Manual:** set symbol 1 and symbol 2 yourself (defaults `ES1!` and `NQ1!`), each with an `Inverse` option. Symbol 2 can be switched off.

### Fair Value Gaps

Optional boxes highlight fair value gaps.

## Alerts

1. Keep `Alerts?` on in the settings.
2. Right-click the indicator on the chart and choose **Add Alert** on this indicator.
3. Pick the alert name and click **Create**.

If you change any setting, delete the alert and create it again.

Alert messages read `LTF-HTF TICKER: [Potential] Bullish/Bearish <event>`, for example `5m-1H EURUSD: Bearish Candle 2`. The events are:

| Event | When it fires | Toggle |
| --- | --- | --- |
| `Potential ... Candle 2` | The CISD triggers while the setup is still forming | `Show live setup` → `Alert` |
| `Candle 2` | A setup is confirmed | `Alerts?` |
| `IC-CISD` | An IC-CISD confirms | `IC-CISD` → `Alert` |
| `SMT` | An SMT divergence is found | `SMT` → `Alerts` |

## Settings

| Setting | Default | What it changes |
| --- | --- | --- |
| `Fractal` | `Automatic` | HTF pairing: Automatic, Quarterly (quarterly-cycle timing), Custom, or a fixed pair from `1s-1m` to `1D-1M`. |
| `Custom Fractal` | `5` / `60` | Lower and higher timeframe when Fractal is Custom. |
| `Bias` | `Neutral` | Shows both directions, or only bullish or bearish setups. |
| `History` | `10` | How many past setups stay on the chart. |
| `HTF Candles` | `4`, Small | Number and size of projected HTF candles. |
| `Hide Projection?` / `Hide Setups?` | Off | Hide the candle panel, or show only the candles and lines. |
| `Show Sweeps` / `Show live setup` | On | Sweep lines and real-time tracking of forming setups. |
| `C2` / `C3` / `C4` labels | On | Stage labels on the chart. |
| `Show CISD` / `IC-CISD` | On | Confirmation lines and IC-CISD levels with swing dots. |
| `Entry Zone` | On | C3 and C4 entry-zone boxes. |
| `Enable Projections?` | Off | Projection levels after confirmation. |
| `Formation Liquidity` | On | C1/C0 liquidity lines. |
| `SMT` / `SMT Mode` | On, Automatic | Divergence marks against correlated symbols. |
| `Fair Value Gap` | On | FVG boxes. |
| `Calculate on Close?` | On | Detects live setups only on candle close, which reduces load. |

## Using It In TradingView

1. Open the [Pine Script source](fractal-model.pine) and copy it into a new TradingView Pine editor script.
2. Add the script to a chart.
3. Begin with automatic fractal selection and neutral bias.
4. Review a completed sweep, C2 and CISD before enabling projections.
5. Test the model across symbols and timeframes before relying on it in a trading workflow.

## Chart Examples

Add original, anonymized screenshots to `images/` and embed them here as the library grows. Useful first examples:

- `bullish-sequence.png`: sweep below a higher-timeframe low, C2, and CISD confirmation.
- `bearish-sequence.png`: sweep above a higher-timeframe high, C2, and CISD confirmation.
- `entry-zones.png`: C3 and C4 entry zones, including an `xC4`.
- `ic-cisd.png`: IC-CISD levels with swing dots after the main CISD.
- `smt.png`: an SMT divergence line and label.
- `projections.png`: projection levels after a confirmed setup.

Remove account names, orders, balances, personal annotations, and other identifying details before uploading an image.

## Scope And Attribution

This independent implementation was informed by publicly available trading education, including material published by TTrades. It is not affiliated with, endorsed by, or presented as the official TTrades Fractal Model. The explanation here describes this script's behavior in original language and does not reproduce third-party charts or course material.

Automatic SMT symbol pairing uses the public TradingView library `fstarcapital/AssetCorrelationUtils`.

The source includes an MPL 2.0 notice. This repository is for education and research, not financial or legal advice.
