# wink-statistics

> Fast and Numerically Stable Statistical Analysis Utilities

### [![Build Status](https://api.travis-ci.org/decisively/wink-statistics.svg?branch=master)](https://travis-ci.org/decisively/wink-statistics) [![Coverage Status](https://coveralls.io/repos/github/decisively/wink-statistics/badge.svg?branch=master)](https://coveralls.io/github/decisively/wink-statistics?branch=master) [![Inline docs](http://inch-ci.org/github/decisively/wink-statistics.svg?branch=master)](http://inch-ci.org/github/decisively/wink-statistics) [![devDependencies Status](https://david-dm.org/decisively/wink-statistics/dev-status.svg)](https://david-dm.org/decisively/wink-statistics?type=dev)

<img align="right" src="https://decisively.github.io/wink-logos/logo-title.png" width="100px" >

Perform fast and numerically stable statistical analysis using **`wink-statistics`**. It is a part of _[wink](https://www.npmjs.com/~sanjaya)_ — a growing family of high quality packages for Statistical Analysis, Natural Language Processing and Machine Learning in NodeJS.

Summarize, Discover and Analyze data with  Descriptive, Robust, Streaming Statistics along with Probability.  A class of its functions operate on a real-time stream of data and incrementally compute required statistic that usually would take more than one pass over the data. These are especially useful in IoT environment.

## Installation

Use [npm](https://www.npmjs.com/package/wink-statistics) to install:

    npm install wink-statistics --save

## Example [![Build Status](https://badge.runkitcdn.com/wink-statistics.svg)](https://npm.runkit.com/wink-statistics)

```javascript
// Load Wink Statistics
var stats = require( 'wink-statistics' );
```

## API

The APIs are available under `probability`,  `stats` and `streaming` name spaces. All `streaming` functions are higher order functions that return an object containing `compute()/build()`, `value()`, `result()`, and `reset()` functions.

```javascript
// Load the entire library of functions
var ws = require( 'wink-statistics' );
//  Use probability functions
ws.probability.aggregate( 0.5, 0.6 );
// Use stats functions
ws.stats.stdev( [ 2, 3, 5, 7 ] );
// Use streaming higher order functions
stdev = ws.streaming.stdev();
```

Alternatively, a single API may be required independently:

```javascript
// Load a single function
// Use a probability function
var aggregate = require( './src/probability-aggregate.js' );
// Use a stats function
var stdev = require( './src/stats-stdev.js' );
// Use a streaming higher order function
var stdev = require( './src/streaming-stdev.js' )();
```

### probability

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### aggregate

Aggregates two probability estimates from independent sources about the occurrence of
a **single** event **a**. It returns the aggregated probability of occurrence
of the event **a**. The assumption here is that the two probabilities
(estimates) are not correlated with each other and the **common prior**
probability of **a** is **0.5**.

For a detailed explanation, refer to the paper titled
_[Bayesian Group Belief by Franz Dietrich](http://link.springer.com/article/10.1007/s00355-010-0453-x)_
published in Social Choice and Welfare October 2010, Volume 35, Issue 4, pp 595–626.

**Parameters**

-   `pa1` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — first estimate of probability of occurrence of event **a**.
-   `pa2` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — second estimate of probability of occurrence of event **a**.

**Examples**

```javascript
aggregate( 0.5, 0.6 );
// returns 0.6
aggregate( 0.5, 0.4 );
// returns 0.4
aggregate( 0.6, 0.6 );
// returns 0.6923076923076923
aggregate( 0.4, 0.6 );
// returns 0.5
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — the aggregated probability.

#### range4CI

Computes probability from the **observed** count of successes (`successCount`) out of the total count (`totalCount`)
along with its **range** for required level of Confidence Interval (CI)  i.e. `zscore` .
The range is the minimum and maximum probability values for given `zscore` or CI.

These computations are based on approach specified in the Wilson's _Notes on
Probable Inference, The Law of Succession, and Statistical Inference_
published in ASA's Journal.

For quick reference, typical value of `zscore` for 90% and 95% CI is approximately
1.645 and 1.960 respectively.

**Parameters**

-   `successCount` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — observed count of successes out of
-   `totalCount` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — the total count.
-   `zscore` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — for the required level of CI. (optional, default `1.645`)

**Examples**

```javascript
range4CI( 1, 10 );
// returns {
//   probability: 0.18518871952479238,
//   min: 0.02263232984000629,
//   max: 0.34774510920957846
// }
range4CI( 10, 100 );
// returns {
//   probability: 0.1105389143431459,
//   min: 0.06071598345043355,
//   max: 0.16036184523585828
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `probability`, `min` and `max`.

### stats

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### boxplot

Performs complete [boxplot](https://en.wikipedia.org/wiki/Box_plot) analysis
including computation of notches and outliers.

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `coeff` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — used for outliers computation. (optional, default `1.5`)
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `sortedData` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
var data = [
  -12, 14, 14, 14, 16, 18, 20, 20, 21, 23, 27, 27, 27, 29, 31,
  31, 32, 32, 34, 36, 40, 40, 40, 40, 40, 42, 51, 56, 60, 88
];
boxplot( data );
returns {
//   min: -12, q1: 20, median: 31, q3: 40, max: 88,
//   iqr: 20, range: 100, size: 30,
//   leftOutliers: { begin: 0, end: 0, count: 1, fence: 14 },
//   rightOutliers: { begin: 29, end: 29, count: 1, fence: 60 },
//   leftNotch: 25.230655727612252,
//   rightNotch: 36.76934427238775
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — consisting of `min`, `q1`, `median`, `q3`,
`max`, `iqr`, `range`, `size` along with `leftNotch`, and `rightNotch`.
The `leftOutliers/rightOutliers` (object), if present, contains the `count`, `fence`
and `begin/end` indexes to `sortedData` for easy extraction of exact values.

#### fiveNumSummary

Returns the [five number summary](https://en.wikipedia.org/wiki/Five-number_summary) from the `sortedData`.

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
fiveNumSummary( [ 1, 1, 2, 2, 3, 3, 4, 4 ] );
// returns {
//   q1: 1.25, median: 2.5, q3: 3.75, iqr: 2.5,
//   size: 8, min: 1, max: 4, range: 3
// }
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — 5-number summary consisting of `min`, `q1`, `median`, `q3`,
`max` along with `iqr`, `range`, and `size`.

#### histogram

Generates histogram using Freedman–Diaconis method.
If both IQR and MAD are `0` then it automatically
switches to Sturges' Rule while ensuring minimum of 5 bins.
It attempts to reduce excessive sparsity of distribution,
if any, by adjusting the number of bins using Sturges' Rule.

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `dataPrecision` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — typically the minumum number of
    decimal places observed in the `sortedData`. (optional, default `0`)
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
var data = [
  12, 14, 14, 14, 16, 18, 20, 20, 21, 23, 27, 27, 27, 29, 31,
  31, 32, 32, 34, 36, 40, 40, 40, 40, 40, 42, 51, 56, 60, 65
];
histogram( data );
// returns {
//   classes: [
//     { min: 12, mid: 19, max: 25 },
//     { min: 25, mid: 32, max: 38 },
//     { min: 38, mid: 45, max: 51 },
//     { min: 51, mid: 58, max: 64 },
//     { min: 64, mid: 71, max: 77 } ],
//   frequencies: [ 10, 10, 7, 2, 1 ],
//   q1: 20,  q3: 40, iqr: 20, size: 30, min: 12, max: 65,range: 53
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — conatining arrays `classes` and the corresponding `frequencies`.
Each element of `classes` array is an object with values for `min/max (class intervals)`
and `mid` point of a class. <br/><br/>In addition, the returned object
contains useful statistics like `q1`, `q3`, `iqr`, `min`, `max`, and `range`.

#### mad

Returns the median of the `sortedData`.

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
mad( [ 1, 1, 2, 2, 3, 3, 4, 4 ] );
// returns 1
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — median of the `sortedData`.

#### max

Finds the maximum value in the `x` array.

**Parameters**

-   `x` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — array containing 1 or more elements.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
max( [ 99, 1, -1, +222, 0, -99 ] )
// returns 222
max( [ { x: 33 }, { x: 11 }, { x:44 } ], 'x' )
// returns 44
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — maximum value from array `x`.

#### mean

Comuptes the mean of numbers contained in the `x` array.
The computations are inspired by the method proposed by [B. P. Welford](http://dx.doi.org/10.1080/00401706.1962.10490022).

**Parameters**

-   `x` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — array containing 1 or more elements.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
mean( [ 2, 3, 5, 7 ] )
// returns 4.25
mean( [ { x: 2 }, { x: 3 }, { x: 5 }, { x: 7 } ], 'x' )
// returns 4.25
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — mean value.

#### median

Returns the median of the `sortedData`.

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — Useful when each element of
    `sortedData` is an object or an array instead of number. If it is an object
    then it should be the key (string) to access the value; or if it is an array
    then it should be the index (number) to access the value; or it should be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
median( [ 1, 1, 2, 2, 3, 3, 4, 4 ] );
// returns 2.5
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — median of the `sortedData`.

#### min

Finds the minimum value in the `x` array.

**Parameters**

-   `x` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — array containing 1 or more elements.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
min( [ 99, 1, -1, +222, 0, -99 ] )
// returns -99
min( [ { x: 33 }, { x: 11 }, { x:44 } ], 'x' )
// returns 11
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — minimum value from array `x`.

#### percentile

Returns the `q`<sup>th</sup> percentile from the `sortedData`. The computation is
based on Method 11 described in [Quartiles in Elementary Statistics](https://ww2.amstat.org/publications/jse/v14n3/langford.html)
by Eric Langford published in Journal of Statistics Education Volume 14, Number 3 (2006).

**Parameters**

-   `sortedData` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — sorted in ascending order of value.
-   `q` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — should be between 0 and 1 indicating percentile;
    for example, to get 25<sup>th</sup> percentile, it should be 0.25.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
percentile( [ 1, 1, 2, 2, 3, 3, 4, 4 ], 0.25 );
// returns 1.25
percentile( [ 1, 1, 2, 2, 3, 3, 4, 4 ], 0.75 );
// returns 3.75
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — `q`<sup>th</sup> percentile of `sortedData`.

#### stdev

Comuptes the sample standard deviation of numbers contained in the `x` array.
The computations are inspired by the method proposed by [B. P. Welford](http://dx.doi.org/10.1080/00401706.1962.10490022).

**Parameters**

-   `x` **[array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)** — array containing 1 or more elements.
-   `accessor` **([string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String) \| [number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) \| [function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function))** — required when elements of
    `x` are objects or arrays instead of numbers.
    For objects, use key (string) to access the value; in case of arrays, use
    index (number) to access the value; or it could be a function
    that extracts the value from the element passed to it. (optional, default `undefined`)

**Examples**

```javascript
stdev( [ 2, 3, 5, 7 ] )
// returns 2.217355782608345
stdev( [ { x: 2 }, { x: 3 }, { x: 5 }, { x: 7 } ], 'x' )
// returns 2.217355782608345
```

Returns **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** — standard deviation of sample.

### streaming

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

#### freqTable

It is a higher order function that returns an object containing `build()`, `value()`, `result()`, and `reset()` functions.

Use `build()` to construct a frequency table from value of data items passed to it in real-time.
Probe the object containing data-item/frequency pairs using `value()`, which may be reset via `reset()`.

The `result()` returns an object containing the frequency `table` sorted in descending order of category counts or frequency, along
with it's `size`, `sum` of all counts, `x2` - chi-squared statistic, `df` - degree of freedom, and the
`entropy`.

The `x2` along with the `df` can be used test the hypothesis that "the distribution is a uniform one". The
`percentage` in `table` give the percentage of a category count against the `sum`; and `expected` is the count
assuming an uniform distribution.

**Examples**

```javascript
var ft = freqTable();
ft.build( 'Tea' );
ft.build( 'Tea' );
ft.build( 'Tea' );
ft.build( 'Pepsi' );
ft.build( 'Pepsi' );
ft.build( 'Gin' );
ft.build( 'Coke' );
ft.build( 'Coke' );
ft.value();
// returns { Tea: 3, Pepsi: 2, Gin: 1, Coke: 2 }
ft.result();
// returns {
//  table: [
//   { category: 'Tea', observed: 3, percentage: 37.5, expected: 2 },
//   { category: 'Pepsi', observed: 2, percentage: 25, expected: 2 },
//   { category: 'Coke', observed: 2, percentage: 25, expected: 2 },
//   { category: 'Gin', observed: 1, percentage: 12.5, expected: 2 }
//  ],
//  size: 4,
//  sum: 8,
//  x2: 1,
//  df: 3,
//  entropy: 1.9056390622295665
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### max

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **maximum** value of data items passed to it in real-time.
Probe the maximum anytime using `value()`, which may be reset via `reset()`.
The `result()` returns an object containing `max`.

**Examples**

```javascript
var maximum = max();
maximum.compute( 3 );
maximum.compute( 6 );
maximum.value();
// returns 6
maximum.result();
// returns { max: 6 }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### mean

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **mean** aka average value of data items passed to it in real-time.
Probe the mean anytime using `value()`, which may be reset via `reset()`.
The computations are inspired by the method proposed by [B. P. Welford](http://dx.doi.org/10.1080/00401706.1962.10490022).

The `result()` returns an object containing sample `mean` along with `size` of data.

**Examples**

```javascript
var avg = mean();
avg.compute( 2 );
avg.compute( 3 );
avg.compute( 5 );
avg.compute( 7 );
avg.value();
// returns 4.25
avg.result();
// returns { n: 4, mean: 4.25 }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### min

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **minimum** value of data items passed to it in real-time.
Probe the minimum anytime using `value()`, which may be reset via `reset()`.
The `result()` returns an object containing `min`.

**Examples**

```javascript
var minimum = min();
minimum.compute( 3 );
minimum.compute( 6 );
minimum.value();
// returns 3
minimum.result();
// returns { min: 3 }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### stdev

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **standard deviation** value of data items passed to it in real-time.
Probe the sample standard deviation anytime using `value()`, which may be reset via `reset()`.
The computations are inspired by the method proposed by [B. P. Welford](http://dx.doi.org/10.1080/00401706.1962.10490022).

The `result()` returns an object containing sample `stdev` and
`variance`, along with `mean`, `size` of data; it also
contains population standard deviation and variance as `stdevp` and `variancep`.

**Examples**

```javascript
var sd = stdev();
sd.compute( 2 );
sd.compute( 3 );
sd.compute( 5 );
sd.compute( 7 );
sd.value();
// returns 2.217355782608345
sd.result();
// returns { size: 4, mean: 4.25,
//   variance: 4.916666666666666,
//   stdev: 2.217355782608345,
//   variancep: 3.6874999999999996,
//   stdevp: 1.920286436967152
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### sum

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **sum** of data items passed to it in real-time.
Probe the sum anytime using `value()`, which may be reset via `reset()`. The sum
is compensated for floating point errors using Neumaier Method.
The `result()` returns an object containing `sum`.

**Examples**

```javascript
var addition = sum();
addition.compute( 1 );
addition.compute( 10e+100 );
addition.compute( 1 );
addition.compute( -10e+100 );
addition.value();
// returns 2
addition.result();
// returns { sum: 2 }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

#### summary

It is a higher order function that returns an object containing `compute()`, `value()`, `result()`, and `reset()` functions.

Use `compute()` to continuously determine the **summary statistics** of data items passed to it in real-time.
Probe the sample summary statistics anytime using `value()`, which may be reset via `reset()`. The
`result()` is also an alias of `value()`.
The computations are inspired by the method proposed by [B. P. Welford](http://dx.doi.org/10.1080/00401706.1962.10490022).

The summary statistics is an object containing `size`, `min`, `mean`, `max`, sample `stdev` along with
sample `variance` of data; it also
contains population standard deviation and variance as `stdevp` and `variancep`.

**Examples**

```javascript
var ss = summary();
ss.compute( 2 );
ss.compute( 3 );
ss.compute( 5 );
ss.compute( 7 );
ss.result();
// returns { size: 4, min: 2, mean: 4.25, max: 7,
//   variance: 4.916666666666666,
//   stdev: 2.217355782608345,
//   variancep: 3.6874999999999996,
//   stdevp: 1.920286436967152
// }
```

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** — containing `compute`, `value`, `result`, and `reset` functions.

## Need Help?

If you spot a bug and the same has not yet been reported, raise a new [issue](https://github.com/decisively/wink-statistics/issues) or consider fixing it and sending a pull request.

## Copyright & License

**wink-statistics** is copyright 2017 GRAYPE Systems Private Limited.

It is licensed under the under the terms of the GNU Affero General Public License as published by the Free
Software Foundation, version 3 of the License.
