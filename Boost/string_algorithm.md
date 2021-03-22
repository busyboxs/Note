# Boost String algorithm

## Apis

### boost/algorithm/string/case_conv.hpp

* to_lower()
* to_lower_copy()
* to_upper()
* to_upper_copy()

### boost/algorithm/string/predicate.hpp

* starts_with()
* istarts_with()
* ends_with()
* iends_with()
* contains()
* icontains()
* equals()
* iequals()
* lexicographical_compare()
* ilexicographical_compare()
* all()

### boost/algorithm/string/classification.hpp

* is_classified()
* is_space()
* is_alnum()
* is_alpha()
* is_cntrl()
* is_digit()
* is_graph()
* is_lower()
* is_print()
* is_punct()
* is_upper()
* is_xdigit()

### boost/algorithm/string/trim.hpp

* trim_left_copy_if()
* trim_left_copy()
* trim_left_if()
* trim_left()
* trim_right_copy_if()
* trim_right_copy()
* trim_right_if()
* trim_right()
* trim_copy_if()
* trim_copy()
* trim_if()
* trim()

### boost/algorithm/string/find.hpp

* find()
* find_first()
* ifind_first()
* find_last()
* ifind_last()
* find_nth()
* ifind_nth()
* find_head()
* find_tail()
* find_token()

### boost/algorithm/string/replace.hpp

* replace_range_copy()
* replace_range()
* replace_first_copy()
* replace_first()
* ireplace_first_copy()
* ireplace_first()
* replace_last_copy()
* replace_last()
* ireplace_last_copy()
* ireplace_last()
* replace_nth_copy()
* replace_nth()
* ireplace_nth_copy()
* ireplace_nth()
* replace_all_copy()
* replace_all()
* ireplace_all_copy()
* ireplace_all()
* replace_head_copy()
* replace_head()
* replace_tail_copy()
* replace_tail()

### boost/algorithm/string/erase.hpp

* erase_range_copy()
* erase_range()
* erase_first_copy()
* erase_first()
* ierase_first_copy()
* ierase_first()
* erase_last_copy()
* erase_last()
* ierase_last_copy()
* ierase_last()
* erase_nth_copy()
* erase_nth()
* ierase_nth_copy()
* ierase_nth()
* erase_all_copy()
* erase_all()
* ierase_all_copy()
* ierase_all()
* erase_head_copy()
* erase_head()
* erase_tail_copy()
* erase_tail()

### <boost/algorithm/string/find_format.hpp

* find_format_copy()
* find_format()
* find_format_all_copy()
* find_format_all()

### boost/algorithm/string/find_iterator.hpp

* make_find_iterator()
* make_split_iterator()

### boost/algorithm/string/split.hpp

* find_all()
* ifind_all()
* split()

## Quick Reference

### Case Conversion

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`to_upper`|Convert a string to upper case|<ul><li>[to_upper_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/to_upper_copy.html)</li><li>[to_upper()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/to_upper.html)</li></ul>|
|`to_lower`|Convert a string to lower case|<ul><li>[to_lower_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/to_lower_copy.html)</li><li>[to_lower()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/to_lower.html)</li></ul>|


### Trimming

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`trim_left`|Remove leading spaces from a string|<ul><li>[trim_left_copy_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_left_copy_if.html)</li><li>[trim_left_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_left_if.html)</li><li>[trim_left_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_left_copy.html)</li><li>[trim_left()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_left.html)</li></ul>|
|`trim_right`|Remove trailing spaces from a string|<ul><li>[trim_right_copy_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_right_copy_if.html)</li><li>[trim_right_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_right_if.html)</li><li>[trim_right_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_right_copy.html)</li><li>[trim_right()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_right.html)</li></ul>|
|`trim`|Remove leading and trailing spaces from a string|<ul><li>[trim_copy_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_copy_if.html)</li><li>[trim_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_if.html)</li><li>[trim_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim_copy.html)</li><li>[trim()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/trim.html)</li></ul>|

### Predicates

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`starts_with`|Check if a string is a prefix of the other one|<ul><li>[starts_with()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/starts_with.html)</li><li>[istarts_with()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/istarts_with.html)</li></ul>|
|`ends_with`|Check if a string is a suffix of the other one|<ul><li>[ends_with()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ends_with.html)</li><li>[iends_with()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/iends_with.html)</li></ul>|
|`contains`|Check if a string is contained of the other one|<ul><li>[contains()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/contains.html)</li><li>[icontains()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/icontains.html)</li></ul>|
|`equals`|Check if two strings are equal|<ul><li>[equals()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/equals.html)</li><li>[iequals()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/iequals.html)</li></ul>|
|`lexicographical_compare`|Check if a string is lexicographically less then another one|<ul><li>[lexicographical_compare()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/lexicographical_compare.html)</li><li>[ilexicographical_compare()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ilexicographical_compare.html)</li></ul>|
|`all`|Check if all elements of a string satisfy the given predicate|<ul><li>[all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/all.html)</li></ul>|

### Find algorithm

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`find_first`|Find the first occurrence of a string in the input|<ul><li>[find_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_first.html)</li><li>[ifind_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ifind_first.html)</li></ul>|
|`find_last`|Find the last occurrence of a string in the input|<ul><li>[find_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_last.html)</li><li>[ifind_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ifind_last.html)</li></ul>|
|`find_nth`|Find the nth (zero-indexed) occurrence of a string in the input|<ul><li>[find_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_nth.html)</li><li>[ifind_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ifind_nth.html)</li></ul>|
|`find_head`|Retrieve the head of a string|<ul><li>[find_head()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_head.html)</li></ul>|
|`find_tail`|Retrieve the tail of a string|<ul><li>[find_tail()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_tail.html)</li></ul>|
|`find_token`|Find first matching token in the string|<ul><li>[find_token()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_token.html)</li></ul>|
|`find_regex`|Use the regular expression to search the string|<ul><li>[find_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_regex.html)</li></ul>|
|`find`|Generic find algorithm|<ul><li>[find()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find.html)</li></ul>|

### Erase/Replace

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`replace/erase_first`|Replace/Erase the first occurrence of a string in the input|	<ul><li>[replace_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_first.html)</li><li>[replace_first_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_first_copy.html)</li><li>[ireplace_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_first.html)</li><li>[ireplace_first_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_first_copy.html)</li><li>[erase_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_first.html)</li><li>[erase_first_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_first_copy.html)</li><li>[ierase_first()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_first.html)</li><li>[ierase_first_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_first_copy.html)</li></ul>|
|`replace/erase_last`|Replace/Erase the last occurrence of a string in the input|	<ul><li>[replace_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_last.html)</li><li>[replace_last_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_last_copy.html)</li><li>[ireplace_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_last.html)</li><li>[ireplace_last_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_last_copy.html)</li><li>[erase_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_last.html)</li><li>[erase_last_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_last_copy.html)</li><li>[ierase_last()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_last.html)</li><li>[ierase_last_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_last_copy.html)</li></ul>|
|`replace/erase_nth`|Replace/Erase the nth (zero-indexed) occurrence of a string in the input|<ul><li>[replace_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_nth.html)</li><li>[replace_nth_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_nth_copy.html)</li><li>[ireplace_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_nth.html)</li><li>[ireplace_nth_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_nth_copy.html)</li><li>[erase_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_nth.html)</li><li>[erase_nth_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_nth_copy.html)</li><li>[ierase_nth()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_nth.html)</li><li>[ierase_nth_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_nth_copy.html)</li></ul>|
|`replace/erase_all`|Replace/Erase the all occurrences of a string in the input|<ul><li>[replace_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_all.html)</li><li>[replace_all_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_all_copy.html)</li><li>[ireplace_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_all.html)</li><li>[ireplace_all_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ireplace_all_copy.html)</li><li>[erase_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_all.html)</li><li>[erase_all_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_all_copy.html)</li><li>[ierase_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_all.html)</li><li>[ierase_all_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ierase_all_copy.html)</li></ul>|
|`replace/erase_head`|Replace/Erase the head of the input|<ul><li>[replace_head()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_head.html)</li><li>[replace_head_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_head_copy.html)</li><li>[erase_head()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_head.html)</li><li>[erase_head_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_head_copy.html)</li></ul>|
|`replace/erase_tail`|Replace/Erase the tail of the input|<ul><li>[replace_tail()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_tail.html)</li><li>[replace_tail_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_tail_copy.html)</li><li>[erase_tail()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_tail.html)</li><li>[erase_tail_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_tail_copy.html)</li></ul>|
|`replace/erase_regex`|Replace/Erase a substring matching the given regular expression|<ul><li>[replace_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_regex.html)</li><li>[replace_regex_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_regex_copy.html)</li><li>[erase_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_regex.html)</li><li>[erase_regex_copy](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_regex_copy.html)</li></ul>|
|`replace/erase_regex_all`|Replace/Erase all substrings matching the given regular expression|<ul><li>[replace_all_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_all_regex.html)</li><li>[replace_all_regex_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/replace_all_regex_copy.html)</li><li>[erase_all_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_all_regex.html)</li><li>[erase_all_regex_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/erase_all_regex_copy.html)</li></ul>|
|`find_format`|Generic replace algorithm|<ul><li>[find_format()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_format.html)</li><li>[find_format_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_format_copy.html)</li><li>[find_format_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_format_all.html)</li><li>[find_format_all_copy()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_format_all_copy.html)</li></ul>|

### Split

|Algorithm name|Description|Functions|
|:-------------|:----------|:--------|
|`find_all`|Find/Extract all matching substrings in the input|<ul><li>[find_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_all.html)</li><li>[ifind_all()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/ifind_all.html)</li><li>[find_all_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_all_regex.html)</li></ul>|
|`split`|Split input into parts|<ul><li>[split()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/split.html)</li><li>[split_regex()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/split_regex.html)</li></ul>|
|`iter_find`|Iteratively apply the finder to the input to find all matching substrings|<ul><li>[iter_find()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/iter_find.html)</li></ul>|
|`iter_split`|Use the finder to find matching substrings in the input and use them as separators to split the input into parts|<ul><li>[iter_split()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/iter_split.html)</li></ul>|

### Join

|`join`|Join all elements in a container into a single string|<ul><li>[join()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/join.html)</li></ul>|
|`join_if`|Join all elements in a container that satisfies the condition into a single string|<ul><li>[join_if()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/join_if_1_3_3_9_14_3_1_2.html)</li></ul>|

### Finders and Formatters

|Finder|Description|Generators|
|:-----|:----------|:---------|
|`first_finder`|Search for the first match of the string in an input|[first_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/first_finder.html)|
|`last_finder`|Search for the last match of the string in an input|[last_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/last_finder.html)|
|`nth_finder`|Search for the nth (zero-indexed) match of the string in an input|[nth_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/nth_finder.html)|
|`head_finder`|Retrieve the head of an input|[head_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/head_finder.html)|
|`tail_finder`|Retrieve the tail of an input|[tail_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/tail_finder.html)|
|`token_finder`|Search for a matching token in an input|[token_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/token_finder.html)|
|`range_finder`|Do no search, always returns the given range|[range_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/range_finder.html)|
|`regex_finder`|Search for a substring matching the given regex|[regex_finder()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/regex_finder.html)|

|Formatter|Description|Generators|
|:-----|:----------|:---------|
|`const_formatter`|Constant formatter. Always return the specified string|[const_formatter()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/const_formatter.html)|
|`identity_formatter`|Identity formatter. Return unmodified input|[identity_formatter()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/identity_formatter.html)|
|`empty_formatter`|Null formatter. Always return an empty string|[empty_formatter()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/empty_formatter.html)|
|`regex_formatter`|Regex formatter. Format regex match using the specification in the format string|[regex_formatter()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/regex_formatter.html)|

### Iterator

|Iterator name|Description|Iterator class|
|:-----|:----------|:---------|
|`find_iterator`|Iterates through matching substrings in the input|[find_iterator](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/find_iterator.html)|
|`split_iterator`|Iterates through gaps between matching substrings in the input|[split_iterator](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/split_iterator.html)|

### Classification

|Predicate name|Description|Generator|
|:-----|:----------|:---------|
|`is_classified`|Generic `ctype` mask based classification|[is_classified()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_classified.html)|
|`is_space`|Recognize spaces|[is_space()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_space.html)|
|`is_alnum`|Recognize alphanumeric characters|[is_alnum()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_alnum.html)|
|`is_alpha`|Recognize letters|[is_alpha()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_alpha.html)|
|`is_cntrl`|Recognize control characters|[is_cntrl()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_cntrl.html)|
|`is_digit`|Recognize decimal digits|[is_digit()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_digit.html)|
|`is_graph`|Recognize graphical characters|[is_graph()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_graph.html)|
|`is_lower`|Recognize lower case characters|[is_lower()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_lower.html)|
|`is_print`|Recognize printable characters|[is_print()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_print.html)|
|`is_punct`|Recognize punctuation characters|[is_punct()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_punct.html)|
|`is_upper`|Recognize uppercase characters|[is_upper()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_upper.html)|
|`is_xdigit`|Recognize hexadecimal digits|[is_xdigit()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_xdigit.html)|
|`is_any_of`|Recognize any of a sequence of characters|[is_any_of()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_any_of.html)|
|`is_from_range`|Recognize characters inside a min..max range|[is_from_range()](https://www.boost.org/doc/libs/1_75_0/doc/html/boost/algorithm/is_from_range.html)|


