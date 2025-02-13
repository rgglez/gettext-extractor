# gettext-extractor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) 
![GitHub all releases](https://img.shields.io/github/downloads/rgglez/gettext-extractor/total)
![GitHub issues](https://img.shields.io/github/issues/rgglez/gettext-extractor)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/rgglez/gettext-extractor)

A flexible and powerful [Gettext](https://www.gnu.org/software/gettext/) message extractor with support for JavaScript, TypeScript, JSX and HTML, and others via  [regular expressions](https://en.wikipedia.org/wiki/Regular_expression).

This program works by running your files through a parser and then uses the AST (Abstract Syntax Tree) to find and extract translatable strings from your source code. All extracted strings can then be saved as `.pot` file to act as template for translation files.

Unlike many of the alternatives, this library is highly configurable and is designed to work with most existing setups.

For the full documentation check out the [Github Wiki](https://github.com/lukasgeiter/gettext-extractor/wiki) of the original project.

## Notes

This is a fork of the [original program](https://github.com/lukasgeiter/gettext-extractor) written by [Lukas Geiter](https://lukasgeiter.com/). As per some posts in the issues of the original repository, Lukas was in the middle of a full rewrite of the program, which has not materialized yet. However, a useful patch to use regex was [contributed](https://github.com/lukasgeiter/gettext-extractor/pull/44) but it was never merged. Hence the fork.

## Installation

> **Note:** This package requires Node.js version 6 or higher.

#### Yarn

```text
yarn add @rgglez/gettext-extractor
```

#### NPM

```text
npm install @rgglez/gettext-extractor
```

## Getting Started

In order to execute the extraction, you should write and run a simple script. Some a example on how to use the different extractors follow:

```javascript
const { GettextExtractor, JsExtractors, HtmlExtractors } = require('@rgglez/gettext-extractor');

let extractor = new GettextExtractor();

extractor
    .createJsParser([
        JsExtractors.callExpression('getText', {
            arguments: {
                text: 0,
                context: 1
            }
        }),
        JsExtractors.callExpression('getPlural', {
            arguments: {
                text: 1,
                textPlural: 2,
                context: 3
            }
        })
    ])
    .parseFilesGlob('./src/**/*.@(ts|js|tsx|jsx)');

extractor
    .createHtmlParser([
        HtmlExtractors.elementContent('translate, [translate]')
    ])
    .parseFilesGlob('./src/**/*.html');
```

Using regex to extract from an Svelte app where a store like ```$_("Hello world")``` is being used:

```javascript
const { GettextExtractor, JsExtractors, RegexExtractors } = require('@rgglez/gettext-extractor');

let extractor = new GettextExtractor();

extractor
    .createRegexParser([
        RegexExtractors.addCondition({
            regex: /\$_\((["'`])((?:\\.|(?!\1).)*?)\1\)/,
            text: 2
        })
    ])
    .parseFilesGlob('./src/**/*.@(js|svelte)');

extractor.savePotFile('./messages.pot');

extractor.printStats();
```

## License

Copyright (c) 2017 Lukas Geiter.<br>
Copyright (c) 2024 Rodolfo González González.

[MIT license](https://opensource.org/license/mit). Please read the [LICENSE](https://raw.githubusercontent.com/rgglez/gettext-extractor/main/LICENSE) file.

