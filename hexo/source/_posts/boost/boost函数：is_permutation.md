---
title: boost函数：is_permutation
date: 2017-03-09 19:00:00
tags: boost
---


<!--more-->

```C++

template< class ForwardIterator1, class ForwardIterator2 >
bool is_permutation ( ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2 );

template< class ForwardIterator1, class ForwardIterator2, class BinaryPredicate >
bool is_permutation ( ForwardIterator1 first1, ForwardIterator1 last1,
                      ForwardIterator2 first2, BinaryPredicate p );


template< class ForwardIterator1, class ForwardIterator2 >
bool is_permutation ( ForwardIterator1 first1, ForwardIterator1 last1, ForwardIterator2 first2, ForwardIterator2 last2 );

template <typename Range, typename ForwardIterator>
bool is_permutation ( const Range &r, ForwardIterator first2 );

template <typename Range, typename ForwardIterator, typename BinaryPredicate>
bool is_permutation ( const Range &r, ForwardIterator first2, BinaryPredicate pred );

template< class ForwardIterator1, class ForwardIterator2, class BinaryPredicate >
bool is_permutation ( ForwardIterator1 first1, ForwardIterator1 last1,
                      ForwardIterator2 first2, ForwardIterator2 last2,
                      BinaryPredicate p );


```

简介：比较两个容器区间的元素是否一致(不是相等)。


示例：

```C++

int myints_1[] = {0, 1, 2, 3, 14, 15};
    int myints_2[] = {15, 14, 3, 1, 2};

    vector<int> c1(myints_1, myints_1 + sizeof(myints_1) / sizeof(int));
    vector<int> c2(myints_2, myints_2 + sizeof(myints_2) / sizeof(int));

    boost::algorithm::is_permutation ( c1.begin(),     c1.end (), c2.begin()); //false
    boost::algorithm::is_permutation ( c1.begin() + 1, c1.end (), c2.begin()); //true

    boost::algorithm::is_permutation ( c1.end (), c1.end (), c2.end()); //true

```

注意：

下列函数的头文件在boost\algorithm\cxx14

```C++

template< class ForwardIterator1, class ForwardIterator2, class BinaryPredicate >
bool is_permutation ( ForwardIterator1 first1, ForwardIterator1 last1,
                      ForwardIterator2 first2, ForwardIterator2 last2,
                      BinaryPredicate p );

```
