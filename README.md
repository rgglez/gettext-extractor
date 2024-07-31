# gettext-extractor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) 
![GitHub all releases](https://img.shields.io/github/downloads/rgglez/gettext-extractor/total)
![GitHub issues](https://img.shields.io/github/issues/rgglez/gettext-extractor)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/rgglez/gettext-extractor)

A flexible and powerful [Gettext](https://www.gnu.org/software/gettext/) message extractor with support for JavaScript, TypeScript, JSX and HTML, and others via  [regular expressions](https://en.wikipedia.org/wiki/Regular_expression).

This is a fork of the [original program](https://github.com/lukasgeiter/gettext-extractor) written by [Lukas Geiter](https://lukasgeiter.com/). As per some posts in the issues of the original repository, Lukas was in the middle of a full rewrite of the program, which has not materialized yet. However, a useful patch to use regex was [contributed](https://github.com/lukasgeiter/gettext-extractor/pull/44) but it was never merged. Hence the fork.

This program works by running your files through a parser and then uses the AST (Abstract Syntax Tree) to find and extract translatable strings from your source code. All extracted strings can then be saved as `.pot` file to act as template for translation files.

Unlike many of the alternatives, this library is highly configurable and is designed to work with most existing setups.

For the full documentation check out the [Github Wiki](https://github.com/lukasgeiter/gettext-extractor/wiki) of the original project.

## Installation

> **Note:** This package requires Node.js version 6 or higher.

#### Yarn

```text
yarn add gettext-extractor
```

#### NPM

```text
npm install gettext-extractor
```

## Getting Started

Let's start with a code example:

```javascript
const { GettextExtractor, JsExtractors, HtmlExtractors, RegexExtractors } = require('gettext-extractor');

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

extractor
    .createRegexParser([
        RegexExtractors.addCondition({
            regex: /\_translate\((.*)\)/i,
            text: 1
        })
    ])
    .parseFilesGlob('./src/**/*.tmpl');

extractor.savePotFile('./messages.pot');

extractor.printStats();
```

## Contributing

From reporting a bug to submitting a pull request: every contribution is appreciated and welcome.
Report bugs, ask questions and request features using [Github issues][github-issues].
If you want to contribute to the code of this project, please read the [Contribution Guidelines][contributing].

## License

Copyright (c) 2017 Lukas Geiter.
Copyright (c) 2024 Rodolfo González González.

Published under the [MIT license](https://opensource.org/license/mit). Please read the [LICENSE](https://raw.githubusercontent.com/rgglez/gettext-extractor/main/LICENSE) file included in the source code.

