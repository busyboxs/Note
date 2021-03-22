# boost date_time library

## Gregorian

### Date

----

#### Construction

|Sytax|Description|Example|
|:----|:----------|:------|
|`date(greg_year, greg_month, greg_day)`|从日期的一部分构造，如果年、月或日超出范围，则抛出 `bad_year`, `bad_day_of_month`, `bad_day_month`|`date d(200, Jan, 10);`|
|`date(date d)`|拷贝构造函数|`date d1(d);`|
|`date(special_values sv)`|构造无限日期时间，非日期时间，最大最小日期时间|<ul><li>`date d1(neg_infin);`</li><li>`date d2(pos_infin);`</li><li>`date d3(not_a_date_time);`</li><li>`date d4(max_date_time);`</li><li>`date d5(min_date_time);`</li></ul>|
|`date()`|默认构造函数，初始化为 `not_a_date_time`，可以通过定义 ` DATE_TIME_NO_DEFAULT_CONSTRUCTOR` 来禁用此构造函数|`date d; // d => not_a_date_time`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // construction
    date d(2021, Mar, 14);
    std::cout << d << std::endl;  // 2021-Mar-14

    date d1(d);
    std::cout << d1 << std::endl;  // 2021-Mar-14

    date d_neg_inf(neg_infin);
    std::cout << d_neg_inf << std::endl;  // -infinity
    date d_pos_inf(pos_infin);
    std::cout << d_pos_inf << std::endl;  // +infinity
    date d_not_a_date(not_a_date_time);
    std::cout << d_not_a_date << std::endl;  // not-a-date-time
    date d_max(max_date_time);
    std::cout << d_max << std::endl;  // 9999-Dec-31
    date d_min(min_date_time);
    std::cout << d_min << std::endl;  // 1400-Jan-01

    date d_default;
    std::cout << d_default << std::endl;  // not-a-date-time
}
```

----

#### Construct from String

|Sytax|Description|Example|
|:----|:----------|:------|
|`date from_string(std::string)`|从 `-` 或者 `/` 分隔符的字符串构造日期对象，顺序为年月日|`date d(from_string(std::string("2021-3-14")));`|
|`date from_undelimited_string(std::string)`|从 ISO 类型日期字符串构建日期对象，顺序为年月日|`date d(from_undelimited_string(std::string("20210314")));`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // Construct from string
    std::string ds("2021/3/14");
    date dfs(from_string(ds));
    std::cout << dfs << std::endl;  // 2021-Mar-14

    std::string ds2("2021-3-14");
    date dfs2(from_string(ds2));
    std::cout << dfs2 << std::endl;  // 2021-Mar-14

    std::string ds_undel("20210314");
    date d_undel(from_undelimited_string(ds_undel));
    std::cout << d_undel << std::endl;  // 2021-Mar-14
}
```

#### Construct from Clock

|Sytax|Description|Example|
|:----|:----------|:------|
|`day_clock::local_day()`|根据计算机的时区设置获取本地日期|`date d(day_clock::local_day());`|
|`day_clock::universal_day(`|获取 UTC 日期|`date d(day_clock::universal_day());`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // construct from clock
    date d_local_clock(day_clock::local_day());
    std::cout << d_local_clock << std::endl;  // 2021-Mar-14

    date d_utc_clock(day_clock::universal_day());
    std::cout << d_utc_clock << std::endl;  // 2021-Mar-14

    date dddd(from_string(std::string("2021-3-14")));
    std::cout << dddd << std::endl;
}
```

#### Accessors

|Sytax|Description|Example|
|:----|:----------|:------|
|`greg_year year() const`|获取日期的年份部分|`date d(2002,Jan,10); d.year(); // --> 2002`|
|`greg_month month() const`|获取日期的月份部分|`date d(2002,Jan,10); d.month(); // --> 1`|
|`greg_day day() const`|获取日期的日部分|`date d(2002,Jan,10); d.day(); // --> 10`|
|`greg_ymd year_month_day() const`|返回 `year_month_day` 结构体。当需要日期的所有3个部分时，效率更高|`date d(2002,Jan,10); date::ymd_type ymd = d.year_month_day(); // ymd.year  --> 2002, ymd.month --> 1, ymd.day   --> 10`|
|`greg_day_of_week day_of_week() const`|获取星期几|`date d(2002,Jan,10); d.day_of_week(); // --> Thursday`|
|`greg_day_of_year day_of_year() const`|获取一年中的哪一天。从 1 到 366|`date d(2000,Jan,10); d.day_of_year(); // --> 10`|
|`date end_of_month() const`|返回当前月的最后一天的日期对象|`date d(2000,Jan,10); d.end_of_month(); // --> 2000-Jan-31`|
|`bool is_infinity() const`|如果日期是正无穷大或负无穷大，则返回 true|`date d(pos_infin);  d.is_infinity(); // --> true`|
|`bool is_neg_infinity() const`|如果日期为负无穷，则返回 true|`date d(neg_infin); d.is_neg_infinity(); // --> true`|
|`bool is_pos_infinity() const`|如果日期为正无穷大，则返回 true|`date d(pos_infin);  d.is_pos_infinity(); // --> true`|
|`bool is_not_a_date() const`|如果值不是日期，则返回 true|`date d(not_a_date_time); d.is_not_a_date(); // --> true`|
|`bool is_special() const`|如果日期是任何 `special_value`，则返回 true|`date d(pos_infin);  date d2(not_a_date_time); date d3(2005,Mar,1); d.is_special(); // --> true d2.is_special(); // --> true d3.is_special(); // --> false`|
|`long modjulian_day() const`|返回日期的修改后的儒略日||
|`long julian_day() const`|返回日期的儒略日||
|`int week_number() const`|返回日期的ISO 8601周编号||
|`date end_of_month() const`|返回日期的月份的最后一天|`date d(2000,Feb,1); //gets Feb 29 -- 2000 was leap year`, `date eom = d.end_of_month();`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>
#include <vector>

using namespace boost::gregorian;

int main()
{
    // Accessors
    date today(day_clock::local_day());
    greg_year year = today.year();
    greg_month month = today.month();
    greg_day day = today.day();
    std::cout << "year: "<< year << ", month: " << month << ", day: " << day << std::endl;  // year: 2021, month: Mar, day: 14

    date::ymd_type ymd = today.year_month_day();
    std::cout << "year: "<< ymd.year << ", month: " << ymd.month << ", day: " << ymd.day << std::endl;  // year: 2021, month: Mar, day: 14

    date::day_of_week_type dow = today.day_of_week();
    std::cout << "day of week for today: " << dow << std::endl;  // day of week for today: Sun

    date::day_of_year_type doy = today.day_of_year();
    std::cout << "day of year for today: " << doy << std::endl;  // day of year for today: 73

    date eom = today.end_of_month();
    std::cout << "end of month for today: " << eom << std::endl;  // end of month for today: 2021-Mar-31

    std::cout << std::boolalpha;
    std::cout << "Is infinity for today: " << today.is_infinity() << std::endl;  // Is infinity for today: false
    std::cout << "Is neg infinity for today: " << today.is_neg_infinity() << std::endl;  // Is neg infinity for today: false
    std::cout << "Is pos infinity for today: " << today.is_pos_infinity() << std::endl;  // Is pos infinity for today: false
    std::cout << "Is not a date for today: " << today.is_not_a_date() << std::endl;  // Is not a date for today: false
    std::cout << "Is special for today: " << today.is_special() << std::endl;  // Is special for today: false

    std::vector<std::string> special_string = {"not_a_date_time",
                                               "neg_infin", "pos_infin",
                                               "min_date_time",  "max_date_time",
                                               "not_special", "NumSpecialValues"};
    std::cout << "As special for today: " << special_string[static_cast<int>(today.as_special())] << std::endl;  // As special for today: not_special

    std::cout << "modified julian day for today: " << today.modjulian_day() << std::endl;  // modified julian day for today: 59287
    std::cout << "julian day for today: " << today.julian_day() << std::endl;  // julian day for today: 2459288
    std::cout << "week number of today: " << today.week_number() << std::endl;  // week number of today: 10
    std::cout << "day number of today: " << today.day_number() << std::endl;  // day number of today: 2459288
}
```

----

#### Convert to String

|Sytax|Description|Example|
|:----|:----------|:------|
|`std::string to_simple_string(date d)`|到 `YYYY-mmm-DD` 字符串，其中 `mmm` 是 3 个字符的月份名称|`"2002-Jan-01"`|
|`std::string to_iso_string(date d)`|到 `YYYYMMDD`，其中所有部分都是整数|`"20020131"`|
|`std::string to_iso_extended_string(date d)`|到 `YYYY-MM-DD`，其中所有部分均为整数|`"2002-01-31"`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // convert to string
    std::string str_today = to_simple_string(today);
    std::cout << "to_simple_string: " << str_today << std::endl;  // to_simple_string: 2021-Mar-14
    std::string str_iso_today = to_iso_string(today);
    std::cout << "to_iso_string: " << str_iso_today << std::endl;  // to_iso_string: 20210314
    std::string str_iso_extend = to_iso_extended_string(today);
    std::cout << "to_iso_extended_string: " << str_iso_extend << std::endl;  // to_iso_extended_string: 2021-03-14
    std::wstring wstr_today = to_simple_wstring(today);
    std::wcout << "to_simple_wstring: "<< wstr_today << std::endl;  // to_simple_wstring: 2021-Mar-14
}
```

----

#### Operators

|Sytax|Description|
|:----|:----------|
|`operator<<`|流输出运算符|
|`operator>>`|流输入运算符|
|`operator==`, `operator!=`,`operator>`, `operator<`, `operator>=`, `operator<=`|比较运算符|
|`date operator+(date_duration) const`|返回日期加上日期偏移量|
|`date operator-(date_duration) const`|减去日期偏移量以返回日期|
|`date_duration operator-(date) const`|通过减去两个日期来返回 `date_duration`|

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // operator
    std::stringstream ss("2021-Mar-14");
    date date_from_str;
    ss >> date_from_str;
    std::cout << date_from_str << std::endl;  // 2021-Mar-14

    date yesterday;
    date tomorrow;
    date_duration one_day(1);
    yesterday = today - one_day;
    tomorrow = today + one_day;
    std::cout << "yesterday: " << yesterday << std::endl;  // yesterday: 2021-Mar-13
    std::cout << "today: " << today << std::endl;  // today: 2021-Mar-14
    std::cout << "tomorrow:" << tomorrow << std::endl;  // tomorrow:2021-Mar-15

    std::cout << "yesterday == today? :" << (yesterday == today) << std::endl;  // yesterday == today? :false
    std::cout << "yesterday != today? :" << (yesterday != today) << std::endl;  // yesterday != today? :true
    std::cout << "yesterday > today? :" << (yesterday > today) << std::endl;  // yesterday > today? :false
    std::cout << "yesterday < today? :" << (yesterday < today) << std::endl;  // yesterday < today? :true
    std::cout << "yesterday >= today? :" << (yesterday >= today) << std::endl;  // yesterday >= today? :false
    std::cout << "yesterday <= today? :" << (yesterday <= today) << std::endl;  // yesterday <= today? :true
}
```

----

#### Struct tm Functions

|Sytax|Description|Example|
|:----|:----------|:------|
|`tm to_tm(date)`|用于将日期对象转换为 `tm` 结构的函数。字段 `tm_hour`，`tm_min` 和 `tm_sec` 设置为零。`tm_isdst` 字段设置为 `-1`||
|`date date_from_tm(tm datetm)`|用于将 `tm` 结构转换为日期对象的函数。字段：`tm_wday`, `tm_yday`, `tm_hour`, `tm_min`, `tm_sec` 和 `tm_isdst` 被忽略||

```cpp
#include <iostream>
#include <boost/date_time/gregorian/gregorian.hpp>

using namespace boost::gregorian;

int main()
{
    // Struct tm Functions
    tm d_tm = to_tm(today);
    std::cout << "tm_year: " << d_tm.tm_year << std::endl;  // tm_year: 121
    std::cout << "tm_mon: " << d_tm.tm_mon << std::endl;  // tm_mon: 2
    std::cout << "tm_mday: " << d_tm.tm_mday << std::endl;  // tm_mday: 14
    std::cout << "tm_wday: " << d_tm.tm_wday << std::endl;  // tm_wday: 0
    std::cout << "tm_yday: " << d_tm.tm_yday << std::endl;  // tm_yday: 72
    std::cout << "tm_hour: " << d_tm.tm_hour << std::endl;  // tm_hour: 0
    std::cout << "tm_min: " << d_tm.tm_min << std::endl;  // tm_min: 0
    std::cout << "tm_sec: " << d_tm.tm_sec << std::endl;  // tm_sec: 0
    std::cout << "tm_isdst: " << d_tm.tm_isdst << std::endl;  // tm_isdst: -1

    d_tm.tm_hour = 12;
    d_tm.tm_min = 15;
    d_tm.tm_sec = 49;
    date d_from_tm = date_from_tm(d_tm);
    std::cout << d_from_tm << std::endl;  // 2021-Mar-14
}
```

----

### Date Duration

`date_duration` 和 `days` 是一样的

```cpp
typedef date_duration days;
```

|Syntax|Description|
|:-----|:----------|
|`date_duration(long)`|Create a duration count|
|`days(special_values sv)`|Constructor for infinities, not-a-date-time, max_date_time, and min_date_time|
|`long days() const`|Get the day count|
|`bool is_negative() const`|True if number of days is less than zero|
|`static date_duration unit()`|Return smallest possible unit of duration type|
|`bool is_special() const`|Returns true if days is any `special_value`|
|`operator<<`, `operator>>`|Streaming operators|
|`operator==`, `operator!=`, `operator>`, `operator<`, `operator>=`, `operator<=`|A full complement of comparison operators|
|`date_duration operator+(date_duration) const`|Add date durations|
|`date_duration operator-(date_duration) const`|Subtract durations|
|`months(int num_of_months)`|A logical month representation|
|`years(int num_of_years)`|A logical representation of a year|
|`weeks(int num_of_weeks)`|A duration type representing a number of `n * 7` days|

### Date Period

|Syntax|Description|
|:-----|:----------|
|`date_period(date, date)`|Create a period as [begin, end). If end is <= begin then the period will be invalid|
|`date_period(date, days)`|Create a period as [begin, begin+len) where end point would be begin+len. If len is <= zero then the period will be defined as invalid|
|`date_period(date_period)`|Copy constructor|
|`date_period shift(days)`|Add duration to both begin and end|
|`date_period expand(days)`|Subtract duration from begin and add duration to end|
|`date begin()`|Return first day of period|
|`date last()`|Return last date in the period|
|`date end()`|Return one past the last in period|
|`days length()`|Return the length of the date_period|
|`bool is_null()`|True if period is not well formed. eg: end less than or equal to begin|
|`bool contains(date)`|True if date is within the period. Zero length periods cannot contain any points|
|`bool contains(date_period)`|True if date period is within the period|
|`bool intersects(date_period)`|True if periods overlap|
|`date_period intersection(date_period)`|Calculate the intersection of 2 periods. Null if no intersection|
|`date_period is_adjacent(date_period)`|Check if two periods are adjacent, but not overlapping|
|`date_period is_after(date)`|Determine the period is after a given date.|
|`date_period is_before(date)`|Determine the period is before a given date|
|`date_period merge(date_period)`|Returns union of two periods. Null if no intersection|
|`date_period span(date_period)`|Combines two periods and any gap between them such that begin = min(p1.begin, p2.begin) and end = max(p1.end , p2.end)|
|`date_period shift(days)`|Add duration to both begin and end|
|`date_period expand(days)`|Subtract duration from begin and add duration to end|
|`std::string to_simple_string(date_period dp)`|To [YYYY-mmm-DD/YYYY-mmm-DD] string where mmm is 3 char month name|
|`operator<<`|ostream operator for date_period. Uses facet to format time points. Typical output: `[2002-Jan-01/2002-Jan-31]`.|
|`operator>>`|istream operator for date_period. Uses facet to parse time points|
|`operator==`, `operator!=`, `operator>`, `operator<`|A full complement of comparison operators|
|`operator<`|True if `dp1.end()` less than `dp2.begin()`|
|`operator>`|True if `dp1.begin()` greater than d`p2.end()`|

## Posix Time

### Ptime

|Syntax|Description|
|:-----|:----------|
|`ptime(date,time_duration)`|Construct from a date and offset|
|`ptime(ptime)`|Copy constructor|
|`ptime(special_values sv)`|Constructor for infinities, not-a-date-time, max_date_time, and min_date_time|
|`ptime`|Default constructor. Creates a ptime object initialized to not_a_date_time|
|`ptime time_from_string(std::string)`|From delimited string|
|`ptime from_iso_string(std::string)`|From non delimited iso form string|
|`ptime from_iso_extended_string(std::string)`|From delimited iso form string|
|`ptime second_clock::local_time()`|Get the local time, second level resolution, based on the time zone settings of the computer.|
|`ptime second_clock::universal_time()`|Get the UTC time|
|`ptime microsec_clock::local_time()`|Get the local time using a sub second resolution clock.|
|`ptime microsec_clock::universal_time()`|Get the UTC time using a sub second resolution clock|
|`ptime from_time_t(time_t t)`|Converts a time_t into a ptime|
|`ptime from_ftime<ptime>(FILETIME ft)`|Creates a ptime object from a FILETIME structure|
|`date date()`|Get the date part of a time|
|`time_duration time_of_day()`|Get the time offset in the day|
|`bool is_infinity() const`|Returns true if ptime is either positive or negative infinity|
|`bool is_neg_infinity() const`|Returns true if ptime is negative infinity|
|`bool is_pos_infinity() const`|Returns true if ptime is positive infinity|
|`bool is_not_a_date_time() const`|Returns true if value is not a ptime|
|`bool is_special() const`|Returns true if ptime is any special_value|
|`std::string to_simple_string(ptime)`|To `YYYY-mmm-DD HH:MM:SS.fffffffff` string where `mmm` 3 char month name. Fractional seconds only included if non-zero|
|`std::string to_iso_string(ptime)`|Convert to form `YYYYMMDDTHHMMSS,fffffffff` where `T` is the date-time separator|
|`std::string to_iso_extended_string(ptime)`|Convert to form `YYYY-MM-DDTHH:MM:SS,fffffffff` where `T` is the date-time separator|
|`operator<<`, `operator>>`|Streaming operators|
|`operator==`, `operator!=`, `operator>`, `operator<`, `operator>=`, `operator<=`|A full complement of comparison operators|
|`ptime operator+(days)`|Return a ptime adding a day offset|
|`ptime operator-(days)`|Return a ptime subtracting a day offset|
|`ptime operator+(time_duration)`|Return a ptime adding a time duration|
|`ptime operator-(time_duration)`|Return a ptime subtracting a time duration|
|`time_duration operator-(ptime)`|Take the difference between two times|
|`tm to_tm(ptime)`|A function for converting a `ptime` object to a `tm` struct. The `tm_isdst` field is set to `-1`|
|`ptime ptime_from_tm(tm timetm)`|A function for converting a `tm` struct to a `ptime` object. The fields: `tm_wday`, `tm_yday`, and `tm_isdst` are ignored|
|`tm to_tm(time_duration)`|A function for converting a `time_duration` object to a `tm` struct. The fields: `tm_year`, `tm_mon`, `tm_mday`, `tm_wday`, `tm_yday` are set to zero. The `tm_isdst` field is set to -1|
|`ptime from_time_t(std::time_t)`|Creates a `ptime` from the `time_t` parameter. The seconds held in the time_t are added to a time point of `1970-Jan-01`|
|`ptime from_ftime<ptime>(FILETIME)`|A template function that constructs a `ptime` from a FILETIME struct|

----

### Time Duration

|Syntax|Description|
|:-----|:----------|
|`time_duration(hours, minutes, seconds, fractional_seconds)`|Construct a duration from the counts|
|`time_duration(special_value sv)`|Special values constructor|
|`hours(long)`|Number of hours|
|`minutes(long)`|Number of minutes|
|`seconds(long)`|Number of seconds|
|`milliseconds(long)`|Number of milliseconds|
|`microseconds(long)`|Number of microseconds|
|`nanoseconds(long)`|Number of nanoseconds|
|`time_duration duration_from_string(std::string)`|From delimited string|
|`boost::int64_t hours()`|Get the number of normalized hours (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t minutes()`|Get the number of minutes normalized `+/-(0..59)` (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t seconds() const`|Get the normalized number of second `+/-(0..59)` (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t total_seconds() const`|Get the total number of seconds truncating any fractional seconds (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t total_milliseconds() const`|Get the total number of milliseconds truncating any remaining digits (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`oost::int64_t total_microseconds() const`|Get the total number of microseconds truncating any remaining digits (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t total_nanoseconds() const`|Get the total number of nanoseconds truncating any remaining digits (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`boost::int64_t fractional_seconds() const`|Get the number of fractional seconds (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`bool is_negative() const`|True if and only if duration is negative|
|`bool is_zero() const`|True if and only if duration is zero|
|`bool is_positive() const`|True if and only if duration is positive|
|`time_duration invert_sign() const`|Generate a new duration with the sign inverted|
|`time_duration abs() const`|Generate a new duration with the absolute value of the time duration|
|`date_time::time_resolutions time_duration::resolution()`|Describes the resolution capability of the `time_duration` class. `time_resolutions` is an enum of resolution possibilities ranging from seconds to nanoseconds|
|`unsigned short time_duration::num_fractional_digits()`|Returns the number of fractional digits the time resolution has|
|`boost::int64_t time_duration::ticks_per_second()`|Return the number of ticks in a second. For example, if the duration supports nanoseconds then the returned result will be `1,000,000,000 (1e+9)`|
|`boost::int64_t ticks()`|Return the raw count of the duration type (will give unpredictable results if calling `time_duration` is a `special_value`)|
|`time_duration time_duration::unit()`|Return smallest possible unit of duration type (1 nanosecond)|
|`bool is_neg_infinity() const`|Returns true if time_duration is negative infinity|
|`bool is_pos_infinity() const`|Returns true if time_duration is positive infinity|
|`bool is_not_a_date_time() const`|Returns true if value is not a time|
|`bool is_special() const`|Returns true if time_duration is any `special_value`|
|`std::string to_simple_string(time_duration)`|To `HH:MM:SS.fffffffff` were `fff` is fractional seconds that are only included if non-zero|
|`std::string to_iso_string(time_duration)`|Convert to form `HHMMSS,fffffffff`|
|`operator<<`, `operator>>`|Streaming operators|
|`operator==`, `operator!=`, `operator>`, `operator<`, `operator>=`, `operator<=`|A full complement of comparison operators|
|`time_duration operator+(time_duration)`|Add durations|
|`time_duration operator-(time_duration)`|Subtract durations|
|`time_duration operator/(int)`|Divide the length of a duration by an integer value. Discards any remainder|
|`time_duration operator*(int)`|Multiply the length of a duration by an integer value|
|`tm to_tm(time_duration)`|A function for converting a `time_duration` object to a `tm` struct. The fields: `tm_year`, `tm_mon`, `tm_mday`, `tm_wday`, `tm_yday` are set to zero. The `tm_isdst` field is set to -1|

### Time Period

|Syntax|Description|
|:-----|:----------|
|`time_period(ptime, ptime)`|Create a period as `[begin, end)`. If end is <= begin then the period will be defined as invalid|
|`time_period(ptime, time_duration)`|Create a period as `[begin, begin+len)` where end would be `begin+len`. If len is <= zero then the period will be defined as invalid|
|`time_period(time_period rhs)`|Copy constructor|
|`time_period shift(time_duration)`|Add duration to both begin and end|
|`time_period expand(days)`|Subtract duration from begin and add duration to end|
|`ptime begin()`|Return first time of period|
|`ptime last()`|Return last time in the period|
|`ptime end()`|Return one past the last in period|
|`time_duration length()`|Return the length of the time period|
|`bool is_null()`|True if period is not well formed. eg: end is less than or equal to begin.|
|`bool contains(ptime)`|True if ptime is within the period. Zero length periods cannot contain any points|
|`bool contains(time_period)`|True if period is within the period|
|`bool intersects(time_period)`|True if periods overlap|
|`time_period intersection(time_period)`|Calculate the intersection of 2 periods. Null if no intersection|
|`time_period merge(time_period)`|Returns union of two periods. Null if no intersection|
|`time_period span(time_period)`|Combines two periods and any gap between them such that begin = min(p1.begin, p2.begin) and end = max(p1.end , p2.end)|
|`std::string to_simple_string(time_period dp)`|To `[YYYY-mmm-DD hh:mm:ss.fffffffff/YYYY-mmm-DD hh:mm:ss.fffffffff]` string where `mmm` is 3 char month name|
|`operator<<`|Output streaming operator for time duration|
|`operator>>`|Input streaming operator for time duration|
|`operator==`, `operator!=`|Equality operators. Periods are equal if `p1.begin == p2.begin && p1.last == p2.last`|
|`operator<`|Ordering with no overlap. True if `tp1.end()` less than `tp2.begin()`|
|`operator>`|Ordering with no overlap. True if `tp1.begin()` greater than `tp2.end()`|
|`operator<=`, `operator>=`|Defined in terms of the other operators|

### Time Iterators

|Syntax|Description|
|:-----|:----------|
|`operator==(const ptime& rhs)`, `operator!=(const ptime& rhs)`, `operator>`, `operator<`, `operator>=`, `operator<=`|A full complement of comparison operators|
|`prefix increment`|Increment the iterator by the specified duration|
|`prefix decrement`|Decrement the iterator by the specified time duration|

----

## Local Time

### Time Zone (abstract)

|Syntax|Description|
|:-----|:----------|
|`string_type dst_zone_abbrev()`|Returns the daylight savings abbreviation for the represented time zone|
|`string_type std_zone_abbrev()`|Returns the standard abbreviation for the represented time zone|
|`string_type dst_zone_name()`|Returns the daylight savings name for the represented time zone|
|`string_type std_zone_name()`|Returns the standard name for the represented time zone|
|`bool has_dst()`|Returns true if this time zone does not make a daylight savings shift|
|`time_type dst_local_start_time(year_type)`|The date and time daylight savings time begins in given year|
|`time_type dst_local_end_time(year_type)`|The date and time daylight savings time ends in given year|
|`time_duration_type base_utc_offset()`|The amount of time offset from UTC (typically in hours)|
|`time_duration_type dst_offset()`|The amount of time shifted during daylight savings|
|`std::string to_posix_string()`|Returns a posix time zone string representation of this time_zone_base object|

### Posix Time Zone

|Syntax|Description|
|:-----|:----------|
|`posix_time_zone(std::string)`|Construction|
|`std::string dst_zone_abbrev()`|Returns the daylight savings abbreviation for the represented time zone|
|`std::string std_zone_abbrev()`|Returns the standard abbreviation for the represented time zone|
|`std::string dst_zone_name()`|Returns the daylight savings ABBREVIATION for the represented time zone|
|`std::string std_zone_name()`|Returns the standard ABBREVIATION for the represented time zone|
|`bool has_dst()`|Returns true when time_zone's shared_ptr to dst_calc_rules is not NULL|
|`ptime dst_local_start_time(greg_year)`|The date and time daylight savings time begins in given year. Returns not_a_date_time if this zone has no daylight savings|
|`ptime dst_local_end_time(greg_year)`|The date and time daylight savings time ends in given year. Returns not_a_date_time if this zone has no daylight savings|
|`time_duration base_utc_offset()`|The amount of time offset from UTC (typically in hours)|
|`posix_time::time_duration dst_offset()`|The amount of time shifted during daylight saving|
|`std::string to_posix_string()`|Returns a posix time zone string representation of this time_zone_base object|

### Time Zone Database

|Syntax|Description|
|:-----|:----------|
|`tz_database()`|Constructor creates an empty database|
|`bool load_from_file(std::string)`|Parameter is path to a time zone spec csv file|
|`bool tz_db.add_record(std::string id, time_zone_ptr tz)`|Adds a time_zone, or a posix_time_zone, to the database. ID is the region name for this zone|
|`time_zone_ptr tz_db.time_zone_from_region(string id)`|Returns a time_zone, via a time_zone_ptr, that matches the region listed in the data file|
|`vector<string> tz_db.region_list()`|Returns a vector of strings that holds all the region ID strings from the database|

### ...