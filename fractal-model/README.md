# Fractal Model

`Fractal Model` is a TradingView tool for marking a multi-timeframe reversal sequence. It projects selected higher-timeframe candles beside the active chart, watches their highs and lows for a sweep, then maps the confirmation and target levels that follow.

It is a visual-analysis tool. It does not place orders or guarantee a trade outcome.

## The Model In One View

The script looks for this sequence:

```text
Higher-timeframe level -> sweep -> close back through the level -> C2 -> CISD -> projections / trade plan
```

The higher-timeframe high or low is the reference point. A sweep alone is not enough: the candle must close back through the swept level to create a reversal candidate. The script labels that candidate `C2`, then waits for a lower-timeframe change in delivery (`CISD`) before treating the setup as confirmed.

## Bullish Sequence

1. Price trades below a prior higher-timeframe low.
2. The active candle closes back above that low.
3. The script identifies the reversal candle as a bullish `C2` candidate.
4. A later close above the calculated CISD level confirms the move.
5. Optional liquidity lines, projection levels, and position-sizing boxes help frame the next upside objective.

## Bearish Sequence

1. Price trades above a prior higher-timeframe high.
2. The active candle closes back below that high.
3. The script identifies the reversal candle as a bearish `C2` candidate.
4. A later close below the calculated CISD level confirms the move.
5. Optional liquidity lines, projection levels, and position-sizing boxes help frame the next downside objective.

## What The Script Draws

### Higher-Timeframe Candle Projection

The panel on the right of the chart is a compact view of higher-timeframe candles. It gives the sweep logic a visible reference without requiring a separate chart window.

### Sweep And C2 Marking

When price sweeps a projected high or low and returns through it on close, the script marks the sweep and tracks the corresponding C2. A setup that later breaks its invalidation point can be marked as `xC2`.

### CISD Level

The CISD line is the script's confirmation level. It is derived from the candles around the setup and acts as the boundary price must close through before the script considers the reversal confirmed.

### Projection Levels

After confirmation, the script can draw range-based projection levels from the setup. The default values are `0`, `1`, `-1`, `-2`, and `-2.5`. Treat these as charting reference levels, not predicted prices.

### Formation Liquidity And Position Sizer

The optional formation-liquidity lines mark selected C1/C0 levels. The position sizer uses the CISD, C2 extreme, and configured risk/reward ratio to draw a visual entry, stop, and target plan.

## Settings To Start With

| Setting | Suggested starting point | What it changes |
| --- | --- | --- |
| `Fractal` | `Automatic` | Selects the higher-timeframe relationship. |
| `Bias` | `Neutral` | Shows bullish and bearish setups. |
| `Show Sweeps` | On | Shows sweep lines and live setup tracking. |
| `Show CISD` | On | Shows the confirmation level. |
| `Enable Projections` | Off at first | Adds range-based target references after confirmation. |
| `Enable Position Sizer` | Off at first | Adds visual entry, risk, and reward boxes. |
| `Calculate on Close` | On | Uses confirmed candles and can reduce live-chart workload. |

## Using It In TradingView

1. Open the [Pine Script source](fractal-model.pine) and copy it into a new TradingView Pine editor script.
2. Add the script to a chart.
3. Begin with automatic fractal selection and neutral bias.
4. Review a completed sweep and C2 before enabling projections or position sizing.
5. Test the model across symbols and timeframes before relying on it in a trading workflow.

## Chart Examples

Add original, anonymized screenshots to `images/` and embed them here as the library grows. Useful first examples:

- `bullish-sequence.png`: sweep below a higher-timeframe low, C2, and CISD confirmation.
- `bearish-sequence.png`: sweep above a higher-timeframe high, C2, and CISD confirmation.
- `projections.png`: projection levels after a confirmed setup.
- `position-sizer.png`: entry, stop, and target boxes.

Remove account names, orders, balances, personal annotations, and other identifying details before uploading an image.

## Scope And Attribution

This independent implementation was informed by publicly available trading education, including material published by TTrades. It is not affiliated with, endorsed by, or presented as the official TTrades Fractal Model. The explanation here describes this script's behavior in original language and does not reproduce third-party charts or course material.

The source includes an MPL 2.0 notice. This repository is for education and research, not financial or legal advice.
