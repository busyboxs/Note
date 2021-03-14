# boost date_time library

## Date

----

### Construction

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

### Construct from String

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

### Construct from Clock

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

### Accessors

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

### Convert to String

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

### Operators

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

### Struct tm Functions

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