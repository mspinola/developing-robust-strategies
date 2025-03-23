# Bonus #1 Moving Average Crossover Strategy

## Step 1: Formulate

### Entry

* Long: MA crosses above another MA
* Short: MA crosses below another MA
* Volatility Filter: identify (high) expansion or (low) contraction in volatility at time of MA crossover

### Exit

* Go flat on opposite MA signal

### Risk

* No stop, limit, or target

## Step 2: Define

### Variation 1: Low Volatility Filter

```c
// Calculate Signals
ma1Length = 5
ma2Length = 20
goLong = goShort = position = 0
ma1 = average(close, ma1Length)
ma2 = average(close, ma2Length)

// Calculate Filters
vfThreshold = 1.25
vfLength = 10
vfLow = trueRange() < (average(trueRange(), vfLength) * vfThreshold)
vfHigh = trueRange() > (average(trueRange(), vfLength) * vfThreshold)

// Variation 1 -- no filters
if ma1[0] > ma2[0] and ma1[1] < ma2[1]
    goLong = 1
if ma1[0] < ma2[0] and ma1[1] > ma2[1]
    goShort = 1

if position == 0 and goLong == 1
    Buy("LE", close)
else if position == 0 and goShort == 1
    Sell("SE", close)

// Variation 2 -- low volatility filter
// Variation 2 -- high volatility filter

```

## Step 3: