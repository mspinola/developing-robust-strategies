# Bonus #2 Filtered High Low Breakout Strategy

## Step 1: Formulate

Highest high / lowest low breakout

### Entry

* Long: Highest High of last N days + Entry Filter value
* Short: Lowest Low of last N days - Entry Filter value
* Entry Filter: % of range over lookback period (N days)
* Volatility Filter: none, low, high

### Exit

* Go flat on opposite signal

### Risk

* No stop, limit, or target

## Step 2: Define

### Entries

```python
// Volatility Filter based on True Range
volatilityGain = 0.25
volatilityLookBack = 5

volatilityAverage = average(trueRange(), volatilityLookBack)
volatilityThreshold = volatilityAverage * volatilityGain

highLowLookBack = 10
highest = highest(high, highLowLookBack)
lowest = lowest(low, highLowLookBack)
volatilityRange = (highest(high, volatilityLookBack) - lowest(low, volatilityLookBack)) / volatilityAverage

marketPosition = 0
tradeOn = false
vfNone = true
vfLow = TrueRange < volatilityThreshold
vfHigh = TrueRange > volatilityThreshold
if volatilityType == NONE or
   volatilityType == LOW and vfLow
   volatilityType == HIGH and vfHigh
        longEntry = highest + volatilityThreshold
        shortEntry = lowest - volatilityThreshold
        tradeOn = true

if tradeOn and marketPosition[0] == 0
    buy("longEntry", NEXT_BAR, longEntry, STOP)

if marketPosition == 1
    sell("longExit", NEXT_BAR, longExit, STOP)

if tradeOn and marketPosition[0] == 0
    sellShort("shortEntry", NEXT_BAR, shortEntry, STOP)

if marketPosition == -1
    buyToCover("shortExit", NEXT_BAR, shortExit, STOP)
```

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