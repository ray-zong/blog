---
title: boost函数：partition_point
date: 2017-03-09 19:00:00
tags: boost
---

<!--more-->

```C++

template <typename ForwardIterator, typename Predicate>
ForwardIterator partition_point ( ForwardIterator first, ForwardIterator last, Predicate p )
{
    std::size_t dist = std::distance ( first, last );
    while ( first != last ) {
        std::size_t d2 = dist / 2;
        ForwardIterator ret_val = first;
        std::advance (ret_val, d2);
        if (p (*ret_val)) {
            first = ++ret_val;
            dist -= d2 + 1;
            }
        else {
            last = ret_val;
            dist = d2;
            }
        }
    return first;
}

```

简介：二分法检测一组序列(针对当前的判断条件是有序的)中第一个不满足条件要求的元素


示例：

```C++
    bool lessThan10(int i){return i < 10;}      

    int myints_1[] = {0, 1, 3, 2, 14, 15, 1};  

    vector<int> c1(myints_1, myints_1 + sizeof(myints_1) / sizeof(int));  

    boost::algorithm::partition_point(c1, lessThan10);  //14的迭代器  

```

此函数功能的用途暂时无法清楚知道，可以与is_partitioned函数做对比。
