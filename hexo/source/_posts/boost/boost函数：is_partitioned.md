---
title: boost函数：is_partitioned
date: 2017-03-09 19:00:00
tags: boost
---


<!--more-->

```C++

template <typename InputIterator, typename UnaryPredicate>
bool is_partitioned ( InputIterator first, InputIterator last, UnaryPredicate p )
{
//  Run through the part that satisfy the predicate
    for ( ; first != last; ++first )
        if ( !p (*first))
            break;
//  Now the part that does not satisfy the predicate
    for ( ; first != last; ++first )
        if ( p (*first))
            return false;
    return true;
}

```

功能描述：

容器区间内的元素是否以函数P进行划分；容器区间元素分为两部分，第一部分满足函数要求，第二部分不满足函数要求。


示例：

```C++

bool isOdd(int i) {return i % 2 == 1;}
bool lessThan10(int i){return i < 10;}

int myints[] = {0, 1, 2, 3, 14, 15};
vector<int> c(myints,  myints + sizeof(myints) / sizeof(int) );
is_partitioned ( c, isOdd ); //false
is_partitioned ( c, lessThan10 ); //true
is_partitioned ( c.begin (), c.end (), lessThan10 ); //true
is_partitioned ( c.begin (), c.begin () + 3, lessThan10 ); //true
is_partitioned ( c.end (), c.end (), isOdd ); //true:empty range

```
