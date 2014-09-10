[![Build Status](https://travis-ci.org/fisharebest/ext-calendar.svg?branch=master)](https://travis-ci.org/fisharebest/ext-calendar)
[![Coverage Status](https://coveralls.io/repos/fisharebest/ext-calendar/badge.png)](https://coveralls.io/r/fisharebest/ext-calendar)
[![SensioLabsInsight](https://insight.sensiolabs.com/projects/952d6e11-6941-447b-9757-fc8dbc3d2a1f/mini.png)](https://insight.sensiolabs.com/projects/952d6e11-6941-447b-9757-fc8dbc3d2a1f)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/fisharebest/ext-calendar/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/fisharebest/ext-calendar/?branch=master)
[![Code Climate](https://codeclimate.com/github/fisharebest/ext-calendar/badges/gpa.svg)](https://codeclimate.com/github/fisharebest/ext-calendar)

PHP calendar functions
======================

This package provides an implementation of the
[Gregorian](https://en.wikipedia.org/wiki/Gregorian_calendar),
[Julian](https://en.wikipedia.org/wiki/Julian_calendar),
[French Republican](https://en.wikipedia.org/wiki/French_Republican_Calendar) and
[Jewish calendars](https://en.wikipedia.org/wiki/Hebrew_calendar) calendars, plus
[shims](https://en.wikipedia.org/wiki/Shim_%28computing%29) for the
[functions](https://php.net/ref.calendar) and [constants](https://php.net/calendar.constants)
in PHP‘s [ext/calendar](https://php.net/calendar) extension.
It allows you to use these functions on servers that do not have the extension installed.

* [cal_days_in_month()](https://php.net/cal_days_in_month)
* [cal_from_jd()](https://php.net/cal_from_jd)
* [cal_info()](https://php.net/cal_info)
* [cal_to_jd()](https://php.net/cal_to_jd)
* [easter_date()](https://php.net/easter_date)
* [easter_days()](https://php.net/easter_days)
* [FrenchToJD()](https://php.net/FrenchToJD)
* [GregorianToJD()](https://php.net/GregorianToJD)
* [JDDayOfWeek()](https://php.net/JDDayOfWeek)
* [JDMonthName()](https://php.net/JDMonthName)
* [JDToFrench()](https://php.net/JDToFrench)
* [JDToGregorian()](https://php.net/JDToGregorian)
* [jdtojewish()](https://php.net/jdtojewish)
* [JDToJulian()](https://php.net/JDToJulian)
* [jdtounix()](https://php.net/jdtounix)
* [JewishToJD()](https://php.net/JewishToJD)
* [JulianToJD()](https://php.net/JulianToJD)
* [unixtojd()](https://php.net/unixtojd)

How to use it
=============

Add the package as a dependency in your `composer.json` file:

    require {
        "fisharebest/ext-calendar": "1.*"
    }

Bootstrap the package:

    // bootstrap.php
    require 'vendor/autoload.php';
    \Fisharebest\ExtCalendar\Bootstrap::init();

Use the PHP functions, whether the extension is loaded or not:

    print_r(cal_info(CAL_GREGORIAN));

Use the calendar classes directly:

    $cal = new JewishCalendar;
    $jd = $cal->ymdToJd($year, $month, $day);
    list($year, $month, $day) = $cal->jdToYmd($jd);
    $is_leap_year = $cal->leapYear($year);
    $day_of_week = $cal->dayOfWeek($jd);
    $month_length = $cal->daysInMonth($year, $month);
    // See the source for more…

Known restrictions and limitations
==================================

When faced with invalid inputs, the shim functions trigger `E_USER_WARNING` instead of `E_WARNING`.  The text of the error messages is the same.

The functions `easterdate()` and `jdtounixtime()` use PHP‘s timezone, instead of the operating system‘s timezone.  These may be different.

Compatibility with different versions of PHP
============================================

The following bugs are emulated, according to the version of PHP being used.
Thus the package always provides the same behaviour as the native `ext/calendar` extension.

* [#54254](https://bugs.php.net/bug.php?id=54254) Jewish month "Adar" - fixed in PHP 5.5

* [#67960](https://bugs.php.net/bug.php?id=67960) Constants `CAL_DOW_SHORT` and `CAL_DOW_LONG` - found/fixed by this project!

* [#67976](https://bugs.php.net/bug.php?id=67976) Wrong value in `cal_days_in_month()` for French calendar - found by this project!

Performance
===========

These functions are naturally slower than the native functions, especially for complex calendars such as Jewish.  This table shows the relative decrease in performance.

    JDToFrench()                     FrenchCalendar->jdToYmd()         3.7
    JDToGregorian()                  GregorianCalendar->jdToYmd()      5.9
    JDToJewish()                     JewishCalendar->jdToYmd()        23.7
    JDToJulian()                     JulianCalendar->jdToYmd()         4.8
    FrenchToJD()                     FrenchCalendar->ymdToJd()         2.0
    GregorianToJD()                  GregorianCalendar->ymdToJd()      3.0
    jewishtojd()                     JewishCalendar->ymdToJd()        22.1
    JulianToJD()                     JulianCalendar->ymdToJd()         3.5
    cal_days_in_month(CAL_FRENCH)    FrenchCalendar->daysInMonth()     2.7
    cal_days_in_month(CAL_GREGORIAN) GregorianCalendar->daysInMonth()  2.6
    cal_days_in_month(CAL_JEWISH)    JewishCalendar->daysInMonth()    12.2
    cal_days_in_month(CAL_JULIAN)    JulianCalendar->daysInMonth()     3.0

HHVM
====

The package should work on [HHVM](http://hhvm.com/).  However, we can’t run the unit
tests on it, as HHVM does not (yet?) include the `ext/calendar` extension.

Development and contributions
=============================

Note that `require-dev` will require `ext-calendar`, as we use it as a reference
implementation for testing.

Due to the known restrictions above, you may need to run unit tests using `TZ=UTC phpunit`.

Pull requests are welcome.  Please ensure you include unit-tests where
applicable, and follow the existing coding conventions.  These are to follow
[PSR](http://www.php-fig.org/) standards, except for:

* tabs are used for indentation
* opening braces always go on the end of the previous line

History
=======

These functions were originally written for the [webtrees](http://www.webtrees.net)
project.  As part of a refactoring process, they were extracted to a standalone
library, given version numbers, unit tests, etc.

Future plans
============

It is hoped to add support for the Arabic (Jalali) and Persian (Shamsi) calendars.

It is hoped to extend the French calendar to allow it to be used for modern times.