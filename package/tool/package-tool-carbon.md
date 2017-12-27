# Carbon 時間套件

Carbon 是一個很方便的轉換時間的工具，可以很方便地將時間進行轉換，取得我們想要的特定日期或格式

## 安裝

Laravel 5 預設就會安裝 Carbon 套件，若沒有安裝的話可以透過下列的方式進行安裝：


```shell
# 使用 Composer 下載 Carbon
$ composer require nesbot/carbon
```

```php
<?php
// 載入 composer autoload 檔案
require 'vendor/autoload.php';

// 使用 Carbon 類別
use Carbon\Carbon;

printf("Now: %s", Carbon::now());
```

## 快速切換前後日期

```php
<?php

use Carbon\Carbon;

$now = Carbon::now();
echo $now;                               // 2015-03-26 00:36:47
$today = Carbon::today();
echo $today;                             // 2015-03-26 00:00:00
$tomorrow = Carbon::tomorrow('Europe/London');
echo $tomorrow;                          // 2015-03-27 00:00:00
$yesterday = Carbon::yesterday();
echo $yesterday;                         // 2015-03-25 00:00:00
```

## 建立特定日期的時間

```php
<?php

use Carbon\Carbon;

$timezone = 'Asia/Taipei';

// 從「年月日」建立
Carbon::createFromDate($year, $month, $day, $timezone);

// 從「時分秒」建立
Carbon::createFromTime($hour, $minute, $second, $timezone);

// 從完整的「年月日時分秒」建立
Carbon::create($year, $month, $day, $hour, $minute, $second, $timezone);

// 從指定的格式建立
Carbon::createFromFormat($format, $time, $tz);
echo Carbon::createFromFormat('Y-m-d H', '1975-05-21 22')->toDateTimeString(); // 1975-05-21 22:00:00

// 從時間戳記建立
echo Carbon::createFromTimeStamp(-1)->toDateTimeString();                        // 1969-12-31 18:59:59
echo Carbon::createFromTimeStamp(-1, 'Europe/London')->toDateTimeString();       // 1970-01-01 00:59:59
echo Carbon::createFromTimeStampUTC(-1)->toDateTimeString();                     // 1969-12-31 23:59:59
```

## 轉換日期

```php
<?php

use Carbon\Carbon;

// 透過文字移動日期
$knownDate = Carbon::create(2001, 5, 21, 12);          // create testing date
Carbon::setTestNow($knownDate);                        // set the mock
echo new Carbon('tomorrow');                           // 2001-05-22 00:00:00  ... notice the time !
echo new Carbon('yesterday');                          // 2001-05-20 00:00:00
echo new Carbon('next wednesday');                     // 2001-05-23 00:00:00
echo new Carbon('last friday');                        // 2001-05-18 00:00:00
echo new Carbon('this thursday');                      // 2001-05-24 00:00:00
```

## 取得日期資料

```php
<?php

use Carbon\Carbon;

$dt = Carbon::parse('2012-9-5 23:26:11.123789');

// 取的指定時間資料的資訊（整數）
var_dump($dt->year);                                         // int(2012)
var_dump($dt->month);                                        // int(9)
var_dump($dt->day);                                          // int(5)
var_dump($dt->hour);                                         // int(23)
var_dump($dt->minute);                                       // int(26)
var_dump($dt->second);                                       // int(11)
var_dump($dt->micro);                                        // int(123789)
var_dump($dt->dayOfWeek);                                    // int(3)
var_dump($dt->dayOfYear);                                    // int(248)
var_dump($dt->weekOfMonth);                                  // int(1)
var_dump($dt->weekOfYear);                                   // int(36)
var_dump($dt->daysInMonth);                                  // int(30)
var_dump($dt->timestamp);                                    // int(1346901971)
var_dump(Carbon::createFromDate(1975, 5, 21)->age);          // int(39) calculated vs now in the same tz
var_dump($dt->quarter);                                      // int(3)


// 回傳與 UTC 差異的秒數
var_dump(Carbon::createFromTimestampUTC(0)->offset);         // int(0)
var_dump(Carbon::createFromTimestamp(0)->offset);            // int(-18000)

// 回傳與 UTC 差異的時數
var_dump(Carbon::createFromTimestamp(0)->offsetHours);       // int(-5)

// 找出當天日否有日光節約時間
var_dump(Carbon::createFromDate(2012, 1, 1)->dst);           // bool(false)
var_dump(Carbon::createFromDate(2012, 9, 1)->dst);           // bool(true)

// 判斷指定的的時區是否與預設的時區相同
var_dump(Carbon::now()->local);                              // bool(true)
var_dump(Carbon::now('America/Vancouver')->local);           // bool(false)

// 判斷是否為 UTC 的時區時間
var_dump(Carbon::now()->utc);                                // bool(false)
var_dump(Carbon::now('Europe/London')->utc);                 // bool(true)
var_dump(Carbon::createFromTimestampUTC(0)->utc);            // bool(true)

// 取得時區實例
echo get_class(Carbon::now()->timezone);                     // DateTimeZone
echo get_class(Carbon::now()->tz);                           // DateTimeZone

// 取得時區實例的名稱
echo Carbon::now()->timezoneName;                            // America/Toronto
echo Carbon::now()->tzName;                                  // America/Toronto
```

## 設定時間資料


```php
<?php

use Carbon\Carbon;

$dt = Carbon::now();

$dt->year = 1975;
$dt->month = 13;             // 年份會強制 +1，且月份變為 1 月
$dt->month = 5;
$dt->day = 21;
$dt->hour = 22;
$dt->minute = 32;
$dt->second = 5;

$dt->timestamp = 169957925;  // 這個設定不會變更時區

// 透過字串或是 DateTimeZone 實例去設定時區
$dt->timezone = new DateTimeZone('Europe/London');
$dt->timezone = 'Europe/London';
$dt->tz = 'Europe/London';



// 鏈結設定方式
$dt->year(1975)->month(5)->day(21)->hour(22)->minute(32)->second(5)->toDateTimeString();
$dt->setDate(1975, 5, 21)->setTime(22, 32, 5)->toDateTimeString();
$dt->setDateTime(1975, 5, 21, 22, 32, 5)->toDateTimeString();

$dt->timestamp(169957925)->timezone('Europe/London');

$dt->tz('America/Toronto')->setTimezone('America/Vancouver');
```

## 格式化時間資料

```php
<?php

use Carbon\Carbon;

$dt = Carbon::create(1975, 12, 25, 14, 15, 16);

var_dump($dt->toDateTimeString() == $dt);          // bool(true) => uses __toString()
echo $dt->toDateString();                          // 1975-12-25
echo $dt->toFormattedDateString();                 // Dec 25, 1975
echo $dt->toTimeString();                          // 14:15:16
echo $dt->toDateTimeString();                      // 1975-12-25 14:15:16
echo $dt->toDayDateTimeString();                   // Thu, Dec 25, 1975 2:15 PM

// 仍可以使用 format() 函式
echo $dt->format('l jS \\of F Y h:i:s A');         // Thursday 25th of December 1975 02:15:16 PM

// 常用的時間格式
echo $dt->toAtomString();      // in 1 Jahr
echo $dt->toCookieString();    // Thursday, 25-Dec-1975 14:15:16 EST
echo $dt->toIso8601String();   // 1975-12-25T14:15:16-0500
echo $dt->toRfc822String();    // Thu, 25 Dec 75 14:15:16 -0500
echo $dt->toRfc850String();    // Thursday, 25-Dec-75 14:15:16 EST
echo $dt->toRfc1036String();   // Thu, 25 Dec 75 14:15:16 -0500
echo $dt->toRfc1123String();   // Thu, 25 Dec 1975 14:15:16 -0500
echo $dt->toRfc2822String();   // Thu, 25 Dec 1975 14:15:16 -0500
echo $dt->toRfc3339String();   // 1975-12-25T14:15:16-05:00
echo $dt->toRssString();       // Thu, 25 Dec 1975 14:15:16 -0500
echo $dt->toW3cString();       // 1975-12-25T14:15:16-05:00
```

## 比較時間差異

```php
<?php

use Carbon\Carbon;

echo Carbon::now()->tzName;                        // America/Toronto
$first = Carbon::create(2012, 9, 5, 23, 26, 11);
$second = Carbon::create(2012, 9, 5, 20, 26, 11, 'America/Vancouver');

echo $first->toDateTimeString();                   // 2012-09-05 23:26:11
echo $first->tzName;                               // America/Toronto
echo $second->toDateTimeString();                  // 2012-09-05 20:26:11
echo $second->tzName;                              // America/Vancouver

// 大於、等於、小於
var_dump($first->eq($second));                     // bool(true)
var_dump($first->ne($second));                     // bool(false)
var_dump($first->gt($second));                     // bool(false)
var_dump($first->gte($second));                    // bool(true)
var_dump($first->lt($second));                     // bool(false)
var_dump($first->lte($second));                    // bool(true)

$first->setDateTime(2012, 1, 1, 0, 0, 0);
$second->setDateTime(2012, 1, 1, 0, 0, 0);         // Remember tz is 'America/Vancouver'

var_dump($first->eq($second));                     // bool(false)
var_dump($first->ne($second));                     // bool(true)
var_dump($first->gt($second));                     // bool(false)
var_dump($first->gte($second));                    // bool(false)
var_dump($first->lt($second));                     // bool(true)
var_dump($first->lte($second));                    // bool(true)



// 時間區間比較
$first = Carbon::create(2012, 9, 5, 1);
$second = Carbon::create(2012, 9, 5, 5);
var_dump(Carbon::create(2012, 9, 5, 3)->between($first, $second));          // bool(true)
var_dump(Carbon::create(2012, 9, 5, 5)->between($first, $second));          // bool(true)
var_dump(Carbon::create(2012, 9, 5, 5)->between($first, $second, false));   // bool(false)


// 時間大小比較
$dt1 = Carbon::create(2012, 1, 1, 0, 0, 0);
$dt2 = Carbon::create(2014, 1, 30, 0, 0, 0);
echo $dt1->min($dt2);                              // 2012-01-01 00:00:00

$dt1 = Carbon::create(2012, 1, 1, 0, 0, 0);
$dt2 = Carbon::create(2014, 1, 30, 0, 0, 0);
echo $dt1->max($dt2);                              // 2014-01-30 00:00:00

// now is the default param
$dt1 = Carbon::create(2000, 1, 1, 0, 0, 0);
echo $dt1->max();


// 時間差異運算
echo Carbon::now('America/Vancouver')->diffInSeconds(Carbon::now('Europe/London')); // 0

$dtOttawa = Carbon::createFromDate(2000, 1, 1, 'America/Toronto');
$dtVancouver = Carbon::createFromDate(2000, 1, 1, 'America/Vancouver');
echo $dtOttawa->diffInHours($dtVancouver);                             // 3

echo $dtOttawa->diffInHours($dtVancouver, false);                      // 3
echo $dtVancouver->diffInHours($dtOttawa, false);                      // -3

$dt = Carbon::create(2012, 1, 31, 0);
echo $dt->diffInDays($dt->copy()->addMonth());                         // 31
echo $dt->diffInDays($dt->copy()->subMonth(), false);                  // -31

$dt = Carbon::create(2012, 4, 30, 0);
echo $dt->diffInDays($dt->copy()->addMonth());                         // 30
echo $dt->diffInDays($dt->copy()->addWeek());                          // 7

$dt = Carbon::create(2012, 1, 1, 0);
echo $dt->diffInMinutes($dt->copy()->addSeconds(59));                  // 0
echo $dt->diffInMinutes($dt->copy()->addSeconds(60));                  // 1
echo $dt->diffInMinutes($dt->copy()->addSeconds(119));                 // 1
echo $dt->diffInMinutes($dt->copy()->addSeconds(120));                 // 2

echo $dt->addSeconds(120)->secondsSinceMidnight();                     // 120
```

## 時間狀態

```php
<?php

use Carbon\Carbon;

$dt = Carbon::now();

$dt->isWeekday();
$dt->isWeekend();
$dt->isYesterday();
$dt->isToday();
$dt->isTomorrow();
$dt->isFuture();
$dt->isPast();
$dt->isLeapYear();
$dt->isSameDay(Carbon::now());
$born = Carbon::createFromDate(1987, 4, 23);
$noCake = Carbon::createFromDate(2014, 9, 26);
$yesCake = Carbon::createFromDate(2014, 4, 23);
var_dump($born->isBirthday($noCake));              // bool(false)  
var_dump($born->isBirthday($yesCake));             // bool(true)  
```


## 時間運算

```php
<?php

use Carbon\Carbon;

$dt = Carbon::create(2012, 1, 31, 0);

echo $dt->toDateTimeString();            // 2012-01-31 00:00:00

echo $dt->addYears(5);                   // 2017-01-31 00:00:00
echo $dt->addYear();                     // 2018-01-31 00:00:00
echo $dt->subYear();                     // 2017-01-31 00:00:00
echo $dt->subYears(5);                   // 2012-01-31 00:00:00

echo $dt->addMonths(60);                 // 2017-01-31 00:00:00
echo $dt->addMonth();                    // 2017-03-03 00:00:00 equivalent of $dt->month($dt->month + 1); so it wraps
echo $dt->subMonth();                    // 2017-02-03 00:00:00
echo $dt->subMonths(60);                 // 2012-02-03 00:00:00

echo $dt->addDays(29);                   // 2012-03-03 00:00:00
echo $dt->addDay();                      // 2012-03-04 00:00:00
echo $dt->subDay();                      // 2012-03-03 00:00:00
echo $dt->subDays(29);                   // 2012-02-03 00:00:00

echo $dt->addWeekdays(4);                // 2012-02-09 00:00:00
echo $dt->addWeekday();                  // 2012-02-10 00:00:00
echo $dt->subWeekday();                  // 2012-02-09 00:00:00
echo $dt->subWeekdays(4);                // 2012-02-03 00:00:00

echo $dt->addWeeks(3);                   // 2012-02-24 00:00:00
echo $dt->addWeek();                     // 2012-03-02 00:00:00
echo $dt->subWeek();                     // 2012-02-24 00:00:00
echo $dt->subWeeks(3);                   // 2012-02-03 00:00:00

echo $dt->addHours(24);                  // 2012-02-04 00:00:00
echo $dt->addHour();                     // 2012-02-04 01:00:00
echo $dt->subHour();                     // 2012-02-04 00:00:00
echo $dt->subHours(24);                  // 2012-02-03 00:00:00

echo $dt->addMinutes(61);                // 2012-02-03 01:01:00
echo $dt->addMinute();                   // 2012-02-03 01:02:00
echo $dt->subMinute();                   // 2012-02-03 01:01:00
echo $dt->subMinutes(61);                // 2012-02-03 00:00:00

echo $dt->addSeconds(61);                // 2012-02-03 00:01:01
echo $dt->addSecond();                   // 2012-02-03 00:01:02
echo $dt->subSecond();                   // 2012-02-03 00:01:01
echo $dt->subSeconds(61);                // 2012-02-03 00:00:00
```

## 人類閱讀時間格式

```php
<?php

use Carbon\Carbon;

// 通常會用在留言的時間顯示
// 該時間會比較與現在的時間的差異
echo Carbon::now()->subDays(5)->diffForHumans();               // 5 days ago

echo Carbon::now()->diffForHumans(Carbon::now()->subYear());   // 1 year after

$dt = Carbon::createFromDate(2011, 8, 1);

echo $dt->diffForHumans($dt->copy()->addMonth());              // 1 month before
echo $dt->diffForHumans($dt->copy()->subMonth());              // 1 month after

echo Carbon::now()->addSeconds(5)->diffForHumans();            // 5 seconds from now

echo Carbon::now()->subDays(24)->diffForHumans();              // 3 weeks ago
echo Carbon::now()->subDays(24)->diffForHumans(null, true);    // 3 weeks
```

## 時間常數

```php
<?php

use Carbon\Carbon;

var_dump(Carbon::SUNDAY);                          // int(0)
var_dump(Carbon::MONDAY);                          // int(1)
var_dump(Carbon::TUESDAY);                         // int(2)
var_dump(Carbon::WEDNESDAY);                       // int(3)
var_dump(Carbon::THURSDAY);                        // int(4)
var_dump(Carbon::FRIDAY);                          // int(5)
var_dump(Carbon::SATURDAY);                        // int(6)

var_dump(Carbon::YEARS_PER_CENTURY);               // int(100)
var_dump(Carbon::YEARS_PER_DECADE);                // int(10)
var_dump(Carbon::MONTHS_PER_YEAR);                 // int(12)
var_dump(Carbon::WEEKS_PER_YEAR);                  // int(52)
var_dump(Carbon::DAYS_PER_WEEK);                   // int(7)
var_dump(Carbon::HOURS_PER_DAY);                   // int(24)
var_dump(Carbon::MINUTES_PER_HOUR);                // int(60)
var_dump(Carbon::SECONDS_PER_MINUTE);              // int(60)
```

## 參考資料
* [Carbon - A simple PHP API extension for DateTime.](http://carbon.nesbot.com/)
* [Carbon - docs](http://carbon.nesbot.com/docs/)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
