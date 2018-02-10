# pipe-me

A clean & functional way to describe your app.

Under the hood, it uses the [pipeline operator](https://github.com/tc39/proposal-pipeline-operator) and [callbags](https://github.com/callbag/callbag).

Supports promises/async, iterators/generators, events, & observables to provide a hybrid of reactive and interactive programming.

If you are new to JavaScript, this library may sound complicated, but bear with it. Do you know how to use spreadsheets? Well then, you already understand the basics concepts behind this library.

## Status

[![npm](https://img.shields.io/npm/v/pipe-me.svg)]()
[![Build Status](https://travis-ci.org/sartaj/pipe-me.svg?branch=master)](https://travis-ci.org/sartaj/pipe-me)
[![GitHub issues](https://img.shields.io/github/issues/sartaj/pipe-me.svg)](https://github.com/sartaj/pipe-me/issues)
[![codecov](https://codecov.io/gh/sartaj/pipe-me/branch/master/graph/badge.svg)](https://codecov.io/gh/sartaj/pipe-me)
[![npm Downloads](https://img.shields.io/npm/dm/pipe-me.svg)]()
[![Dependencies](https://img.shields.io/david/sartaj/pipe-me.svg)]()
[![DevDependencies](https://img.shields.io/david/dev/sartaj/pipe-me.svg)]()
[![Known Vulnerabilities](https://snyk.io/test/github/sartaj/pipe-me/badge.svg)](https://snyk.io/test/github/sartaj/pipe-me)

## Example

```javascript
import { fromEvent, filter, map, merge, sideEffect } from 'pipe-me'

import { getRandomFruit, getCurrentDateTime } from './utils'
import { renderDOM } from './dom'

const buttonClicked = fromEvent(document, 'click')
  |> filter(event => event.target.tagName === 'BUTTON')
  |> map(() => ({ date: getCurrentDateTime(), fruit: getRandomFruit() }))

buttonClicked
  |> sideEffect(renderDOM)

buttonClicked
  |> sideEffect(state => console.log(state))
```

### Try It Out

* Options 1: Clone this repo. Run `yarn` or `npm install`. Then `yarn example`. Then click the button in the example and watch the DOM and console both print at the same time with such little effort.
* Option 2: Set up your your envrionment with Babel, as listed in the instructions below.

## Setup

### Yarn/npm

```bash
yarn add pipe-me
```

```bash
npm install pipe-me --save
```

### Babel

To use the [pipeline operator](https://github.com/tc39/proposal-pipeline-operator), you'll need to set up babel with the [pipeline-operator plugin](https://github.com/babel/babel/tree/master/packages/babel-plugin-proposal-pipeline-operator).

```json
yarn add @babel/cli @babel/preset-env @babel/preset-plugin-proposal-pipeline --dev
```

```json
{
    "presets": ["@babel/preset-env"],
    "plugins": ["@babel/plugin-proposal-pipeline-operator "]
}
```

### Ways To Import

If you are new to paradigms like this (found in systems like RxJS and IxJS), sometimes it can be hard to remember the purpose of different operators. To simplify this, you can import from 5 different categories.

```js
import { ... } from 'pipe-me'
import { map, accumulate } from 'pipe-me/transforms'
import { take, skip, filter } from 'pipe-me/filters'
import { fromObservable, fromAsync, fromIterable, fromEvent, fromPromise } from 'pipe-me/create'
import { merge, concat, combine, switchTo, flatten } from 'pipe-me/combiners'
import { sideEffect } from 'pipe-me/side-effects'
```

## Goals

### Designed For

* Beginners who may have just fairly new to programming, but have completed either an online intensive or code school.
* Experts who may desire a simple dependency that covers most app use cases.

### Purpose

The purpose of `pipe-me` is to provide an API design with gradual learning in mind. Currently, the API is mostly using operators found in [callbag-basics](https://github.com/staltz/callbag-basics) as a proof of concept. Over time, parts of this may diverge, as I intend to survey multiple code school students on their understanding of different names.

### Is This Standard JavaScript?

While `pipe-me` makes opinions on naming conventions of operators, under the hood is just a mash of two proposal specs, the [pipeline operator](https://github.com/tc39/proposal-pipeline-operator), and [callbags](https://github.com/callbag/callbag). 

In terms of the the [pipeline operator](https://github.com/tc39/proposal-pipeline-operator), it is currently a TC39 proposal, similar to object spread. So technically it is subject to change. However, based on [the issues in the proposal](https://github.com/tc39/proposal-pipeline-operator/issues), most concerns are around how to handle the `await` syntax and multiple parameters. These issues are a mute point in `pipe-me`, because by using the `callbag` spec, all of our non combining operators are single parameter, and `async/await` is handled by the `fromIterable` and `fromAsync` operator.

In terms of the actual operators themselves, the far majority of the magic here is possible because of André Staltz's brilliant [callbag](https://github.com/callbag/callbag) spec. Because callbags are functional compositions, and because pipeline operators are just function compositions under the hood, any callbag can be used with pipeline operators.

This actually means you can use this library with any other callbag library to unleash this awesome writing style in JS.
