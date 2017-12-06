---
title: boost函数：clamp gather hex is_palindrome
date: 2017-03-10 19:00:00
tags: boost
---

<!--more-->

clamp:
```C++
template<typename T, typename Pred>   
  T const & clamp ( T const& val,   
    typename boost::mpl::identity<T>::type const & lo,   
    typename boost::mpl::identity<T>::type const & hi, Pred p )  
  {  
//    assert ( !p ( hi, lo ));    // Can't assert p ( lo, hi ) b/c they might be equal  
    return p ( val, lo ) ? lo : p ( hi, val ) ? hi : val;  
  }   
```

简述：给定三个数，返回中间数（Pred默认使用std::less)。

示例：

```C++
    int foo = 23;  
    foo = boost::algorithm::clamp(foo, 1, 10);  

    cout << foo << endl;  
```

gather:

```C++

template <  
    typename BidirectionalIterator,  // Iter models BidirectionalIterator  
    typename Pred>                   // Pred models UnaryPredicate  
std::pair<BidirectionalIterator, BidirectionalIterator> gather   
        ( BidirectionalIterator first, BidirectionalIterator last, BidirectionalIterator pivot, Pred pred )  
{  
//  The first call partitions everything up to (but not including) the pivot element,  
//  while the second call partitions the rest of the sequence.  
    return std::make_pair (  
        std::stable_partition ( first, pivot, !boost::bind<bool> ( pred, _1 )),  
        std::stable_partition ( pivot, last,   boost::bind<bool> ( pred, _1 )));  
}  

```

简述：将容器中满足pred函数要求的元素聚集到pivot的两边，并返回满足元素的区间迭代器。


stable_partition函数作用是依据pred函数将容器分为两部分(前一部分满足函数要求，后一部分不满足函数要求),且元素的相对位置不发生变化。


示例：
```C++

int arr[] = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};  

auto range = boost::algorithm::gather(arr, arr + 10, arr + 4, isEven);  

//1 3 0 2 4 6 8 5 7 9  
for(auto i = 0; i < sizeof(arr) / sizeof(int); ++i)  
{  
    cout << *(arr + i) << " ";  
}  
cout << endl;  

//0 2 4 6 8  
for(auto ite = range.first; ite != range.second; ++ite)  
{  
    cout << *ite << " ";  
}  
cout << endl;  

```

hex:

is_palindrome:
