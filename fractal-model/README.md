# Fractal Model

`Fractal Model` is a TradingView Pine Script indicator that projects higher-timeframe candle structure onto the active chart and tracks sweep-based setups. It combines HTF candle visualization, liquidity sweep detection, C2 confirmation, CISD levels, optional standard-deviation projections, formation liquidity lines, alerts, and a position-sizer overlay.

## High-Level Purpose

The indicator helps a trader observe when the current lower-timeframe price action interacts with a previous higher-timeframe candle's high or low. When price wicks beyond a prior HTF extreme and closes back inside that level, the script treats that as a potential sweep. From there, it tracks whether the setup confirms, finds a CISD level, draws the relevant levels, and optionally shows risk/reward boxes.

## Main Concepts

### HTF Candle Projection

The script builds an internal array of higher-timeframe candles. Each candle stores open, high, low, close, bar indexes, open time, close time, and display positions. On the last chart bar, those candles are drawn to the right side of the chart as a compact HTF projection.

Supported fractal modes include:

- Automatic timeframe mapping, such as `1m -> 15m`, `5m -> 1H`, `1H -> 1D`.
- Custom lower/higher timeframe selection.
- Quarterly/session modes based on 22.5-minute, 90-minute, and 6-hour session cycles.

### Sweep Detection

A bearish sweep starts when price trades above a previous HTF high. It confirms when price closes back below that swept high.

A bullish sweep starts when price trades below a previous HTF low. It confirms when price closes back above that swept low.

The script tracks both live setups and completed historical setups. It can also filter setups by bias: neutral, bullish only, or bearish only.

### C2 And CISD Logic

The script labels the sweep candle as `C2` after the setup confirms. It then searches backward from the setup extreme to find a CISD level:

- Bearish setup: CISD is triggered when price closes below the tracked CISD level.
- Bullish setup: CISD is triggered when price closes above the tracked CISD level.

If price invalidates the setup after CISD, the label can be shown as `xC2`.

### Projection Levels

When projections are enabled, the script draws standard-deviation levels from the setup range. The default levels are:

```text
0,1,-1,-2,-2.5
```

The projection can be based on wick extremes or candle bodies.

### Formation Liquidity

The script can draw liquidity levels from the C1 and optional C0 candles. It tracks whether those levels are later mitigated by price crossing them.

### Position Sizer

When enabled, the position sizer uses:

- Entry: CISD level.
- Stop: C2 extreme.
- Target: calculated from the configured risk/reward ratio.

The script draws risk and reward boxes so the setup can be assessed visually.

## Important Inputs

| Setting | Purpose |
| --- | --- |
| `Fractal` | Chooses automatic, quarterly, custom, or fixed timeframe mapping. |
| `Bias` | Filters bullish, bearish, or both setup directions. |
| `Show Sweeps` | Draws sweep lines and controls live setup visibility. |
| `Show CISD` | Draws CISD confirmation levels. |
| `Enable Projections` | Draws deviation projection levels after CISD. |
| `Enable Formation Liquidity` | Draws C1/C0 liquidity levels. |
| `Enable Position Sizer` | Draws risk/reward boxes from entry, stop, and target. |
| `Calculate on Close` | Limits live setup calculation to confirmed lower-timeframe candles for better performance. |

## Suggested TradingView Use

1. Open TradingView.
2. Create a new Pine Script indicator.
3. Paste the contents of `fractal-model.pine`.
4. Add it to a chart.
5. Start with `Fractal = Automatic`, `Bias = Neutral`, and projections disabled.
6. Enable projections and the position sizer only after confirming that sweep and CISD markings match your intended model.

## Chart Images

Put TradingView screenshots in images/ and include them near the relevant explanation in this file. Good first examples are:

- automatic-fractal.png: the higher-timeframe candle projection on a chart.
- bullish-sweep-cisd.png: a bullish sweep through confirmation.
- bearish-sweep-cisd.png: a bearish sweep through confirmation.
- position-sizer.png: entry, stop, and target boxes.

For each image, remove account names, open orders, account balances, and any other personal information before uploading.

## Attribution Notes

This implementation is inspired by public trading education concepts, including videos by TTrades. This project is not affiliated with or endorsed by TTrades. Personal author details are intentionally omitted from this public project.

Ideas, concepts, trading methods, and educational frameworks are different from copied source code, though this is not legal advice. To reduce attribution and licensing risk, keep third-party notices in copied or modified code, avoid copying video text or proprietary descriptions verbatim, and document what changed in future commits.

## Limitations

- Pine Script behavior should be verified directly in TradingView after each code change.
- Placeholder inputs such as `Previous EQ` and `Drawings Type` are present but marked as not yet implemented in the code.
- This indicator is a visual analysis tool, not a complete trading system.
- No indicator guarantees profitable trades.


