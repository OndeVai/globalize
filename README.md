# Globalize

[![Build Status](https://secure.travis-ci.org/jquery/globalize.png)](http://travis-ci.org/jquery/globalize)
[![devDependency Status](https://david-dm.org/jquery/globalize/dev-status.png)](https://david-dm.org/jquery/globalize#info=devDependencies)


A JavaScript library for globalization and localization that leverage the
official [Unicode CLDR](http://cldr.unicode.org/) JSON data. Run in browsers and
node.js.


----

## Heads Up!

We're working the migration to CLDR. This is an alpha version of Globalize: 1.0.0-pre.

Patches to the previous stable codebase probably can't be landed. If you have a
problem, please create an issue first before trying to patch it.

----

- [Getting Started](#getting_started)
  - [Why Globalization](#why)
  - [What about Globalize?](#what)
    - [Where to use it?](#where)
    - [Where does I18n data come from?](#cldr)
    - [Only load and use what you need](#modules)
    - [Browser support](#where)
- [API](#api)
  - [Core](#core)
    - [Globalize.load](#load)
    - [Globalize.locale](#locale)
  - [Date module](#date)
    - [Globalize.format](#format)
    - [Globalize.parseDate](#parse_date)
  - [Translate module](#translate_module)
    - [Globalize.loadTranslation](#load_translations)
    - [Globalize.translate](#translate)
  - more to come...


<a name="getting_started"></a>
## Getting Started

<a name="why"></a>
### Why Globalization?

Each language, and the countries that speak that language, have different
expectations when it comes to how numbers (including currency and percentages)
and dates should appear. Obviously, each language has different names for the
days of the week and the months of the year. But they also have different
expectations for the structure of dates, such as what order the day, month and
year are in. In number formatting, not only does the character used to
delineate number groupings and the decimal portion differ, but the placement of
those characters differ as well.

A user using an application should be able to read and write dates and numbers
in the format they are accustomed to. This library makes this possible,
providing an API to convert user-entered number and date strings - in their
own format - into actual numbers and dates, and conversely, to format numbers
and dates into that string format.


<a name="what"></a>
### What about Globalize?

<a name="where"></a>
#### Where to use it?

It's designed to work both in the [browser](#browser_support), or in
[node.js](#commonjs). It supports [AMD](#amd), and [CommonJs](#commonjs);

<a name="cldr"></a>
#### Where does I18n data come from?

Globalize uses the [Unicode CLDR](http://cldr.unicode.org/), the largest and
most extensive standard repository of locale data.

We do NOT embed any I18n data within our library. Although, we make it realy
easy to use. Read below [How to get and load CLDR JSON data](#cldr_usage) for
more information on its usage.

<a name="modules"></a>
#### Load and use only what you need

Globalize is split in modules: core, number (coming soon), date, and translate.
We're evaluating other modules, eg. plural, ordinals, etc.

The core implements [`Globalize.load( cldrData )`](#load), and
[`Globalize.locale( locale )`](#locale).

The date module extends core Globalize, and adds [`Globalize.format( value,
pattern, locale )`](#format), and [`Globalize.parseDate( value, patterns, locale
)`](#parse_date).

The translate module extends core Globalize, and adds
[`Globalize.loadTranslations( locale, json )`](#load_translations), and
[`Globalize.translate( path , locale )`](#translate).

More to come...

<a name="browser_support"></a>
#### Browser Support

We officially support:
 - Firefox (latest - 1)+
 - Chrome (latest - 1)+
 - Safari 5.1+
 - IE 8+
 - Opera (latest - 1)+

Dry tests show Globalize also works on the following browsers:

- Firefox 4+
- Safari 5+
- Chrome 14+
- IE 6+
- Opera 11.1+

If you find any bugs, please just [let us
know](https://github.com/jquery/globalize/issues). We'll be glad to fix them for
the officially supported browsers, or at least update the documentation for the
unsupported ones.


<a name="usage"></a>
## Usage

All distributables are UMD wrapped. So, it supports AMD, CommonJS, or global variables (in case AMD or CommonJS have not been detected).

### Script tags

Example loading with script tags:
```html
<script src="./external/cldr/dist/cldr.js"></script>
<script src="./dist/globalize.js"></script>
<script src="./dist/globalize.date.js"></script>
```
Note that Globalize's Download Builder will eventually embed CLDR, so no dependency is required. We can also distribute it on CDNs embedded this way.


<a name="amd"></a>
### AMD
Example loading with AMD:
```javascript
require.config({
  paths: {
    cldr: "../external/cldr/dist/cldr.runtime"
  }
});
require( [ "./dist/globalize", "./dist/globalize.date" ], function( Globalize ) {
  ...
});
```

<a name="commonjs"></a>
Example loading with node.js:
```javascript
var Globalize = require( "./dist/globalize.date" );
...
```

<a name="api"></a>
## API


<a name="core"></a>
### Core module

<a name="load"></a>
#### `Globalize.load( cldrJSONData )`

This method allows you to load CLDR JSON locale data.

[More Content TBD]

<a name="locale"></a>
#### `Globalize.locale( locale )`

An application that supports globalization and/or localization will need to
have a way to determine the user's preference. Attempting to automatically
determine the appropriate culture is useful, but it is good practice to always
offer the user a choice, by whatever means.

Whatever your mechanism, it is likely that you will have to correlate the
user's preferences with the list of locale data supported in the app. This
method allows you to select the best match given the locale data that you
have included and to set the Globalize locale to the one which the user
prefers.

```javascript
Globalize.locale( "pt" );
console.log( Globalize.culture().attributes );
// {
//    "languageId": "pt",
//    "maxLanguageId": "pt_Latn_BR",
//    "language": "pt",
//    "script": "Latn",
//    "territory": "BR",
//    "region": "BR"
// }

Globalize.locale( "pt_PT" );
console.log( Globalize.culture().attributes );
// {
//    "languageId": "pt_PT",
//    "maxLanguageId": "pt_Latn_PT",
//    "language": "pt",
//    "script": "Latn",
//    "territory": "PT",
//    "region": "PT"
// }
```

LanguageMatching TBD (CLDR's spec http://www.unicode.org/reports/tr35/#LanguageMatching).


### Date module

<a name="format"></a>
#### `Globalize.format( value, format, [locale] )`

Format a date according to the given format string and the given locale (or the
current locale if not specified). See the section <a href="#dates">Date
Formatting</a> below for details on the available formats. See other modules,
eg. number module, for different overloads of Globalize.format().

Parameters:
- **value**
  - Date instance to be formatted, eg. `new Date()`; or
  - Number (TBD);
- **format** (if value is Date)
  - String, skeleton. Eg "GyMMMd";
  - Object, accepts either one:
    - Skeleton, eg. `{ skeleton: "GyMMMd" }`. List of all skeletons [TODO];
    - Date, eg. `{ date: "full" }`. Possible values are full, long, medium, short;
    - Time, eg. `{ time: "full" }`. Possible values are full, long, medium, short;
    - Datetime, eg. `{ datetime: "full" }`. Possible values are full, long, medium, short;
    - Raw pattern, eg. `{ pattern: "dd/mm" }`. [List of all date
      patterns](http://www.unicode.org/reports/tr35/tr35-dates.html#Date_Field_Symbol_Table);
- **locale** Optional String with locale that overrides default;

```javascript
Globalize.format( new Date( 2010, 10, 30, 17, 55 ), { datetime: "short" } );
// "11/30/10, 5:55 PM"

Globalize.format( new Date( 2010, 10, 30, 17, 55 ), { datetime: "short" }, "de" );
// "30.11.10 17:55"
```

Comparison between different locales.

| locale | `Globalize.format( new Date( 2010, 10, 1, 17, 55 ), { datetime: "short" }` |
| --- | --- |
| **en** | `"11/1/10, 5:55 PM"` |
| **en_GB** | `"01/11/2010 17:55"` |
| **de** | `"01.11.10 17:55"` |
| **zh** | `"10/11/1 下午5:55"` |
| **ar** | `"1‏/11‏/2010 5:55 م"` |
| **pt** | `"01/11/10 17:55"` |
| **es** | `"1/11/10 17:55"` |

<a name="parse_date"></a>
#### `Globalize.parseDate( value, [formats], [locale] )`

Parse a string representing a date into a JavaScript Date object, taking into
account the given possible formats (or the given locale's set of preset
formats if not given). As before, the current locale is used if one is not
specified.

Parameters:
- **value** String with date to be parsed, eg. `"11/1/10, 5:55 PM"`;
- **formats** Optional Array of formats;
- **locale** Optional String with locale that overrides default;

```javascript
Globalize.culture( "en" );
Globalize.parseDate( "1/2/13" );
// Wed Jan 02 2013 00:00:00

Globalize.culture( "es" );
Globalize.parseDate( "1/2/13" );
// Fri Feb 01 2013 00:00:00
``


<a name="translate_module"></a>
### Translate module

<a name="load_translations"></a>
#### `Globalize.loadTranslation( locale, translationData )`

<a name="translate"></a>
#### `Globalize.translate( path, [locale] )`


<a name="cldr_usage"></a>
## How to get and load CLDR JSON data

TBD


## Development

### File structure:
```
├── bower.json (metadata file)
├── CONTRIBUTING.md (doc file)
├── dist/ (output of built bundles)
├── external/ (external dependencies, eg. cldr, qunit, requirejs)
├── Gruntfile.js (grunt tasks)
├── LICENSE (license file)
├── package.json (metadata file)
├── README.md (doc file)
├── src/ (source code)
│   ├── build/ (build helpers, eg. intro, and outro)
│   ├── common/ (common function helpers across modules)
│   ├── core.js (core module)
│   ├── date/ (date source code)
│   ├── date.js (date module)
│   ├── translate.js (translate module)
│   └── util/ (basic javascript helpers polyfills, eg array.map)
└── test/ (unit and functional test files)
    ├── fixtures/ (cldr fixture data)
    ├── functional/ (functional tests)
    ├── functional.html
    ├── functional.js
    ├── unit/ (unit tests)
    ├── unit.html
    └── unit.js
```

### Source files

The source files are as granular as possible. Although, when combined to
generate the build file, all the excessive/overhead wrappers are cut off. It's
following the same build model of jQuery, and modernizr.

Core, and all modules' public APIs are located on `src/` root: eg. `core.js`,
`date.js`, and `translate.js`.

### Build

Install grunt and tests external dependencies. First, install the
[grunt-cli](http://gruntjs.com/getting-started#installing-the-cli) and
[bower](http://bower.io/) packages if you haven't before. These should be done
as global installs. Then:

```bash
npm install
grunt
```

### Tests

Tests can be run either on browser or node (via grunt).

***Unit***

To run the unit tests, run `grunt test:unit`, or browse at
`file:///.../globalize/test/unit.html`. It tests the very specific functionality
of each function (sometimes internal/private).

The goal of the unit tests is to make it easy to spot bugs, easy to debug.

***Functional***

To run the functional tests, create the dist files by running `grunt`. Then, run
`grunt test:functional`, or browse at
`file:///.../globalize/test/functional.html`. Note that `grunt` will
automatically run unit and functional tests for you to ensure the built files
are safe.

The goal of the functional tests is to ensure the combining pieces work as expected.

