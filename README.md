<!-- 
This README describes the package. If you publish this package to pub.dev,
this README's contents appear on the landing page for your package.

For information about how to write a good package README, see the guide for
[writing package pages](https://dart.dev/guides/libraries/writing-package-pages). 

For general information about developing packages, see the Dart guide for
[creating packages](https://dart.dev/guides/libraries/create-library-packages)
and the Flutter guide for
[developing packages and plugins](https://flutter.dev/developing-packages). 
-->

# 🧪 Parameterized test

Supercharge your Dart testing with parameterized_test! Built on top of the [dart test package](https://pub.dev/packages/test), this [JUnit ParameterizedTest](https://junit.org/junit5/docs/current/user-guide/#writing-tests-parameterized-tests) inspired package wrap around `group` and `test` to take your testing to the next level!

## Table of contents
* [Features](#features-)
* [Getting started](#getting-started)
* [Usage](#usage)
* [Examples](#examples)
* [How it works](#how-it-works)
* [Additional information](#additional-information)

## Features ✨

- ✅ Run a test multiple times based on provide parameter list.
- ✅ Uses [dart test package](https://pub.dev/packages/test) under the hood.
- ✅ Type cast test parameters used in the tests.
- ✅ Include test options for parameter_test.
- ✅ Include test options per parameters.

## Getting started

Include `parameterized_test` in your projects `pubspec.yaml`.

## Usage 

Instead of using `groups` or `test` you can now use `parameterizedTest` and supply it list of test parameters to use in the same test.
To specify the test body use `TestParametersX` that matches the same amount of test parameters for 1 test. For example when the test has 2 parameters `actual` and `expected` use `TestParameters2` for supplying the test body.
The package offers `TestParameters` classes up to 10 parameters. Instead of writing `TestParameters` completely it also possible to use `typedef`'s `p1`..`p10`.

## Examples

Example parameterizedTest with 1 parameter:

```dart
parameterizedTest(
  'Number are less than 4 tests',
  [
    1,
    2,
    3,
  ],
  p1((int number) {
    final result = number < 4;
    expect(result, true);
  }),
);
```

Example parameterizedTest with 2 parameters:

```dart
parameterizedTest(
  'Amount of letters tests',
  [
    ['kiwi', 4],
    ['apple', 5],
    ['banana', 6],
  ],
  p2((String word, int length) {
    expect(word.length, length);
  }),
);
```

Example parameterizedTest with extra test options for a value:

```dart
parameterizedTest(
  'Amount of letters',
  [
    ['kiwi', 4],
    ['apple', 5],
    ['banana', 6].withTestOptions(skip: true),
  ],
  p2((String word, int length) {
    expect(word.length, length);
  }),
);
```

Example parameterizedTest with test enum values:

```dart
enum AwesomeEnum { such, woow, much, skill}

parameterizedTest(
  'Doge enum tests',
  AwesomeEnum.values,
  p1((AwesomeEnum doge) {
    final result = doge.name.length == 4;
    expect(result, true);
  }),
);
```

## How it works

`parameterized_test` is basically a wrapper that executes a `group` test and loops over the provide `List` of test values. Each set of values is cast to the specified type inside the body. Which is wrapped inside a `test`.

```dart
parameterizedTest(
  'Amount of letters',
  [
    ['kiwi', 4],
    ['apple', 5],
    ['banana', 6],
  ],
  p2((String word, int length) {
    expect(word.length, length);
  }),
);
```

The above example roughly translates to:
All parameterizedTest arguments except `values` & `body` are passed to a dart test `group`. 
For the list of `values` it will loop over each value and execute a new `test` for it. So `['kiwi', 4]` will executed in the first `test` and `['banana', 6]` in the last `test`.
Each set of test values (`['banana', 6]`) will passed to the `TestParameters` body class which will map and cast the values to the provided `body` function. And finally execute the `body`.

## Additional information

Its just a simple wrapper to easily execute tests multiple times with different values. Feel free to
leave some feedback or open an pull request :)