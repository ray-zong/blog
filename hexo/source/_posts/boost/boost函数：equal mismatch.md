---
title: boost函数：equal mismatch
date: 2017-03-10 19:00:00
tags: boost
---

<!--more-->

std::equal

```C++
template <class InputIterator1, class InputIterator2>
bool equal ( InputIterator1 first1, InputIterator1 last1,
             InputIterator2 first2);

template <class InputIterator1, class InputIterator2, class BinaryPredicate>
bool equal ( InputIterator1 first1, InputIterator1 last1,
             InputIterator2 first2, BinaryPredicate pred );
```

boost::algorithm::equal

```C++

template <class InputIterator1, class InputIterator2>
bool equal ( InputIterator1 first1, InputIterator1 last1,
             InputIterator2 first2, InputIterator2 last2 );

template <class InputIterator1, class InputIterator2, class BinaryPredicate>
bool equal ( InputIterator1 first1, InputIterator1 last1,
             InputIterator2 first2, InputIterator2 last2, BinaryPredicate pred );

```

简述：判断两个容器区间的元素是否相等。

boost::algorithm::equal相对于std::equal的参数，需要多传入第二个容器的右区间，安全性提高。

在我们使用std::equal的时候，一般也建议先判断第二个容器元素个数是否不小于第一个容器的元素个数。

示例：

```C++

auto c1 = {0, 1, 2, 3, 14, 15};  
auto c2 = {1, 2, 3};  

cout << boost::algorithm::equal ( c1.begin (),     c1.end (),       c2.begin (), c2.end ()); //  
cout << boost::algorithm::equal ( c1.begin () + 1, c1.begin () + 4, c2.begin (), c2.end ()); //true  
cout << boost::algorithm::equal ( c1.end (),       c1.end (),       c2.end (),   c2.end ()); //true  // empty sequences are alway equal to each other  


cout << endl;  

auto seq1 = { 0, 1, 2 };  
auto seq2 = { 0, 1, 2, 3, 4 };  

cout << std::equal ( seq1.begin (), seq1.end (), seq2.begin ()); // true  
        cout << std::equal ( seq2.begin (), seq2.end (), seq1.begin ()); // vs return false  
cout << std::equal ( seq1.begin (), seq1.end (), seq2.begin (), seq2.end ()); // false  

cout << endl;  

```

std::mismatch与boost::algorithm::mismatch参数接口类似，增加了对第二个容器范围的限定。

简述：判断两个容器的元素不同的起始位置。


示例：

```C++
auto c1 = {0, 1, 2, 3, 14, 15};  
auto c2 = {1, 2, 3};  

auto result = boost::algorithm::mismatch ( c1.begin (), c1.end (), c2.begin (), c2.end ());  
cout << *(result.first) << "\t" << *(result.second) << endl; // 0 1  
result = boost::algorithm::mismatch ( c1.begin () + 1, c1.begin () + 4, c2.begin (), c2.end ());  
cout << *(result.first) << "\t" << *(result.second) << endl; // 14 16119860  
result = boost::algorithm::mismatch ( c1.end (), c1.end (), c2.end (), c2.end ());  
cout << *(result.first) << "\t" << *(result.second) << endl; //266392705 16119860  

cout << endl;  

auto seq1 = { 0, 1, 2 };  
auto seq2 = { 0, 1, 2, 3, 4 };  

result = std::mismatch ( seq1.begin (), seq1.end (), seq2.begin ()); //vs  
cout << *(result.first) << "\t" << *(result.second) << endl; //1 3  
result = std::mismatch ( seq2.begin (), seq2.end (), seq1.begin ()); //vs  
cout << *(result.first) << "\t" << *(result.second) << endl; //3 1  
result = std::mismatch ( seq1.begin (), seq1.end (), seq2.begin (), seq2.end ()); //vs  
cout << *(result.first) << "\t" << *(result.second) << endl;//1 3  

cout << endl;  

```
