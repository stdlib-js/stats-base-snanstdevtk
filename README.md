<!--

@license Apache-2.0

Copyright (c) 2020 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# snanstdevtk

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Calculate the [standard deviation][standard-deviation] of a single-precision floating-point strided array ignoring `NaN` values and using a one-pass textbook algorithm.

<section class="intro">

The population [standard deviation][standard-deviation] of a finite size population of size `N` is given by

<!-- <equation class="equation" label="eq:population_standard_deviation" align="center" raw="\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}" alt="Equation for the population standard deviation."> -->

```math
\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}
```

<!-- <div class="equation" align="center" data-raw-text="\sigma = \sqrt{\frac{1}{N} \sum_{i=0}^{N-1} (x_i - \mu)^2}" data-equation="eq:population_standard_deviation">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@30aa8f4333f62d8bfaa4cce77df48e63c349da13/lib/node_modules/@stdlib/stats/base/snanstdevtk/docs/img/equation_population_standard_deviation.svg" alt="Equation for the population standard deviation.">
    <br>
</div> -->

<!-- </equation> -->

where the population mean is given by

<!-- <equation class="equation" label="eq:population_mean" align="center" raw="\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i" alt="Equation for the population mean."> -->

```math
\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i
```

<!-- <div class="equation" align="center" data-raw-text="\mu = \frac{1}{N} \sum_{i=0}^{N-1} x_i" data-equation="eq:population_mean">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@30aa8f4333f62d8bfaa4cce77df48e63c349da13/lib/node_modules/@stdlib/stats/base/snanstdevtk/docs/img/equation_population_mean.svg" alt="Equation for the population mean.">
    <br>
</div> -->

<!-- </equation> -->

Often in the analysis of data, the true population [standard deviation][standard-deviation] is not known _a priori_ and must be estimated from a sample drawn from the population distribution. If one attempts to use the formula for the population [standard deviation][standard-deviation], the result is biased and yields an **uncorrected sample standard deviation**. To compute a **corrected sample standard deviation** for a sample of size `n`,

<!-- <equation class="equation" label="eq:corrected_sample_standard_deviation" align="center" raw="s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}" alt="Equation for computing a corrected sample standard deviation."> -->

```math
s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}
```

<!-- <div class="equation" align="center" data-raw-text="s = \sqrt{\frac{1}{n-1} \sum_{i=0}^{n-1} (x_i - \bar{x})^2}" data-equation="eq:corrected_sample_standard_deviation">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@30aa8f4333f62d8bfaa4cce77df48e63c349da13/lib/node_modules/@stdlib/stats/base/snanstdevtk/docs/img/equation_corrected_sample_standard_deviation.svg" alt="Equation for computing a corrected sample standard deviation.">
    <br>
</div> -->

<!-- </equation> -->

where the sample mean is given by

<!-- <equation class="equation" label="eq:sample_mean" align="center" raw="\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i" alt="Equation for the sample mean."> -->

```math
\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i
```

<!-- <div class="equation" align="center" data-raw-text="\bar{x} = \frac{1}{n} \sum_{i=0}^{n-1} x_i" data-equation="eq:sample_mean">
    <img src="https://cdn.jsdelivr.net/gh/stdlib-js/stdlib@30aa8f4333f62d8bfaa4cce77df48e63c349da13/lib/node_modules/@stdlib/stats/base/snanstdevtk/docs/img/equation_sample_mean.svg" alt="Equation for the sample mean.">
    <br>
</div> -->

<!-- </equation> -->

The use of the term `n-1` is commonly referred to as Bessel's correction. Note, however, that applying Bessel's correction can increase the mean squared error between the sample standard deviation and population standard deviation. Depending on the characteristics of the population distribution, other correction factors (e.g., `n-1.5`, `n+1`, etc) can yield better estimators.

</section>

<!-- /.intro -->



<section class="usage">

## Usage

```javascript
import snanstdevtk from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-base-snanstdevtk@deno/mod.js';
```
The previous example will load the latest bundled code from the deno branch. Alternatively, you may load a specific version by loading the file from one of the [tagged bundles](https://github.com/stdlib-js/stats-base-snanstdevtk/tags). For example,

```javascript
import snanstdevtk from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-base-snanstdevtk@v0.1.0-deno/mod.js';
```

#### snanstdevtk( N, correction, x, stride )

Computes the [standard deviation][standard-deviation] of a single-precision floating-point strided array `x` ignoring `NaN` values and using a one-pass textbook algorithm.

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';

var x = new Float32Array( [ 1.0, -2.0, NaN, 2.0 ] );

var v = snanstdevtk( x.length, 1, x, 1 );
// returns ~2.0817
```

The function has the following parameters:

-   **N**: number of indexed elements.
-   **correction**: degrees of freedom adjustment. Setting this parameter to a value other than `0` has the effect of adjusting the divisor during the calculation of the [standard deviation][standard-deviation] according to `N-c` where `c` corresponds to the provided degrees of freedom adjustment. When computing the [standard deviation][standard-deviation] of a population, setting this parameter to `0` is the standard choice (i.e., the provided array contains data constituting an entire population). When computing the corrected sample [standard deviation][standard-deviation], setting this parameter to `1` is the standard choice (i.e., the provided array contains data sampled from a larger population; this is commonly referred to as Bessel's correction).
-   **x**: input [`Float32Array`][@stdlib/array/float32].
-   **stride**: index increment for `x`.

The `N` and `stride` parameters determine which elements in `x` are accessed at runtime. For example, to compute the [standard deviation][standard-deviation] of every other element in `x`,

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';
import floor from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-base-special-floor@deno/mod.js';

var x = new Float32Array( [ 1.0, 2.0, 2.0, -7.0, -2.0, 3.0, 4.0, 2.0, NaN ] );
var N = floor( x.length / 2 );

var v = snanstdevtk( N, 1, x, 2 );
// returns 2.5
```

Note that indexing is relative to the first index. To introduce an offset, use [`typed array`][mdn-typed-array] views.

<!-- eslint-disable stdlib/capitalized-comments -->

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';
import floor from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-base-special-floor@deno/mod.js';

var x0 = new Float32Array( [ 2.0, 1.0, 2.0, -2.0, -2.0, 2.0, 3.0, 4.0, NaN ] );
var x1 = new Float32Array( x0.buffer, x0.BYTES_PER_ELEMENT*1 ); // start at 2nd element

var N = floor( x0.length / 2 );

var v = snanstdevtk( N, 1, x1, 2 );
// returns 2.5
```

#### snanstdevtk.ndarray( N, correction, x, stride, offset )

Computes the [standard deviation][standard-deviation] of a single-precision floating-point strided array ignoring `NaN` values and using a one-pass textbook algorithm and alternative indexing semantics.

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';

var x = new Float32Array( [ 1.0, -2.0, NaN, 2.0 ] );

var v = snanstdevtk.ndarray( x.length, 1, x, 1, 0 );
// returns ~2.0817
```

The function has the following additional parameters:

-   **offset**: starting index for `x`.

While [`typed array`][mdn-typed-array] views mandate a view offset based on the underlying `buffer`, the `offset` parameter supports indexing semantics based on a starting index. For example, to calculate the [standard deviation][standard-deviation] for every other value in `x` starting from the second value

```javascript
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';
import floor from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-base-special-floor@deno/mod.js';

var x = new Float32Array( [ 2.0, 1.0, 2.0, -2.0, -2.0, 2.0, 3.0, 4.0 ] );
var N = floor( x.length / 2 );

var v = snanstdevtk.ndarray( N, 1, x, 2, 1 );
// returns 2.5
```

</section>

<!-- /.usage -->

<section class="notes">

## Notes

-   If `N <= 0`, both functions return `NaN`.
-   If `n - c` is less than or equal to `0` (where `c` corresponds to the provided degrees of freedom adjustment and `n` corresponds to the number of non-`NaN` indexed elements), both functions return `NaN`.
-   Some caution should be exercised when using the one-pass textbook algorithm. Literature overwhelmingly discourages the algorithm's use for two reasons: 1) the lack of safeguards against underflow and overflow and 2) the risk of catastrophic cancellation when subtracting the two sums if the sums are large and the variance small. These concerns have merit; however, the one-pass textbook algorithm should not be dismissed outright. For data distributions with a moderately large standard deviation to mean ratio (i.e., **coefficient of variation**), the one-pass textbook algorithm may be acceptable, especially when performance is paramount and some precision loss is acceptable (including a risk of computing a negative variance due to floating-point rounding errors!). In short, no single "best" algorithm for computing the standard deviation exists. The "best" algorithm depends on the underlying data distribution, your performance requirements, and your minimum precision requirements. When evaluating which algorithm to use, consider the relative pros and cons, and choose the algorithm which best serves your needs.

</section>

<!-- /.notes -->

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```javascript
import randu from 'https://cdn.jsdelivr.net/gh/stdlib-js/random-base-randu@deno/mod.js';
import round from 'https://cdn.jsdelivr.net/gh/stdlib-js/math-base-special-round@deno/mod.js';
import Float32Array from 'https://cdn.jsdelivr.net/gh/stdlib-js/array-float32@deno/mod.js';
import snanstdevtk from 'https://cdn.jsdelivr.net/gh/stdlib-js/stats-base-snanstdevtk@deno/mod.js';

var x;
var i;

x = new Float32Array( 10 );
for ( i = 0; i < x.length; i++ ) {
    x[ i ] = round( (randu()*100.0) - 50.0 );
}
console.log( x );

var v = snanstdevtk( x.length, 1, x, 1 );
console.log( v );
```

</section>

<!-- /.examples -->

* * *

<section class="references">

## References

-   Ling, Robert F. 1974. "Comparison of Several Algorithms for Computing Sample Means and Variances." _Journal of the American Statistical Association_ 69 (348). American Statistical Association, Taylor & Francis, Ltd.: 859–66. doi:[10.2307/2286154][@ling:1974a].

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/stats-base/dnanstdevtk`][@stdlib/stats/base/dnanstdevtk]</span><span class="delimiter">: </span><span class="description">calculate the standard deviation of a double-precision floating-point strided array ignoring NaN values and using a one-pass textbook algorithm.</span>
-   <span class="package-name">[`@stdlib/stats-base/nanstdevtk`][@stdlib/stats/base/nanstdevtk]</span><span class="delimiter">: </span><span class="description">calculate the standard deviation of a strided array ignoring NaN values and using a one-pass textbook algorithm.</span>
-   <span class="package-name">[`@stdlib/stats-base/snanstdev`][@stdlib/stats/base/snanstdev]</span><span class="delimiter">: </span><span class="description">calculate the standard deviation of a single-precision floating-point strided array ignoring NaN values.</span>
-   <span class="package-name">[`@stdlib/stats-base/snanvariancetk`][@stdlib/stats/base/snanvariancetk]</span><span class="delimiter">: </span><span class="description">calculate the variance of a single-precision floating-point strided array ignoring NaN values and using a one-pass textbook algorithm.</span>
-   <span class="package-name">[`@stdlib/stats-base/sstdevtk`][@stdlib/stats/base/sstdevtk]</span><span class="delimiter">: </span><span class="description">calculate the standard deviation of a single-precision floating-point strided array using a one-pass textbook algorithm.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2023. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/stats-base-snanstdevtk.svg
[npm-url]: https://npmjs.org/package/@stdlib/stats-base-snanstdevtk

[test-image]: https://github.com/stdlib-js/stats-base-snanstdevtk/actions/workflows/test.yml/badge.svg?branch=v0.1.0
[test-url]: https://github.com/stdlib-js/stats-base-snanstdevtk/actions/workflows/test.yml?query=branch:v0.1.0

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/stats-base-snanstdevtk/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/stats-base-snanstdevtk?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/stats-base-snanstdevtk.svg
[dependencies-url]: https://david-dm.org/stdlib-js/stats-base-snanstdevtk/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/stats-base-snanstdevtk/tree/deno
[umd-url]: https://github.com/stdlib-js/stats-base-snanstdevtk/tree/umd
[esm-url]: https://github.com/stdlib-js/stats-base-snanstdevtk/tree/esm
[branches-url]: https://github.com/stdlib-js/stats-base-snanstdevtk/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/stats-base-snanstdevtk/main/LICENSE

[standard-deviation]: https://en.wikipedia.org/wiki/Standard_deviation

[@stdlib/array/float32]: https://github.com/stdlib-js/array-float32/tree/deno

[mdn-typed-array]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray

[@ling:1974a]: https://doi.org/10.2307/2286154

<!-- <related-links> -->

[@stdlib/stats/base/dnanstdevtk]: https://github.com/stdlib-js/stats-base-dnanstdevtk/tree/deno

[@stdlib/stats/base/nanstdevtk]: https://github.com/stdlib-js/stats-base-nanstdevtk/tree/deno

[@stdlib/stats/base/snanstdev]: https://github.com/stdlib-js/stats-base-snanstdev/tree/deno

[@stdlib/stats/base/snanvariancetk]: https://github.com/stdlib-js/stats-base-snanvariancetk/tree/deno

[@stdlib/stats/base/sstdevtk]: https://github.com/stdlib-js/stats-base-sstdevtk/tree/deno

<!-- </related-links> -->

</section>

<!-- /.links -->
