---
title: boost的函数：is_sorted is_sorted_until is_increasing(decreasing) is_strictly_increasing(decreasing)
date: 2017-03-09 19:00:00
tags: boost
---

本文件的最底层实现为is_sorted_until函数，其他函数都是通过间接调用此函数实现函数功能。

<!--more-->

is_sorted_until函数源码

```C++

template <typename ForwardIterator, typename Pred>  
    ForwardIterator is_sorted_until ( ForwardIterator first, ForwardIterator last, Pred p )  
    {  
        if ( first == last ) return last;  // the empty sequence is ordered  
        ForwardIterator next = first;  
        while ( ++next != last )  
        {  
            if ( p ( *next, *first ))  
                return next;  
                first = next;  
        }  
        return last;      
    }

```

 (1) is_sorted、is_increasing的Pred参数为C++的标准模板函数less;

 (2) is_decreasing的Pred参数为C++的标准模板函数greater;

 (3) is_strictly_increasing的Pred参数为C++的标准模板函数less_equal;

 (4) is_strictly_decreasing的Pred参数为C++的标准模板函数greater_equal.
