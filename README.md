
# Adaptive RSI Overlay – TradingView Indicator

```
//▄▄▄▄▄▄▄   ▄▄▄▄▄   ▄▄▄▄▄▄    ▄▄▄▄▄▄▄                ▄▄▄▄▄▄▄    ▄▄▄▄▄▄▄   ▄▄▄▄   ▄▄▄▄▄▄▄   ▄▄▄    
//███▀▀▀▀▀ ▄███████▄ ███▀▀██▄ ███▀▀▀▀▀                ███▀▀███▄ ███▀▀▀▀▀ ▄██▀▀██▄ ███▀▀███▄ ███    
//███      ███   ███ ███  ███ ███▄▄                   ███▄▄███▀ ███▄▄    ███  ███ ███▄▄███▀ ███    
//███      ███▄▄▄███ ███  ███ ███                     ███▀▀▀▀   ███      ███▀▀███ ███▀▀██▄  ███    
//▀███████  ▀█████▀  ██████▀  ▀███████                ███       ▀███████ ███  ███ ███  ▀███ ████████
//                                    ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄                                             
```

**A direct conversion of the Thinkorswim “Adaptive RSI” study to Pine Script™ v6.**  
This overlay indicator dynamically adapts RSI‑style bands to price, using a Wilder‑style smoothing (RMA) and normalizing the width based on the average absolute price change.

---

## 📊 Indicator Overview

The **Adaptive RSI Overlay** plots a **centre line** (an exponential moving average of price) and **upper/lower bands** that represent specific RSI levels (e.g., 70 and 30). The distance between the centre line and the bands is **adaptive** – it expands and contracts with volatility, similar to how Bollinger Bands work, but using RMA smoothing instead of standard deviation.

Unlike a traditional RSI displayed in a separate pane, this overlay places the RSI‑equivalent levels directly on the price chart, making it easier to spot overbought/oversold conditions in context.

---

## ⚙️ Inputs & Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| **Color Bars** | `false` | If enabled, bars are coloured green when price is above the centre EMA, red when below, and grey otherwise. |
| **Length** | `14` | The smoothing period for the Wilder‑style RMA (same as classic RSI length). |
| **Upper Level** | `70` | The RSI level that defines the upper band. |
| **Lower Level** | `30` | The RSI level that defines the lower band. |
| **Show EMA** | `true` | Toggles the visibility of the centre line (the smoothed price). |
| **Show Fill** | `false` | Toggles the filled zones between intermediate levels (UZ1–UZ2 and DZ1–DZ2). |

---

## 📈 How It Works

1. **Centre Line (EMA)** – A recursive Wilder‑style moving average (RMA) of the closing price:

   ```
   myEMA = alpha * close + (1 - alpha) * myEMA[1]
   where alpha = 1 / length
   ```

2. **Volatility Measure** – The average absolute price change (also Wilder‑smoothed) is used to scale the bands:

   ```
   ccVol = alpha * abs(close - close[1]) + (1 - alpha) * ccVol[1]
   ```

3. **Band Width** – The distance from the centre to each band is proportional to `ccVol` and the chosen RSI level:

   ```
   maxWidth = ccVol * (length - 1)
   upperBand = myEMA + (RSI_level - 50) / 50 * maxWidth
   lowerBand = myEMA - (50 - RSI_level) / 50 * maxWidth
   ```

4. **Intermediate Zones** – Additional levels are plotted at RSI values of 59.9, 63.5 (upper) and 40.9, 36.5 (lower), providing early warning zones. Extra zones at 72.33, 76.58 and 27.7, 23.4 are also included.

5. **Cross Signals** – Built‑in crossover/crossunder logic (commented out) can be enabled to show buy/sell arrows when price crosses the centre line.

---

## 🎨 Visual Elements

- **Yellow line** – Centre EMA.
- **Red line** – Upper band (default RSI 70).
- **Aqua line** – Lower band (default RSI 30).
- **Grey dashed/filled zones** – Intermediate levels (UZ1/UZ2, DZ1/DZ2) that can be filled for visual context.
- **Optional bar coloring** – Green/red bars based on price relative to the centre line.

---

## 🚀 Usage & Trading Signals

- **Overbought / Oversold** – When price trades near or beyond the upper/lower bands, the market may be overextended.
- **Trend Direction** – When price stays consistently above the centre EMA, the trend is bullish; below is bearish.
- **Crossovers** – A price cross above the centre EMA can signal a bullish shift, while a cross below may indicate bearish momentum.
- **Volatility Context** – Wider bands indicate higher volatility; narrower bands suggest consolidation.

---

## 🧰 Compatibility

- **Pine Script™ version 6** – Works on TradingView with overlay mode enabled.
- **Timeframes** – Suitable for any timeframe (adjust the `Length` parameter to suit your trading style).

---

## 📝 Notes

This study is a **faithful conversion** of the Thinkorswim “Adaptive RSI” study. The original Thinkorswim code used:

```
AddCloud(uz2, uz1, Color.LIGHT_GRAY, Color.LIGHT_GRAY)
AddCloud(dz1, dz2, Color.LIGHT_GRAY, Color.LIGHT_GRAY)
```

We have replicated this behaviour with Pine’s `fill()` function. The extra zones (uz3/uz4, dz3/dz4) are plotted but not filled – they are included for completeness.





**Happy trading!** 📉📈
```
