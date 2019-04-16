<h1>Terminus ESLint Frontend Configuration</h1>

[![CircleCI][circle-badge]][circle-link]
[![semantic-release][semantic-release-badge]][semantic-release]
[![MIT License][license-image]][license-url]
[![NPM version][npm-version-image]][npm-url]
[![Github release][gh-release-badge]][gh-releases]

A collection of JavaScript lint rules for Terminus frontend codebases.

> For TSLint configuration, see: https://github.com/GetTerminus/tslint-config-frontend


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [Installation](#installation)
- [Rulesets overview](#rulesets-overview)
- [Set up](#set-up)
  - [CI](#ci)
  - [Development](#development)
- [Rule Decisions](#rule-decisions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


## Installation

1. Install the package:

```
$ yarn add eslint @terminus/eslint-config-frontend -D
```


## Rulesets overview

There are 2 rulesets defined for ESLint:

1. **[CI](#CI)**: This is the default ruleset. This is meant to be used during your CI builds so it throws hard errors when issues are
   found.
    - NOTE: `.spec` files have some rules relaxed.
2. **[Development](#Development)**: This enforces all the same rules as the CI ruleset but infractions are reported as warnings rather than
   errors.

> NOTE: TSLint requires a separate config for spec files while ESLint's configuration adjusts based on configuration within the base
> ruleset.


## Set up

You will need to set up separate configs and scripts for each ruleset: `ci` and `development`. Creating a custom script call for each within
your `package.json` will allow for easy composability of commands once all linters are set up.

### CI

[`CI Rules`](./ci.js)

#### 1. Create the file and extend our ruleset

Create a ESLint config file at the root level named `.eslintrc-ci` and extend the base ruleset:

- The `project` key should point to the primary app `tsconfig` file.

```json
{
  "extends": ["@terminus/eslint-config-frontend"],
  "parserOptions": {
    "ecmaVersion": 6,
    "project": "./tsconfig.json",
    "sourceType": "module"
  }
}
```

> Linting during the CI process is the most strict and will fail if _any_ issues are found. The only way a linting issue makes it to CI is
> because someone didn't lint before commiting.

#### 2. Add a linting command to `package.json`

- The `--config` flag reference should point to the `.eslintrc-ci` file.

```json
{
  "name": "My Project",
  "scripts": {
    "lint:eslint:ci": "npx eslint \"src/**/*.{js,ts}\" --config .eslintrc-ci"
  }
}
```


### Development

This ruleset extends the `ci` ruleset but softens the rules to turn many stylistic issues into warnings in order to not impede development.

[`Development Rules`](./development.js)

#### 1. Create the file and extend our ruleset

Create an ESLint config file at the root level named `.eslintrc` and extend the development ruleset:

- The `project` key should point to the primary app `tsconfig` file.

```json
{
  "extends": ["@terminus/frontend-lint-config/development"],
  "parserOptions": {
    "ecmaVersion": 6,
    "project": "./tsconfig.json",
    "sourceType": "module"
  }
}
```

#### 2. Add project specific rules

> NOTE: When adjusting an ESLint rule, the entire rule doesn **not** need to be defined again.

- The `project` key should point to the primary app `tsconfig` file.

```json
{
  "extends": ["@terminus/frontend-lint-config/development"],
  "parserOptions": {
    "ecmaVersion": 6,
    "project": "./tsconfig.json",
    "sourceType": "module"
  },
  "rules": {
    "quotes": ["off"]
  }
}
```


## Rule Decisions

Each rule is accompanied by a comment outlining the reasoning behind the decision to include the rule.

For most rules, see [`ci.js`](./ci.js).




<!--
  LINKS
-->

<!-- BADGES -->
[circle-badge]:           https://circleci.com/gh/GetTerminus/eslint-config-frontend/tree/release.svg?style=shield
[npm-version-image]:      http://img.shields.io/npm/v/@terminus/eslint-config-frontend.svg
[semantic-release-badge]: https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg
[license-image]:          http://img.shields.io/badge/license-MIT-blue.svg
[gh-release-badge]:       https://img.shields.io/github/release/GetTerminus/eslint-config-frontend.svg
[gh-releases]:            https://github.com/GetTerminus/eslint-config-frontend/releases/
[circle-link]:            https://circleci.com/gh/GetTerminus/eslint-config-frontend/tree/release
[semantic-release]:       https://github.com/semantic-release/semantic-release
[npm-url]:                https://npmjs.org/package/@terminus/eslint-config-frontend
[license-url]:            https://github.com/GetTerminus/eslint-config-frontend/blob/release/LICENSE
