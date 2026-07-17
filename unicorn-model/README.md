# Unicorn Model

`Unicorn Model` is a TradingView tool that identifies high-probability reversal setups by combining a higher-timeframe liquidity sweep with a breaker block and an overlapping Fair Value Gap (FVG). When all three elements align, the script labels the setup a Unicorn. When only the sweep and breaker are present, it labels it a Breaker.

It is a visual-analysis tool. It does not place orders or guarantee a trade outcome.

## The Model In One View

```text
HTF liquidity level → sweep → breaker block (opposing candle run) → FVG overlap → confirmation close
```

The swept level is the reference point. A sweep alone is not enough: the script looks for a run of opposing candles after the sweep extreme that forms the breaker block. When Unicorn Mode is on, the breaker must also contain an unmitigated Fair Value Gap. A close beyond the breaker top (bullish) or bottom (bearish) confirms the setup.

## Bullish Sequence

1. Price trades below a higher-timeframe low, sweeping liquidity.
2. A run of consecutive bullish candles forms the breaker block above the sweep extreme.
3. A bullish FVG overlaps the breaker (required in Unicorn Mode).
4. A close above the breaker top confirms the setup (`+Unicorn` or `+Breaker`).
5. The breaker box, FVG highlight, sweep line, and optional targets are drawn.

## Bearish Sequence

1. Price trades above a higher-timeframe high, sweeping liquidity.
2. A run of consecutive bearish candles forms the breaker block below the sweep extreme.
3. A bearish FVG overlaps the breaker (required in Unicorn Mode).
4. A close below the breaker bottom confirms the setup (`-Unicorn` or `-Breaker`).
5. The breaker box, FVG highlight, sweep line, and optional targets are drawn.

## What The Script Draws

### Sweep Line

A horizontal dotted line from the swept level's formation bar to the sweep bar. A label at the origin identifies the timeframe and level type (e.g. `1H OHLC`).

### Breaker Block

A box spanning the run of opposing candles. The box extends bar by bar until price invalidates or the history limit removes it. Active setups use the configured bull/bear color; invalidated setups turn grey unless the discard option is on.

### Fair Value Gap Highlight

When a valid FVG overlaps the breaker, a second box highlights the gap range inside the breaker. This is the "unicorn" overlap.

### Target Projections

After confirmation, optional horizontal lines project risk-reward targets forward from the breaker edge. The primary R:R target and any additional multiples are configurable.

### Dashboard

An on-chart table shows the current bias, Unicorn Mode state, active liquidity sources, and the latest confirmed setup's entry, invalidation, and target prices. An optional position-size row calculates contract or lot size from account balance and risk settings.

## Settings To Start With

| Setting | Suggested starting point | What it changes |
| --- | --- | --- |
| `Mode` | `Automatic` | Maps higher timeframes to chart resolution automatically. |
| `Bias` | `Neutral` | Shows both bullish and bearish setups. |
| `Unicorn Mode` | On | Requires a breaker + FVG overlap for confirmation. |
| `Use Swing as Invalidation` | Off | Uses the breaker extreme as the stop reference. |
| `Show R:R Target` | On | Draws the primary risk-reward target line. |
| `Show Position Size` | Off at first | Adds a calculated size row to the dashboard. |

## Liquidity Sources

Up to four higher-timeframe sources (A–D) can be active simultaneously. Each source can pull OHLC highs/lows, swing highs/lows, or both. In Automatic mode the script selects the nearest four timeframes above the chart resolution. In Manual mode you set each timeframe explicitly.

## Using It In TradingView

1. Open the [Pine Script source](unicorn-model.pine) and copy it into a new TradingView Pine editor script.
2. Add the script to a chart.
3. Start with Automatic mode, Neutral bias, and Unicorn Mode on.
4. Observe a completed setup — sweep line, breaker box, FVG highlight, and label — before enabling position sizing or additional targets.
5. Test across symbols and timeframes before relying on it in a trading workflow.

## Chart Examples

Add original, anonymized screenshots to `images/` and embed them here as the library grows. Useful first examples:

- `bullish-unicorn.png`: sweep below an HTF low, bullish breaker + FVG, confirmation close.
- `bearish-unicorn.png`: sweep above an HTF high, bearish breaker + FVG, confirmation close.
- `breaker-only.png`: confirmed breaker without an FVG (Unicorn Mode off).
- `targets.png`: primary and additional R:R targets after confirmation.

Remove account names, orders, balances, personal annotations, and other identifying details before uploading an image.

## Scope And Attribution

This independent implementation was informed by publicly available trading education. It is not affiliated with, endorsed by, or presented as any official Unicorn Model product. The explanation here describes this script's behavior in original language and does not reproduce third-party charts or course material.

The source includes an MPL 2.0 notice. This repository is for education and research, not financial or legal advice.
