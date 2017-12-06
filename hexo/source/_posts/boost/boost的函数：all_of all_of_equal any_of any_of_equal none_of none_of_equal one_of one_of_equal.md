---
title: boost的函数：all_of all_of_equal any_of any_of_equal none_of none_of_equal one_of one_of_equal
date: 2017-03-08 19:00:00
tags: boost
---

## all_of all_of_equal

**目录** algorithm/cxx11/all_of.hpp

<!--more-->

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename Predicate>
    bool all_of ( InputIterator first, InputIterator last, Predicate p );
template<typename Range, typename Predicate>
    bool all_of ( const Range &r, Predicate p );
}}

```

#### 简述
遍历给定容器中的所有元素，并进行条件判断。
#### 返回值
true(所有的元素都满足给定条件)
false(至少有一个元素不满足给定条件)

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename V>
    bool all_of_equal ( InputIterator first, InputIterator last, V const &val );
template<typename Range, typename V>
    bool all_of_equal ( const Range &r, V const &val );
}}

```

#### 简述
遍历给定容器中的所有元素，判断是否与给定的值相等。
#### 返回值
true(所有的元素都与给定的值相等)
false(至少有一个元素与给定的值不相等)


## any_of any_of_equal

** 目录 ** algorithm/cxx11/any_of.hpp

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename Predicate>
    bool any_of ( InputIterator first, InputIterator last, Predicate p );
template<typename Range, typename Predicate>
    bool any_of ( const Range &r, Predicate p );
}}

```
#### 简述
遍历给定容器中的所有元素，并进行条件判断。
#### 返回值
true(至少有一个元素满足给定条件)
false(所有元素都不满足给定条件)

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename V>
    bool any_of_equal ( InputIterator first, InputIterator last, V const &val );
template<typename Range, typename V>
    bool any_of_equal ( const Range &r, V const &val );
}}

```

#### 简述
遍历给定容器中的所有元素，判断是否与给定的值相等。
#### 返回值
true(至少有一个元素与给定的值相等)
false(所有的元素都与给定的值不相等)

## none_of none_of_equal

** 目录 ** algorithm/cxx11/none_of.hpp

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename Predicate>
    bool none_of ( InputIterator first, InputIterator last, Predicate p );
template<typename Range, typename Predicate>
    bool none_of ( const Range &r, Predicate p );
}}

```

#### 简述
遍历给定容器中的所有元素，并进行条件判断。
#### 返回值
true(所有元素都不满足给定条件)
false(至少有一个元素满足给定条件)

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename V>
    bool none_of_equal ( InputIterator first, InputIterator last, V const &val );
template<typename Range, typename V>
    bool none_of_equal ( const Range &r, V const &val );
}}

```

#### 简述
遍历给定容器中的所有元素，判断是否与给定的值相等。
#### 返回值
true(所有的元素都与给定的值不相等)
false(至少有一个元素与给定的值相等)


## one_of one_of_equal

** 目录 ** algorithm/cxx11/one_of.hpp

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename Predicate>
    bool one_of ( InputIterator first, InputIterator last, Predicate p );
template<typename Range, typename Predicate>
    bool one_of ( const Range &r, Predicate p );
}}

```
#### 简述
遍历给定容器中的所有元素，并进行条件判断。
#### 返回值
true(至少有一个元素满足给定条件)
false(所有元素都不满足给定条件)

```C++

namespace boost { namespace algorithm {
template<typename InputIterator, typename V>
    bool one_of_equal ( InputIterator first, InputIterator last, V const &val );
template<typename Range, typename V>
    bool one_of_equal ( const Range &r, V const &val );
}}

```
#### 简述
遍历给定容器中的所有元素，判断是否与给定的值相等。
#### 返回值
true(至少有一个元素与给定的值相等)
false(所有的元素都与给定的值不相等)

## 示例

```C++
bool isOdd(int i) {return i % 2 == 1;}
bool lessThan10(int i){return i < 10;}

int myints[] = {0, 1, 2, 3, 14, 15};
vector<int> c(myints,  myints + sizeof(myints) / sizeof(int) );

//all_of
all_of ( c, isOdd ); //false
all_of ( c.begin (), c.end (), lessThan10 ); //false
all_of ( c.begin (), c.begin () + 3, lessThan10 ); //true
all_of ( c.end (), c.end (), isOdd ); //true:empty range

//all_of_equal
all_of_equal ( c, 3 ); //false
all_of_equal ( c.begin () + 3, c.begin () + 4, 3 ); //true
all_of_equal ( c.begin (), c.begin (), 99 ); //true:empty range

//any_of
any_of ( c, isOdd ); //true
any_of ( c.begin (), c.end (), lessThan10 ); //true
any_of ( c.begin () + 4, c.end (), lessThan10 ); //false
any_of ( c.end (), c.end (), isOdd ); // false:empty range

//any_of_equal
any_of_equal ( c, 3 ); //true
any_of_equal ( c.begin (), c.begin () + 3, 3 ); //false
any_of_equal ( c.begin (), c.begin (), 99 ); //false:empty range

//none_of
none_of ( c, isOdd ); //false
none_of ( c.begin (), c.end (), lessThan10 ); //false
none_of ( c.begin () + 4, c.end (), lessThan10 ); //true
none_of ( c.end (), c.end (), isOdd ); //true:empty range

//none_of_equal
none_of_equal ( c, 3 ); //false
none_of_equal ( c.begin (), c.begin () + 3, 3 ); //true
none_of_equal ( c.begin (), c.begin (), 99 ); //true:empty range

//one_of
one_of ( c, isOdd ); //false
one_of ( c.begin (), c.end (), lessThan10 ); //false
one_of ( c.begin () + 3, c.end (), lessThan10 ); //true
one_of ( c.end (), c.end (), isOdd ); //false:empty range

//one_of_equal
one_of_equal ( c, 3 ); //true
one_of_equal ( c.begin (), c.begin () + 3, 3 ); //false
one_of_equal ( c.begin (), c.begin (), 99 ); //false:empty range

```

>注意
    错误：对重载函数的调用不明确
    修改：在all_of前加上域名，boost::algorithm::all_of。
