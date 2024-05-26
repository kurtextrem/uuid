# @lukeed/uuid ![CI](https://github.com/lukeed/uuid/workflows/CI/badge.svg) [![codecov](https://badgen.now.sh/codecov/c/github/lukeed/uuid)](https://codecov.io/gh/lukeed/uuid)

> A tiny (~230B) and [fast](#benchmarks) UUID (v4) generator for Node and the browser.

This module offers two [modes](#modes) for your needs:

* [`@lukeed/uuid`](#lukeeduuid)<br>_The default is "non-secure", which uses `Math.random` to produce UUIDs._
* [`@lukeed/uuid/secure`](#lukeeduuidsecure)<br>_The "secure" mode produces cryptographically secure (CSPRNG) UUIDs using the current environment's `crypto` module._

> **Important:** <br>Version `1.0.0` only offered a "secure" implementation.<br>In `v2.0.0`, this is now exported as the `"@lukeed/uuid/secure"` entry.

Additionally, this module is preconfigured for native ESM support in Node.js with fallback to CommonJS. It will also work with any Rollup and webpack configuration.


## Install

```
$ npm install --save @lukeed/uuid
```

## Modes

There are two "versions" of `@lukeed/uuid` available:

#### `@lukeed/uuid`
> **Size (gzip):** 231 bytes<br>
> **Availability:** [CommonJS](https://unpkg.com/@lukeed/uuid/dist/index.js), [ES Module](https://unpkg.com/@lukeed/uuid/dist/index.mjs), [UMD](https://unpkg.com/@lukeed/uuid/dist/index.min.js)

Relies on `Math.random`, which means that, while faster, this mode **is not** cryptographically secure. <br>Works in Node.js and all browsers.

#### `@lukeed/uuid/secure`
> **Size (gzip):** 235 bytes<br>
> **Availability:** [CommonJS](https://unpkg.com/@lukeed/uuid/secure/index.js), [ES Module](https://unpkg.com/@lukeed/uuid/secure/index.mjs), [UMD](https://unpkg.com/@lukeed/uuid/secure/index.min.js)

Relies on the environment's `crypto` module in order to produce cryptographically secure (CSPRNG) values. <br>Works in all versions of Node.js. Works in all browsers with [`crypto.getRandomValues()` support](https://caniuse.com/#feat=getrandomvalues).


## Usage

```js
import { v4 as uuid } from '@lukeed/uuid';
import { v4 as secure } from '@lukeed/uuid/secure';

uuid(); //=> '400fa120-5e9f-411e-94bd-2a23f6695704'
uuid(); //=> 'cd6ffb4d-2eda-4c84-aef5-71eb360ac8c5'

secure(); //=> '8641f70e-8112-4168-9d81-d38170bfa612'
secure(); //=> 'd175fabc-2a4d-475f-be56-29ba8104c2f2'
```


## API

### uuid.v4()
Returns: `string`

Creates a new Version 4 (random) [RFC4122](http://www.ietf.org/rfc/rfc4122.txt) UUID.


## Benchmarks

> Running on Node.js v17.0.1

```
Validation:
  ✔ String.replace(Math.random)
  ✔ String.replace(crypto)
  ✔ uuid/v4
  ✔ @lukeed/uuid
  ✔ @lukeed/uuid/secure
  ✔ uuid-quick
  ✔ hyperid
  ✔ mongoid
  ✔ KSUID
  ✔ ulidx

Benchmark:
  String.replace(Math.random)  x 364,566 ops/sec ±0.51% (93 runs sampled)
  String.replace(crypto)       x 12,898 ops/sec ±0.53% (91 runs sampled)
  uuid/v4                      x 1,342,666 ops/sec ±0.21% (96 runs sampled)
  @lukeed/uuid                 x 6,397,525 ops/sec ±0.49% (94 runs sampled)
  @lukeed/uuid/secure          x 6,290,642 ops/sec ±0.81% (95 runs sampled)
  uuid-quick                   x 6,441,972 ops/sec ±0.47% (95 runs sampled)   # in some runs slower than luuked/uuid
  hyperid                      x 16,887,066 ops/sec ±0.73% (96 runs sampled)  # fastest, non-sequential but w/ random running number padding
  mongoid                      x 19,163,326 ops/sec ±20.80% (71 runs sampled) # fastest, sequential
  KSUID                        x 100,178 ops/sec ±0.67% (95 runs sampled)
  ulidx                        x 21,173 ops/sec ±1.99% (80 runs sampled)

Validation:
  ✔ crypto.randomUUID
  ✔ @lukeed/uuid
  ✔ @lukeed/uuid/secure
  ✔ @napi-rs/uuid
  ✔ uuid-quick
  ✔ hyperid
  ✔ mongoid
  ✔ KSUID
  ✔ ulidx
  ✔ napi-nanoid
  ✔ uid
  ✔ uid/secure
  ✔ uid/single
  ✔ hexoid

Benchmark:
  crypto.randomUUID            x 20,520,775 ops/sec ±1.29% (86 runs sampled)
  @lukeed/uuid                 x 6,594,182 ops/sec ±0.59% (93 runs sampled)
  @lukeed/uuid/secure          x 6,402,447 ops/sec ±0.31% (94 runs sampled)
  @napi-rs/uuid                x 3,843,522 ops/sec ±0.36% (95 runs sampled)
  uuid-quick                   x 6,986,841 ops/sec ±0.67% (97 runs sampled)
  hyperid                      x 14,923,670 ops/sec ±1.08% (90 runs sampled)  # fastest, non-sequential but w/ random running number padding
  mongoid                      x 18,498,012 ops/sec ±19.21% (71 runs sampled) # fastest, sequential
  KSUID                        x 98,264 ops/sec ±0.34% (96 runs sampled)      # k-sortable
  ulidx                        x 21,158 ops/sec ±1.99% (84 runs sampled)      # lex sortable
  napi-nanoid                  x 4,285,115 ops/sec ±0.24% (95 runs sampled)
  uid                          x 18,391,465 ops/sec ±0.25% (95 runs sampled)
  uid/secure                   x 10,892,492 ops/sec ±0.45% (96 runs sampled)
  uid/single                   x 4,898,395 ops/sec ±0.29% (96 runs sampled)
  hexoid                       x 53,970,382 ops/sec ±0.28% (96 runs sampled)
```

> Running on Chrome v85.0.4183.121

```
Validation:
  ✔ String.replace(Math.random)
  ✔ uuid/v4
  ✔ @lukeed/uuid
  ✔ @lukeed/uuid/secure

Benchmark:
  String.replace(Math.random)  x    313,213 ops/sec ±0.58% (65 runs sampled)
  uuid/v4                      x    302,914 ops/sec ±0.94% (64 runs sampled)
  @lukeed/uuid                 x  5,881,761 ops/sec ±1.29% (62 runs sampled)
  @lukeed/uuid/secure          x    852,939 ops/sec ±0.88% (65 runs sampled)
```

## Performance

The reason why this UUID.V4 implementation is so much faster is two-fold:

1) It composes an output with hexadecimal pairs (from a cached dictionary) instead of single characters.
2) It allocates a larger Buffer/ArrayBuffer up front (expensive) and slices off chunks as needed (cheap).

The `@lukeed/uuid/secure` module maintains an internal ArrayBuffer of 4096 bytes, which supplies **256** `uuid.v4()` invocations. However, the default module preallocates **256** invocations using less memory upfront. Both implementations will regenerate its internal allocation as needed.

A larger buffer would result in higher performance over time, but I found this to be a good balance of performance and memory space.

## License

MIT © [Luke Edwards](https://lukeed.com)
