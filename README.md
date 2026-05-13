# Synthetic Index Alert — MQL4 Script

A MetaTrader 4 script that combines multiple market indices into a **composite synthetic index** and fires alerts whenever a significant upward or downward shift is detected.

---

## Overview

Instead of monitoring a single instrument, this script averages the closing prices of three configurable indices (default: US500, US30, NASDAQ) into one composite value. When the composite shifts by more than a defined threshold, it triggers a sound alert, email, or push notification — giving you a broader, multi-market view of momentum changes.

---

## Features

- **Multi-index composite** — averages three instruments into a single synthetic value
- **Threshold-based alerting** — only fires when the shift is significant, reducing noise
- **Three notification channels:** sound alert, email, and mobile push
- **Configurable symbols and timeframe** — works with any MT4 instruments
- **Lightweight loop** — polls once per minute (`Sleep(60000)`)
- Prints all signals and values to the MT4 **Experts log**

---

## How It Works

1. Every minute, the script fetches the most recent **closing price** for each of the three configured indices using `iClose()`
2. The three prices are averaged into a single **synthetic index value**
3. The current value is compared to the previous value:
   - If the difference is **≥ AlertThreshold** and rising → **Upward Shift Detected**
   - If the difference is **≥ AlertThreshold** and falling → **Downward Shift Detected**
4. Alerts are fired via the enabled notification channels

---

## Input Parameters

| Parameter          | Type            | Default      | Description                                       |
|--------------------|-----------------|--------------|---------------------------------------------------|
| `Index1`           | string          | `US500`      | First index symbol                                |
| `Index2`           | string          | `US30`       | Second index symbol                               |
| `Index3`           | string          | `NASDAQ`     | Third index symbol                                |
| `Timeframe`        | ENUM_TIMEFRAMES | `PERIOD_H1`  | Chart timeframe for analysis                      |
| `LookbackPeriod`   | int             | `14`         | Lookback period for moving average calculation    |
| `AlertThreshold`   | double          | `2.0`        | Minimum shift in composite value to trigger alert |
| `EnableAlerts`     | bool            | `true`       | Fire an on-screen/sound alert                     |
| `EnableEmail`      | bool            | `false`      | Send an email notification                        |
| `EnablePush`       | bool            | `false`      | Send a mobile push notification                   |

---

## Installation

1. Copy `Algo_Signal_Alerts_001.mq4` into your MetaTrader 4 **Scripts** folder:
   ```
   MQL4/Scripts/
   ```
2. Open MetaEditor and **compile** the file (F7), or let MT4 compile it automatically on restart.
3. In MT4, open the **Navigator** panel → **Scripts** → drag the script onto any chart.
4. Configure the input parameters in the dialog that appears and click **OK**.

> **Note:** The symbols for `Index1`, `Index2`, and `Index3` must match exactly how they appear in your broker's Market Watch panel (e.g. `US500`, `US30`, `NASDAQ`).

> **Note:** For email and push notifications, configure settings under **Tools → Options → Email / Notifications** in MetaTrader 4.

---

## Usage Notes

- This is a **script** (not an indicator or EA), so it runs once per chart attach and loops internally. It stops when removed from the chart or when `IsStopped()` returns true.
- The composite value is a simple **equal-weight average** of the three indices. Symbols with very different price scales (e.g. mixing a $40,000 index with a $100 index) will be dominated by the larger value — normalize your selection accordingly.
- Tune `AlertThreshold` to match the typical point range of your chosen symbols to avoid over- or under-alerting.
- All alert events are printed to the **Experts** tab in MT4's terminal for review.

---

## Alert Message Format

```
Upward Shift Detected in Synthetic Index (Timeframe: PERIOD_H1)
Current Value: 15423.67
Previous Value: 15419.20
```

---

## Requirements

- MetaTrader 4 (any build supporting `#property strict`)
- MQL4 compiler (included with MT4 / MetaEditor)
- All three index symbols available in your broker's Market Watch

---

## License

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
