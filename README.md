[![Build Status](https://travis-ci.org/kaelzhang/moving-averages.svg?branch=master)](https://travis-ci.org/kaelzhang/moving-averages)
[![Coverage](https://codecov.io/gh/kaelzhang/moving-averages/branch/master/graph/badge.svg)](https://codecov.io/gh/kaelzhang/moving-averages)
<!-- optional npm version
[![NPM version](https://badge.fury.io/js/moving-averages.svg)](http://badge.fury.io/js/moving-averages)
-->
<!-- optional npm downloads
[![npm module downloads per month](http://img.shields.io/npm/dm/moving-averages.svg)](https://www.npmjs.org/package/moving-averages)
-->
<!-- optional dependency status
[![Dependency Status](https://david-dm.org/kaelzhang/moving-averages.svg)](https://david-dm.org/kaelzhang/moving-averages)
-->

# moving-averages

The complete collection of [FinTech](https://en.wikipedia.org/wiki/Financial_technology) utility methods for [Moving average](https://en.wikipedia.org/wiki/Moving_average), including:

- [simple moving average (MA)](#simple-moving-average-madata-size)
- [dynamic weighted moving average (DMA)](#dynamic-weighted-moving-average-dmadata-alpha-nohead)
- [exponential moving average (EMA)](#exponential-moving-average-emadata-size)
- [smoothed moving average (SMA)](#smoothed-moving-average-smadata-size-times)
- [weighted moving average (WMA)](#weighted-moving-average-wmadata-size)

And `moving-averages` will also handle empty values.

## install

```sh
$ npm i moving-averages
```

## usage

```js
import {
  ma, dma, ema, sma, wma
} from 'moving-averages'

ma([1, 2, 3, 4, 5], 2)    
// [<1 empty item>, 1.5, 2.5, 3.5, 4.5]
```

## Simple Moving Average: `ma(data, size)`

- **data** `Array.<Number|undefined>` the collection of data inside which empty values are allowed. Empty values are useful if a stock is suspended.
- **size** `Number` the size of the periods.

Returns `Array.<Number|undefined>`

#### Special Cases

```js
// If the size is less than `1`
ma([1, 2, 3], 0.5)       // [1, 2, 3]

// If the size is larger than data length
ma([1, 2, 3], 5)         // [<3 empty items>]

ma([, 1,, 3, 4, 5], 2)   
// [<2 empty items>, 0.5, 1.5, 3.5, 4.5]
```

And all of the other moving average methods have similar mechanism.

## Dynamic Weighted Moving Average: `dma(data, alpha, noHead)`

- **data**
- **alpha** `Number|Array.<Number>` the coefficient or list of coefficients `alpha` represents the degree of weighting decrease for each datum.
  - If `alpha` is a number, then the weighting decrease for each datum is the same.
  - If `alpha` larger than `1` is invalid, then the return value will be an empty array of the same length of the original data.
  - If `alpha` is an array, then it could provide different decreasing degree for each datum.
- **noHead** `Boolean=` whether we should abandon the first DMA.

Returns `Array.<Number|undefined>`

```js
dma([1, 2, 3], 2)    // [<3 empty items>]

dma([1, 2, 3], 0.5)  // [1, 1.5, 2.25]

dma([1, 2, 3, 4, 5], [0.1, 0.2, 0.1])  
// [1, 1.2, 1.38]
```

## Exponential Moving Average: `ema(data, size)`

Calulates the most frequent used exponential average which covers about 86% of the total weight (when `alpha = 2 / (N + 1)`).

- **data**
- **size** `Number` the size of the periods.

Returns `Array.<Number|undefined>`

## Smoothed Moving Average: `sma(data, size, times)`

Also known as the modified moving average or running moving average, with `alpha = times / size`.

- **data**
- **size**
- **times** `Number=1`

Returns `Array.<Number|undefined>`

## Weighted Moving Average: `wma(data, size)`

Calculates convolution of the datum points with a fixed weighting function.

Returns `Array.<Number|undefined>`

## Related FinTech Modules

- [bollinger-bands](https://www.npmjs.com/package/bollinger-bands): Fintach math utility to calculate bollinger bands.
- [s-deviation](https://www.npmjs.com/package/s-deviation): Math utility to calculate standard deviations.
- [moving-averages](https://www.npmjs.com/package/moving-averages): The complete collection of utility methods for [Moving average](https://en.wikipedia.org/wiki/Moving_average).

MIT



多种移动平均计算总结

股票期货里面经常会遇到这些公式，通达信，同花顺，文华，基本都有。作为一个程序员觉得网上比较的思路不清晰，在此做个总结，一目了然。

一.函数简介

MA(x,n)-移动平均，是最简单的n日内的平均值

SMA(x,n,m)-简单移动平均，m为当日的权重，是个0~1之间的值

EMA(x,n)-指数移动平均，这个函数以相关周期为权重进行计算

DMA(x,m)-动态移动平均，这个函数以动态设定的权重m进行计算

TMA(x,p,q)-递归移动平均，这个函数可以完全控制当前周期的权重和上一次值的权重

WMA(x,m)-加权移动平均，这个函数对于近日的权重会比其它函数敏感

二.通用公式


(https://images2015.cnblogs.com/blog/550026/201706/550026-20170623105625585-226385750.png)

-------------------------------------------------------------

补充：

这个通用公式的结构大概可以理解为：

当前函数值= 当前权重 X 当前价格 + 当前权重的互补值 X 上一次函数值;
