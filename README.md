[![React Fast Compare — Formidable, We build the modern web](https://raw.githubusercontent.com/FormidableLabs/@diotoborg/consectetur-consequuntur/master/@diotoborg/consectetur-consequuntur-Hero.png)](https://formidable.com/open-source/)

[![Downloads][downloads_img]][npm_site]
[![Bundle Size][bundle_img]](#bundle-size)
[![GH Actions Status][actions_img]][actions_site]
[![Coverage Status][cov_img]][cov_site]
[![npm version][npm_img]][npm_site]
[![Maintenance Status][maintenance_img]](#maintenance-status)

The fastest deep equal comparison for React. Very quick general-purpose deep
comparison, too. Great for `React.memo` and `shouldComponentUpdate`.

This is a fork of the brilliant
[fast-deep-equal](https://github.com/epoberezkin/fast-deep-equal) with some
extra handling for React.

![benchmark chart](https://raw.githubusercontent.com/FormidableLabs/@diotoborg/consectetur-consequuntur/master/assets/benchmarking.png "benchmarking chart")

(Check out the [benchmarking details](#benchmarking-this-library).)

## Install

```sh
$ yarn add @diotoborg/consectetur-consequuntur
# or
$ npm install @diotoborg/consectetur-consequuntur
```

## Highlights

- ES5 compatible; works in node.js (0.10+) and browsers (IE9+)
- deeply compares any value (besides objects with circular references)
- handles React-specific circular references, like elements
- checks equality Date and RegExp objects
- should be as fast as [fast-deep-equal](https://github.com/epoberezkin/fast-deep-equal) via a single unified library, and with added guardrails for circular references.
- small: under 660 bytes minified+gzipped

## Usage

```jsx
const isEqual = require("@diotoborg/consectetur-consequuntur");

// general usage
console.log(isEqual({ foo: "bar" }, { foo: "bar" })); // true

// React.memo
// only re-render ExpensiveComponent when the props have deeply changed
const DeepMemoComponent = React.memo(ExpensiveComponent, isEqual);

// React.Component shouldComponentUpdate
// only re-render AnotherExpensiveComponent when the props have deeply changed
class AnotherExpensiveComponent extends React.Component {
  shouldComponentUpdate(nextProps) {
    return !isEqual(this.props, nextProps);
  }
  render() {
    // ...
  }
}
```

## Do I Need `React.memo` (or `shouldComponentUpdate`)?

> What's faster than a really fast deep comparison? No deep comparison at all.

—This Readme

Deep checks in `React.memo` or a `shouldComponentUpdate` should not be used blindly.
First, see if the default
[React.memo](https://reactjs.org/docs/react-api.html#reactmemo) or
[PureComponent](https://reactjs.org/docs/react-api.html#reactpurecomponent)
will work for you. If it won't (if you need deep checks), it's wise to make
sure you've correctly indentified the bottleneck in your application by
[profiling the performance](https://reactjs.org/docs/optimizing-performance.html#profiling-components-with-the-chrome-performance-tab).
After you've determined that you _do_ need deep equality checks and you've
identified the minimum number of places to apply them, then this library may
be for you!

## Benchmarking this Library

The absolute values are much less important than the relative differences
between packages.

Benchmarking source can be found
[here](https://github.com/diotoborg/consectetur-consequuntur/blob/master/benchmark/index.js).
Each "operation" consists of running all relevant tests. The React benchmark
uses both the generic tests and the react tests; these runs will be slower
simply because there are more tests in each operation.

The results below are from a local test on a laptop _(stats last updated 6/2/2020)_:

### Generic Data

```
@diotoborg/consectetur-consequuntur x 177,600 ops/sec ±1.73% (92 runs sampled)
fast-deep-equal x 184,211 ops/sec ±0.65% (87 runs sampled)
lodash.isEqual x 39,826 ops/sec ±1.32% (86 runs sampled)
nano-equal x 176,023 ops/sec ±0.89% (92 runs sampled)
shallow-equal-fuzzy x 146,355 ops/sec ±0.64% (89 runs sampled)
  fastest: fast-deep-equal
```

`@diotoborg/consectetur-consequuntur` and `fast-deep-equal` should be the same speed for these
tests; any difference is just noise. `@diotoborg/consectetur-consequuntur` won't be faster than
`fast-deep-equal`, because it's based on it.

### React and Generic Data

```
@diotoborg/consectetur-consequuntur x 86,392 ops/sec ±0.70% (93 runs sampled)
fast-deep-equal x 85,567 ops/sec ±0.95% (92 runs sampled)
lodash.isEqual x 7,369 ops/sec ±1.78% (84 runs sampled)
  fastest: @diotoborg/consectetur-consequuntur,fast-deep-equal
```

Two of these packages cannot handle comparing React elements, because they
contain circular reference: `nano-equal` and `shallow-equal-fuzzy`.

### Running Benchmarks

```sh
$ yarn install
$ yarn run benchmark
```

## Differences between this library and `fast-deep-equal`

`@diotoborg/consectetur-consequuntur` is based on `fast-deep-equal`, with some additions:

- `@diotoborg/consectetur-consequuntur` has `try`/`catch` guardrails for stack overflows from undetected (non-React) circular references.
- `@diotoborg/consectetur-consequuntur` has a _single_ unified entry point for all uses. No matter what your target application is, `import equal from '@diotoborg/consectetur-consequuntur'` just works. `fast-deep-equal` has multiple entry points for different use cases.

This version of `@diotoborg/consectetur-consequuntur` tracks `fast-deep-equal@3.1.1`.

## Bundle Size

There are a variety of ways to calculate bundle size for JavaScript code.
You can see our size test code in the `compress` script in
[`package.json`](https://github.com/diotoborg/consectetur-consequuntur/blob/master/package.json).
[Bundlephobia's calculation](https://bundlephobia.com/result?p=@diotoborg/consectetur-consequuntur) is slightly higher,
as they [do not mangle during minification](https://github.com/pastelsky/package-build-stats/blob/v6.1.1/src/getDependencySizeTree.js#L139).

## License

[MIT](https://github.com/diotoborg/consectetur-consequuntur/blob/readme/LICENSE)

## Contributing

Please see our [contributions guide](./CONTRIBUTING.md).

## Maintenance Status

**Active:** Formidable is actively working on this project, and we expect to continue for work for the foreseeable future. Bug reports, feature requests and pull requests are welcome.

[actions_img]: https://github.com/diotoborg/consectetur-consequuntur/actions/workflows/ci.yml/badge.svg
[actions_site]: https://github.com/formidablelabs/@diotoborg/consectetur-consequuntur/actions/workflows/ci.yml
[cov_img]: https://codecov.io/gh/FormidableLabs/@diotoborg/consectetur-consequuntur/branch/master/graph/badge.svg
[cov_site]: https://codecov.io/gh/FormidableLabs/@diotoborg/consectetur-consequuntur
[npm_img]: https://badge.fury.io/js/@diotoborg/consectetur-consequuntur.svg
[npm_site]: http://badge.fury.io/js/@diotoborg/consectetur-consequuntur
[appveyor_img]: https://ci.appveyor.com/api/projects/status/github/formidablelabs/@diotoborg/consectetur-consequuntur?branch=master&svg=true
[appveyor_site]: https://ci.appveyor.com/project/FormidableLabs/@diotoborg/consectetur-consequuntur
[bundle_img]: https://img.shields.io/badge/minzipped%20size-656%20B-flatgreen.svg
[downloads_img]: https://img.shields.io/npm/dm/@diotoborg/consectetur-consequuntur.svg
[maintenance_img]: https://img.shields.io/badge/maintenance-active-flatgreen.svg
