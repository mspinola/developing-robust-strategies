# Bonus #1 Moving Average Crossover Strategy

## Step 1: Formulate

### Entry / Exit

Variation 1: Non-reversing

* Enter Long: MA1 crosses above MA2
* Exit Long: MA1 crosses below MA2

Variation 2: Reversing

* Enter Long: MA1 crosses above MA2
* Enter Short: MA1 crosses below MA2

#### Parameters

MA Lookback Speed

| Speed  | MA1 | MA2 |
| ------ | --- | --- |
| FAST   | 5   | 20  |
| MEDIUM | 10  | 40  |
| SLOW   | 20  | 80  |

### Filters

Entry:

* Volatility Filter: identify (high) expansion or (low) contraction in volatility at time of MA crossover
* Volatility defined as `TrueRange`
* low: TrueRange < average(TrueRange, vfLookback) * vfGain
* high: TrueRange > average(TrueRange, vfLookback) * vfGain

| Reversing | VF low | VF high |
| --------- | ------ | ------- |
| No        | No     | No      |
| No        | No     | Yes     |
| No        | Yes    | No      |
| Yes       | No     | No      |
| Yes       | No     | Yes     |
| Yes       | Yes    | No      |

### Risk Management

* No stop, limit, or target

### Variations

| Entry/Exit | Entry Filter | Parameters | Risk | Total |
| ---------- | ------------ | ---------- | ---- | ----- |
| 2          | 3            | 3          | 1    | 18    |

## Step 2: Define

### Variation 1: Low Volatility Filter

```python
# Signals
ma1Length = { 5, 10, 20 }
ma2Length = { 20, 40, 80 }
ma1 = average(close, ma1Length)
ma2 = average(close, ma2Length)

goLong = goShort = False
if ma1[0] > ma2[0] and ma1[1] < ma2[1]
    goLong = True
if ma1[0] < ma2[0] and ma1[1] > ma2[1]
    goShort = True

# Filters
vfGain = 1.25
vfLength = 10
volatilityAverage = average(trueRange(), vfLength)
volatilityThreshold = volatilityAverage * vfGain
lowVolatility = trueRange() < volatilityThreshold
highVolatility = trueRange() > volatilityThreshold

# Variation 1: No Entry Filter
position = currentPosition()
if position == 0 and goLong == 1
    Buy("LE", close)
else if position == 0 and goShort == 1
    Sell("SE", close)

# Variation 2: Low Volatility Entry Filter
if lowVolatility
    if position == 0 and goLong == 1
        Buy("LE", close)
    else if position == 0 and goShort == 1
        Sell("SE", close)

# Variation 3: High Volatility Entry Filter
if highVolatility
    if position == 0 and goLong == 1
        Buy("LE", close)
    else if position == 0 and goShort == 1
        Sell("SE", close)
```

## Step 3: Confirm

See ./mac-no-profit-management.rts

## Step 4: First Hurdle

