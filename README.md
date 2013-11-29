# Globalize

[![Build Status](https://secure.travis-ci.org/jquery/globalize.png)](http://travis-ci.org/jquery/globalize)
[![devDependency Status](https://david-dm.org/jquery/globalize/dev-status.png)](https://david-dm.org/jquery/globalize#info=devDependencies)


A JavaScript library for globalization and localization. Enables complex
culture-aware number (WIP), currency (WIP) and date parsing and formatting that
leverage the official CLDR JSON data. Run in browsers and node.js.


## Heads Up!

We're working the migration to CLDR. This is an alpha version of Globalize: 1.0.0-pre.

While that is in progress, patches to the previous stable codebase probably can't be landed. If you have a problem, please create an issue first before trying to patch it.

----

FIXME Update TOC

- [Why Globalization](#why)
- [What is a CLDR?](#cldr)
- [API](#api)
  - [Globalize.load](#load)
  - [Globalize.locale](#locale)
  - [Globalize.format](#format)


<a name="why"></a>
## Why Globalization?

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


## CLDR

Globalize uses the [Unicode CLDR](http://cldr.unicode.org/), the largest and
most extensive standard repository of locale data.

[More Content TBD]


## API

<a name="load"></a>
### `Globalize.load( cldrJSONData )`

This method allows you to load CLDR JSON locale data.

[More Content TBD]


<a name="locale"></a>
### `Globalize.locale( locale )`

An application that supports globalization and/or localization will need to
have a way to determine the user's preference. Attempting to automatically
determine the appropriate culture is useful, but it is good practice to always
offer the user a choice, by whatever means.

Whatever your mechanism, it is likely that you will have to correlate the
user's preferences with the list of locale data supported in the app. This
method allows you to select the best match given the locale data that you
have included and to set the Globalize locale to the one which the user
prefers.

```js
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
// {
//    "languageId": "pt_PT",
//    "maxLanguageId": "pt_Latn_PT",
//    "language": "pt",
//    "script": "Latn",
//    "territory": "PT",
//    "region": "PT"
// }
```

[FIXME CLDR has a spec for this algorithm http://www.unicode.org/reports/tr35/#LanguageMatching]


<a name="format"></a>
### `Globalize.format( value, format, locale )`

Formats a date or number according to the given format string and the given
locale (or the current locale if not specified). See the sections
<a href="#numbers">Number Formatting</a> and
<a href="#dates">Date Formatting</a> below for details on the available
formats.

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
    - Raw pattern, eg. `{ pattern: "dd/mm" }`. List of date patterns [TODO];
- **format** (if value is Number)
  - TDB
- **locale** String with locale, eg. `"en"`.

```js
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


<a name="parseInt"></a>
### `Globalize.parseInt( value, radix, culture )` FIXME

Parses a string representing a whole number in the given radix (10 by default),
taking into account any formatting rules followed by the given culture (or the
current culture, if not specified).

If a percentage is passed into parseInt, the percent sign will be removed and the number parsed as is.
Example: 12.34% would be returned as 12.
```js
// assuming a culture where "," is the group separator
// and "." is the decimal separator
Globalize.parseInt( "1,234.56" ); // 1234
// assuming a culture where "." is the group separator
// and "," is the decimal separator
Globalize.parseInt( "1.234,56" ); // 1234
```


<a name="parseFloat"></a>
### `Globalize.parseFloat( value, radix, culture )` FIXME

Parses a string representing a floating point number in the given radix (10 by
default), taking into account any formatting rules followed by the given
culture (or the current culture, if not specified).

If a percentage is passed into parseFloat, the percent sign will be removed and the number parsed as is.
Example: 12.34% would be returned as 12.34
```js
// assuming a culture where "," is the group separator
// and "." is the decimal separator
Globalize.parseFloat( "1,234.56" ); // 1234.56
// assuming a culture where "." is the group separator
// and "," is the decimal separator
Globalize.parseFloat( "1.234,56" ); // 1234.56
```


<a name="parseDate"></a>
### `Globalize.parseDate( value, formats, locale )`

Parses a string representing a date into a JavaScript Date object, taking into
account the given possible formats (or the given locale's set of preset
formats if not given). As before, the current locale is used if one is not
specified.

Parameters:
- **value** String with date to be parsed, eg. `"11/1/10, 5:55 PM"`.
- **formats**
- **locale** String with locale, eg. `"en"`.

```js
Globalize.culture( "en" );
Globalize.parseDate( "1/2/13" );
// Wed Jan 02 2013 00:00:00

Globalize.culture( "es" );
Globalize.parseDate( "1/2/13" );
// Fri Feb 01 2013 00:00:00
``


<a name="cldr-usage"></a>
## How to load and use CLDR JSON data

TBD


## Formatting

<a name="numbers"></a>
### Number Formatting FIXME

When formatting a number with format(), the main purpose is to convert the
number into a human readable string using the culture's standard grouping and
decimal rules. The rules between cultures can vary a lot. For example, in some
cultures, the grouping of numbers is done unevenly. In the "te-IN" culture
(Telugu in India), groups have 3 digits and then 2 digits. The number 1000000
(one million) is written as "10,00,000". Some cultures do not group numbers at
all.

There are four main types of number formatting:

- **n** for number
- **d** for decimal digits
- **p** for percentage
- **c** for currency

Even within the same culture, the formatting rules can vary between these four
types of numbers. For example, the expected number of decimal places may differ
from the number format to the currency format. Each format token may also be
followed by a number. The number determines how many decimal places to display
for all the format types except decimal, for which it means the minimum number
of digits to display, zero padding it if necessary. Also note that the way
negative numbers are represented in each culture can vary, such as what the
negative sign is, and whether the negative sign appears before or after the
number. This is especially apparent with currency formatting, where many
cultures use parentheses instead of a negative sign.
```js
// just for example - will vary by culture
Globalize.format( 123.45, "n" ); // 123.45
Globalize.format( 123.45, "n0" ); // 123
Globalize.format( 123.45, "n1" ); // 123.5

Globalize.format( 123.45, "d" ); // 123
Globalize.format( 12, "d3" ); // 012

Globalize.format( 123.45, "c" ); // $123.45
Globalize.format( 123.45, "c0" ); // $123
Globalize.format( 123.45, "c1" ); // $123.5
Globalize.format( -123.45, "c" ); // ($123.45)

Globalize.format( 0.12345, "p" ); // 12.35 %
Globalize.format( 0.12345, "p0" ); // 12 %
Globalize.format( 0.12345, "p4" ); // 12.3450 %
```
Parsing with parseInt and parseFloat also accepts any of these formats.


<a name="currency"></a>
### Currency Formatting FIXME

Globalize has a default currency symbol for each locale. This is used when
formatting a currency value such as
```js
Globalize.format( 1234.56, "c" ); // $1,234.56
```
You can change the currency symbol for a locale by modifying the culture's
<code>numberFormat.currency.symbol</code> property:
```js
Globalize.culture( "en-US" ).numberFormat.currency.symbol = '\u20ac'; // euro sign U+20AC
```
If you need to switch between currency symbols, you could write a function
to do that, such as
```js
function setCurrency( currSym ) {
  Globalize.culture().numberFormat.currency.symbol = currSym;
}
```

<a name="dates"></a>
### Date Formatting FIXME

Date formatting varies wildly by culture, not just in the spelling of month and
day names, and the date separator, but by the expected order of the various
date components, whether to use a 12 or 24 hour clock, and how months and days
are abbreviated. Many cultures even include "genitive" month names, which are
different from the typical names and are used only in certain cases.

Also, each culture has a set of "standard" or "typical" formats. For example,
in "en-US", when displaying a date in its fullest form, it looks like
"Saturday, November 05, 1955". Note the non-abbreviated day and month name, the
zero padded date, and four digit year. So, Globalize expects a certain set
of "standard" formatting strings for dates in the "patterns" property of the
"standard" calendar of each culture, that describe specific formats for the
culture. The third column shows example values in the neutral English culture
"en-US"; see the second table for the meaning tokens used in date formats.

```js
// just for example - will vary by culture
Globalize.format( new Date(2012, 1, 20), 'd' ); // 2/20/2012
Globalize.format( new Date(2012, 1, 20), 'D' ); // Monday, February 20, 2012
```



<table>
<tr>
  <th>Format</th>
  <th>Meaning</th>
  <th>"en-US"</th>
</tr>
<tr>
   <td>f</td>
   <td>Long Date, Short Time</td>
   <td>dddd, MMMM dd, yyyy h:mm tt</td>
</tr>
<tr>
   <td>F</td>
   <td>Long Date, Long Time</td>
   <td>dddd, MMMM dd, yyyy h:mm:ss tt</td>
</tr>
<tr>
   <td>t</td>
   <td>Short Time</td>
   <td>h:mm tt</td>
</tr>
<tr>
   <td>T</td>
   <td>Long Time</td>
   <td>h:mm:ss tt</td>
</tr>
<tr>
   <td>d</td>
   <td>Short Date</td>
   <td>M/d/yyyy</td>
</tr>
<tr>
   <td>D</td>
   <td>Long Date</td>
   <td>dddd, MMMM dd, yyyy</td>
</tr>
<tr>
   <td>Y</td>
   <td>Month/Year</td>
   <td>MMMM, yyyy</td>
</tr>
<tr>
   <td>M</td>
   <td>Month/Day</td>
   <td>MMMM dd</td>
</tr>
</table>

In addition to these standard formats, there is the "S" format. This is a
sortable format that is identical in every culture:
"**yyyy'-'MM'-'dd'T'HH':'mm':'ss**".

When more specific control is needed over the formatting, you may use any
format you wish by specifying the following custom tokens:
<table>
<tr>
   <th>Token</th>
   <th>Meaning</th>
   <th>Example</th>
</tr>
<tr>
   <td>d</td>
   <td>Day of month (no leading zero)</td>
   <td>5</td>
</tr>
<tr>
   <td>dd</td>
   <td>Day of month (leading zero)</td>
   <td>05</td>
</tr>
<tr>
   <td>ddd</td>
   <td>Day name (abbreviated)</td>
   <td>Sat</td>
</tr>
<tr>
   <td>dddd</td>
   <td>Day name (full)</td>
   <td>Saturday</td>
</tr>
<tr>
   <td>M</td>
   <td>Month of year (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>MM</td>
   <td>Month of year (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>MMM</td>
   <td>Month name (abbreviated)</td>
   <td>Sep</td>
</tr>
<tr>
   <td>MMMM</td>
   <td>Month name (full)</td>
   <td>September</td>
</tr>
<tr>
   <td>yy</td>
   <td>Year (two digits)</td>
   <td>55</td>
</tr>
<tr>
   <td>yyyy</td>
   <td>Year (four digits)</td>
   <td>1955</td>
</tr>
<tr>
   <td>'literal'</td>
   <td>Literal Text</td>
   <td>'of the clock'</td>
</tr>
<tr>
   <td>\'</td>
   <td>Single Quote</td>
   <td>'o'\''clock'</td><!-- o'clock -->
</tr>
<tr>
   <td>m</td>
   <td>Minutes (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>mm</td>
   <td>Minutes (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>h</td>
   <td>Hours (12 hour time, no leading zero)</td>
   <td>6</td>
</tr>
<tr>
   <td>hh</td>
   <td>Hours (12 hour time, leading zero)</td>
   <td>06</td>
</tr>
<tr>
   <td>H</td>
   <td>Hours (24 hour time, no leading zero)</td>
   <td>5 (5am) 15 (3pm)</td>
</tr>
<tr>
   <td>HH</td>
   <td>Hours (24 hour time, leading zero)</td>
   <td>05 (5am) 15 (3pm)</td>
</tr>
<tr>
   <td>s</td>
   <td>Seconds (no leading zero)</td>
   <td>9</td>
</tr>
<tr>
   <td>ss</td>
   <td>Seconds (leading zero)</td>
   <td>09</td>
</tr>
<tr>
   <td>f</td>
   <td>Deciseconds</td>
   <td>1</td>
</tr>
<tr>
   <td>ff</td>
   <td>Centiseconds</td>
   <td>11</td>
</tr>
<tr>
   <td>fff</td>
   <td>Milliseconds</td>
   <td>111</td>
</tr>
<tr>
   <td>t</td>
   <td>AM/PM indicator (first letter)</td>
   <td>A or P</td>
</tr>
<tr>
   <td>tt</td>
   <td>AM/PM indicator (full)</td>
   <td>AM or PM</td>
</tr>
<tr>
   <td>z</td>
   <td>Timezone offset (hours only, no leading zero)</td>
   <td>-8</td>
</tr>
<tr>
   <td>zz</td>
   <td>Timezone offset (hours only, leading zero)</td>
   <td>-08</td>
</tr>
<tr>
   <td>zzz</td>
   <td>Timezone offset (full hours/minutes)</td>
   <td>-08:00</td>
</tr>
<tr>
   <td>g or gg</td>
   <td>Era name</td>
   <td>A.D.</td>
</tr>
</table>


