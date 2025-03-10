<!--
  -- This file is auto-generated from README_js.md. Changes should be made there.
  -->


# uuid [![CI](https://github.com/uuidjs/uuid/workflows/CI/badge.svg)](https://github.com/uuidjs/uuid/actions?query=workflow%3ACI) [![Browser](https://github.com/uuidjs/uuid/workflows/Browser/badge.svg)](https://github.com/uuidjs/uuid/actions?query=workflow%3ABrowser)

For the creation of [RFC9562](https://www.rfc-editor.org/rfc/rfc9562.html) (formally [RFC4122](https://www.rfc-editor.org/rfc/rfc4122.html)) UUIDs

- **Complete** - Support for all RFC9562 UUID versions
- **Cross-platform** - Support for ...
  - CommonJS, [ECMAScript Modules](#ecmascript-modules)
  - NodeJS 16+ ([LTS releases](https://github.com/nodejs/Release))
  - Chrome, Safari, Firefox, Edge browsers
- **Secure** - Cryptographically-strong random values
- **Compact** - No dependencies, [tree-shakable](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking)
- **CLI** - Includes the [`uuid` command line](#command-line) utility
- **Typescript** - Types now included

<!-- prettier-ignore -->
> [!NOTE]
> `uuid@11` has the following breaking changes:
> * Passing `options` to `v1()`, `v6()`, and `v7()` now behaves slightly differently. [See details](#options-handling-for-timestamp-uuids)
> * Binary UUIDs are now of type `Uint8Array`.  This may affect code utilizing `parse()`, `stringify()`,  or that passes a `buf` argument to any of the `v1()`-`v7()` methods.

## Quickstart

**1. Install**

```shell
npm install uuid
```

**2. Create a UUID**

ESM-syntax (must use named exports):

```javascript
import { v4 as uuidv4 } from 'uuid';
uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
```

... CommonJS:

```javascript
const { v4: uuidv4 } = require('uuid');
uuidv4(); // ⇨ '1b9d6bcd-bbfd-4b2d-9b5d-ab8dfbbd4bed'
```

For timestamp UUIDs, namespace UUIDs, and other options read on ...

## API Summary

|  |  |  |
| --- | --- | --- |
| [`uuid.NIL`](#uuidnil) | The nil UUID string (all zeros) | New in `uuid@8.3` |
| [`uuid.MAX`](#uuidmax) | The max UUID string (all ones) | New in `uuid@9.1` |
| [`uuid.parse()`](#uuidparsestr) | Convert UUID string to array of bytes | New in `uuid@8.3` |
| [`uuid.stringify()`](#uuidstringifyarr-offset) | Convert array of bytes to UUID string | New in `uuid@8.3` |
| [`uuid.v1()`](#uuidv1options-buffer-offset) | Create a version 1 (timestamp) UUID |  |
| [`uuid.v1ToV6()`](#uuidv1tov6uuid) | Create a version 6 UUID from a version 1 UUID | New in `uuid@10` |
| [`uuid.v3()`](#uuidv3name-namespace-buffer-offset) | Create a version 3 (namespace w/ MD5) UUID |  |
| [`uuid.v4()`](#uuidv4options-buffer-offset) | Create a version 4 (random) UUID |  |
| [`uuid.v5()`](#uuidv5name-namespace-buffer-offset) | Create a version 5 (namespace w/ SHA-1) UUID |  |
| [`uuid.v6()`](#uuidv6options-buffer-offset) | Create a version 6 (timestamp, reordered) UUID | New in `uuid@10` |
| [`uuid.v6ToV1()`](#uuidv6tov1uuid) | Create a version 1 UUID from a version 6 UUID | New in `uuid@10` |
| [`uuid.v7()`](#uuidv7options-buffer-offset) | Create a version 7 (Unix Epoch time-based) UUID | New in `uuid@10` |
| ~~[`uuid.v8()`](#uuidv8)~~ | "Intentionally left blank" |  |
| [`uuid.validate()`](#uuidvalidatestr) | Test a string to see if it is a valid UUID | New in `uuid@8.3` |
| [`uuid.version()`](#uuidversionstr) | Detect RFC version of a UUID | New in `uuid@8.3` |

## API

### uuid.NIL

The nil UUID string (all zeros).

Example:

```javascript
import { NIL as NIL_UUID } from 'uuid';

NIL_UUID; // ⇨ '00000000-0000-0000-0000-000000000000'
```

### uuid.MAX

The max UUID string (all ones).

Example:

```javascript
import { MAX as MAX_UUID } from 'uuid';

MAX_UUID; // ⇨ 'ffffffff-ffff-ffff-ffff-ffffffffffff'
```

### uuid.parse(str)

Convert UUID string to array of bytes

|           |                                          |
| --------- | ---------------------------------------- |
| `str`     | A valid UUID `String`                    |
| _returns_ | `Uint8Array[16]`                         |
| _throws_  | `TypeError` if `str` is not a valid UUID |

<!-- prettier-ignore -->
> [!NOTE]
> Ordering of values in the byte arrays used by `parse()` and `stringify()` follows the left &Rarr; right order of hex-pairs in UUID strings. As shown in the example below.

Example:

```javascript
import { parse as uuidParse } from 'uuid';

// Parse a UUID
uuidParse('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'); // ⇨ 
  // Uint8Array(16) [
  //   110, 192, 189, 127,  17,
  //   192,  67, 218, 151,  94,
  //    42, 138, 217, 235, 174,
  //    11
  // ]
```

### uuid.stringify(arr[, offset])

Convert array of bytes to UUID string

|                |                                                                              |
| -------------- | ---------------------------------------------------------------------------- |
| `arr`          | `Array`-like collection of 16 values (starting from `offset`) between 0-255. |
| [`offset` = 0] | `Number` Starting index in the Array                                         |
| _returns_      | `String`                                                                     |
| _throws_       | `TypeError` if a valid UUID string cannot be generated                       |

<!-- prettier-ignore -->
> [!NOTE]
> Ordering of values in the byte arrays used by `parse()` and `stringify()` follows the left &Rarr; right order of hex-pairs in UUID strings. As shown in the example below.

Example:

```javascript
import { stringify as uuidStringify } from 'uuid';

const uuidBytes = Uint8Array.of(
  0x6e,
  0xc0,
  0xbd,
  0x7f,
  0x11,
  0xc0,
  0x43,
  0xda,
  0x97,
  0x5e,
  0x2a,
  0x8a,
  0xd9,
  0xeb,
  0xae,
  0x0b
);

uuidStringify(uuidBytes); // ⇨ '6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'
```

### uuid.v1([options[, buffer[, offset]]])

Create an RFC version 1 (timestamp) UUID

|  |  |
| --- | --- |
| [`options`] | `Object` with one or more of the following properties: |
| [`options.node` ] | RFC "node" field as an `Array[6]` of byte values (per 4.1.6) |
| [`options.clockseq`] | RFC "clock sequence" as a `Number` between 0 - 0x3fff |
| [`options.msecs`] | RFC "timestamp" field (`Number` of milliseconds, unix epoch) |
| [`options.nsecs`] | RFC "timestamp" field (`Number` of nanoseconds to add to `msecs`, should be 0-10,000) |
| [`options.random`] | `Array` of 16 random bytes (0-255) |
| [`options.rng`] | Alternative to `options.random`, a `Function` that returns an `Array` of 16 random bytes (0-255) |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |
| _throws_ | `Error` if more than 10M UUIDs/sec are requested |

<!-- prettier-ignore -->
> [!NOTE]
> The default [node id](https://datatracker.ietf.org/doc/html/rfc9562#section-5.1) (the last 12 digits in the UUID) is generated once, randomly, on process startup, and then remains unchanged for the duration of the process.

<!-- prettier-ignore -->
> [!NOTE]
> `options.random` and `options.rng` are only meaningful on the very first call to `v1()`, where they may be passed to initialize the internal `node` and `clockseq` fields.

Example:

```javascript
import { v1 as uuidv1 } from 'uuid';

uuidv1(); // ⇨ '2c5ea4c0-4067-11e9-9bdd-2b0d7b3dcb6d'
```

Example using `options`:

```javascript
import { v1 as uuidv1 } from 'uuid';

const options = {
  node: Uint8Array.of(0x01, 0x23, 0x45, 0x67, 0x89, 0xab),
  clockseq: 0x1234,
  msecs: new Date('2011-11-01').getTime(),
  nsecs: 5678,
};
uuidv1(options); // ⇨ '710b962e-041c-11e1-9234-0123456789ab'
```

### uuid.v1ToV6(uuid)

Convert a UUID from version 1 to version 6

```javascript
import { v1ToV6 } from 'uuid';

v1ToV6('92f62d9e-22c4-11ef-97e9-325096b39f47'); // ⇨ '1ef22c49-2f62-6d9e-97e9-325096b39f47'
```

### uuid.v3(name, namespace[, buffer[, offset]])

Create an RFC version 3 (namespace w/ MD5) UUID

API is identical to `v5()`, but uses "v3" instead.

<!-- prettier-ignore -->
> [!IMPORTANT]
> Per the RFC, "_If backward compatibility is not an issue, SHA-1 [Version 5] is preferred_."

### uuid.v4([options[, buffer[, offset]]])

Create an RFC version 4 (random) UUID

|  |  |
| --- | --- |
| [`options`] | `Object` with one or more of the following properties: |
| [`options.random`] | `Array` of 16 random bytes (0-255) |
| [`options.rng`] | Alternative to `options.random`, a `Function` that returns an `Array` of 16 random bytes (0-255) |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |

Example:

```javascript
import { v4 as uuidv4 } from 'uuid';

uuidv4(); // ⇨ '9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d'
```

Example using predefined `random` values:

```javascript
import { v4 as uuidv4 } from 'uuid';

const v4options = {
  random: Uint8Array.of(
    0x10,
    0x91,
    0x56,
    0xbe,
    0xc4,
    0xfb,
    0xc1,
    0xea,
    0x71,
    0xb4,
    0xef,
    0xe1,
    0x67,
    0x1c,
    0x58,
    0x36
  ),
};
uuidv4(v4options); // ⇨ '109156be-c4fb-41ea-b1b4-efe1671c5836'
```

### uuid.v5(name, namespace[, buffer[, offset]])

Create an RFC version 5 (namespace w/ SHA-1) UUID

|  |  |
| --- | --- |
| `name` | `String \| Array` |
| `namespace` | `String \| Array[16]` Namespace UUID |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |

<!-- prettier-ignore -->
> [!NOTE]
> The RFC `DNS` and `URL` namespaces are available as `v5.DNS` and `v5.URL`.

Example with custom namespace:

```javascript
import { v5 as uuidv5 } from 'uuid';

// Define a custom namespace.  Readers, create your own using something like
// https://www.uuidgenerator.net/
const MY_NAMESPACE = '1b671a64-40d5-491e-99b0-da01ff1f3341';

uuidv5('Hello, World!', MY_NAMESPACE); // ⇨ '630eb68f-e0fa-5ecc-887a-7c7a62614681'
```

Example with RFC `URL` namespace:

```javascript
import { v5 as uuidv5 } from 'uuid';

uuidv5('https://www.w3.org/', uuidv5.URL); // ⇨ 'c106a26a-21bb-5538-8bf2-57095d1976c1'
```

### uuid.v6([options[, buffer[, offset]]])

Create an RFC version 6 (timestamp, reordered) UUID

This method takes the same arguments as uuid.v1().

```javascript
import { v6 as uuidv6 } from 'uuid';

uuidv6(); // ⇨ '1e940672-c5ea-64c0-9b5d-ab8dfbbd4bed'
```

Example using `options`:

```javascript
import { v6 as uuidv6 } from 'uuid';

const options = {
  node: [0x01, 0x23, 0x45, 0x67, 0x89, 0xab],
  clockseq: 0x1234,
  msecs: new Date('2011-11-01').getTime(),
  nsecs: 5678,
};
uuidv6(options); // ⇨ '1e1041c7-10b9-662e-9234-0123456789ab'
```

### uuid.v6ToV1(uuid)

Convert a UUID from version 6 to version 1

```javascript
import { v6ToV1 } from 'uuid';

v6ToV1('1ef22c49-2f62-6d9e-97e9-325096b39f47'); // ⇨ '92f62d9e-22c4-11ef-97e9-325096b39f47'
```

### uuid.v7([options[, buffer[, offset]]])

Create an RFC version 7 (random) UUID

|  |  |
| --- | --- |
| [`options`] | `Object` with one or more of the following properties: |
| [`options.msecs`] | RFC "timestamp" field (`Number` of milliseconds, unix epoch). Default = `Date.now()` |
| [`options.random`] | `Array` of 16 random bytes (0-255) |
| [`options.rng`] | Alternative to `options.random`, a `Function` that returns an `Array` of 16 random bytes (0-255) |
| [`options.seq`] | 32-bit sequence `Number` between 0 - 0xffffffff. This may be provided to help insure uniqueness for UUIDs generated within the same millisecond time interval. Default = random value. |
| [`buffer`] | `Array \| Buffer` If specified, uuid will be written here in byte-form, starting at `offset` |
| [`offset` = 0] | `Number` Index to start writing UUID bytes in `buffer` |
| _returns_ | UUID `String` if no `buffer` is specified, otherwise returns `buffer` |

Example:

```javascript
import { v7 as uuidv7 } from 'uuid';

uuidv7(); // ⇨ '01695553-c90c-705a-b56d-778dfbbd4bed'
```

### ~~uuid.v8()~~

**_"Intentionally left blank"_**

<!-- prettier-ignore -->
> [!NOTE]
> Version 8 (experimental) UUIDs are "[for experimental or vendor-specific use cases](https://www.rfc-editor.org/rfc/rfc9562.html#name-uuid-version-8)".  The RFC does not define a creation algorithm for them, which is why this package does not offer a `v8()` method.  The `validate()` and `version()` methods do work with such UUIDs, however.

### uuid.validate(str)

Test a string to see if it is a valid UUID

|           |                                                     |
| --------- | --------------------------------------------------- |
| `str`     | `String` to validate                                |
| _returns_ | `true` if string is a valid UUID, `false` otherwise |

Example:

```javascript
import { validate as uuidValidate } from 'uuid';

uuidValidate('not a UUID'); // ⇨ false
uuidValidate('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'); // ⇨ true
```

Using `validate` and `version` together it is possible to do per-version validation, e.g. validate for only v4 UUIds.

```javascript
import { version as uuidVersion } from 'uuid';
import { validate as uuidValidate } from 'uuid';

function uuidValidateV4(uuid) {
  return uuidValidate(uuid) && uuidVersion(uuid) === 4;
}

const v1Uuid = 'd9428888-122b-11e1-b85c-61cd3cbb3210';
const v4Uuid = '109156be-c4fb-41ea-b1b4-efe1671c5836';

uuidValidateV4(v4Uuid); // ⇨ true
uuidValidateV4(v1Uuid); // ⇨ false
```

### uuid.version(str)

Detect RFC version of a UUID

|           |                                          |
| --------- | ---------------------------------------- |
| `str`     | A valid UUID `String`                    |
| _returns_ | `Number` The RFC version of the UUID     |
| _throws_  | `TypeError` if `str` is not a valid UUID |

Example:

```javascript
import { version as uuidVersion } from 'uuid';

uuidVersion('45637ec4-c85f-11ea-87d0-0242ac130003'); // ⇨ 1
uuidVersion('6ec0bd7f-11c0-43da-975e-2a8ad9ebae0b'); // ⇨ 4
```

<!-- prettier-ignore -->
> [!NOTE]
> This method returns `0` for the `NIL` UUID, and `15` for the `MAX` UUID.

## Command Line

UUIDs can be generated from the command line using `uuid`.

```shell
$ npx uuid
ddeb27fb-d9a0-4624-be4d-4615062daed4
```

The default is to generate version 4 UUIDS, however the other versions are supported. Type `uuid --help` for details:

```shell
$ npx uuid --help

Usage:
  uuid
  uuid v1
  uuid v3 <name> <namespace uuid>
  uuid v4
  uuid v5 <name> <namespace uuid>
  uuid v7
  uuid --help

Note: <namespace uuid> may be "URL" or "DNS" to use the corresponding UUIDs
defined by RFC9562
```

## `options` Handling for Timestamp UUIDs

As of `uuid@11`, all timestamp-based UUID APIs (`v1()`, `v6()`, and `v7()`) now operate in two distinct modes:

- Without `options`: If no `options` argument is passed, these APIs will make use of internal state such as a sequence counter to improve UUID uniqueness.
- With `options`: If an `options` argument of any kind is passed, no internal state is used or updated. Instead, appropriate defaults are used. See the respective APIs for details.

Prior to `uuid@11`, this distinction was less clear. Internal state was was being combined with `options` values in ways that were difficult to rationalize about, and that could lead to unpredictable behavior. Hence, this change.

---


Markdown generated from [README_js.md](README_js.md) by <a href="https://github.com/broofa/runmd"><image height="13px" src="https://camo.githubusercontent.com/5c7c603cd1e6a43370b0a5063d457e0dabb74cf317adc7baba183acb686ee8d0/687474703a2f2f692e696d6775722e636f6d2f634a4b6f3662552e706e67" /></a>
