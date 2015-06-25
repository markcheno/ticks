# Ticks
A utility for choosing nice tick marks or histogram intervals.

## API

`ticks(min, max, n)`

 * `min` The minimum value of the interval.
 * `max` The maximum value of the interval.
 * `n` The approximate number of desired ticks.

Returns an array of "ticks", numbers that are suitable to use as tick mark values. The first tick will be less than or equal to `min`, and the last tick will be greater than or equal to `max`.

## Usage

Install via NPM:

`npm install -S ticks`

Example use:

```javascript
var ticks = require("ticks");

ticks(0.5432, 9.543, 10); //[ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 ]
ticks(1, 10000, 5); // [ 0, 2000, 4000, 6000, 8000, 10000 ]
ticks(-1, 1, 10); // [-1,-0.8,-0.6,-0.4,-0.2,0,0.2,0.4,0.6,0.8,1]
ticks(1000, 1002, 10); //[1000,1000.2,1000.4,1000.6,1000.8,1001,1001.2,1001.4,1001.6,1001.8,1002]
```

## Algorithm
The algorithm for computing ticks is based on the idea of a "nice interval". Nice intervals can be expressed as `(base * 10^exp)`, where `exp` is some integer exponent, and `base` is either 1, 2, or 5. Examples of nice intervals are 0.1, 0.5, 10, 20, 5, 2, and 500.

The Ticks algorithm computes the exponent of the raw interval, by `Math.log10((max - min) / n)`, then computes both the floor and ceiling of this value, which are candidate exponents for use in generating nice intervals. The algorithm then tries all 6 possible combinations of the two candidate exponents with the possible bases (1, 2, and 5) to generate a set of candidate nice intervals. From the generated set of nice intervals, the one that is closest to the raw interval (`(max - min) / n`) is chosen.

## Related Work

The seminal approach for generating tick marks appeared in ["Nice Numbers for Graph Labels" by Paul S. Heckbert in the book "Graphics Gems"](https://books.google.com/books?id=Mqn8BAAAQBAJ&pg=PA61&lpg=PA61&dq=heckbert+nice+numbers+axis+graphics+gems&source=bl&ots=FtY2gnkRov&sig=o4053FRSlSxhaPbF0BPIwU3VZlI&hl=en&sa=X&ved=0CDkQ6AEwCWoVChMI4O2nn4mTxgIVFhiSCh3S2ACz#v=onepage&q=heckbert%20nice%20numbers%20axis%20graphics%20gems&f=false), originally [published in 1990](http://dl.acm.org/citation.cfm?id=90783).

Subsequently, there have been a variety of new takes on the problem. This research paper presents an approach for this that takes more factors into account, including the actual size of label text: [An Extension of Wilkinson’s Algorithm for Positioning Tick Labels on Axes, by Justin Talbot, Sharon Lin, and Pat Hanrahan](http://graphics.stanford.edu/vis/publications/2010/labeling-preprint.pdf), and also surveys other algorithms.

Also, a similar algorithm is implemented in [D3.scale.linear.ticks](https://github.com/mbostock/d3/wiki/Quantitative-Scales#linear_ticks).

## Future Goals

The Graphics Gems chapter "Nice Numbers for Graph Labels" by Paul S. Heckbert details "loose" and "tight" tick marks. Currently this library only provides "loose" ticks, but could be easily modified to generate "tight" ticks. It could also be extended to account for actual text label size.

## Development Tooling

This module is published as an NPM package. The module itself is authored using ES6 module syntax in `index.js`, which is declared as the `jsnext:main` entry point in `package.json` so it can be included in Rollup-based builds. The module is converted to CommonJS format in the `prepublish` script, which runs the `pretest` and `test` scripts`, making the built file `ticks.js` available in the NPM package. The built file is excluded from the Git repository via the `.gitignore` file. Usually NPM ignores files in `.gitignore`, so to cause NPM to include `ticks.js`, an [empty `.npmignore` file was added.

Here's what it looks like when `npm publish` is run:

![](http://curran.github.io/images/ticks/publishFlow.png)

<small>Curran Kelleher June 2015</small>
