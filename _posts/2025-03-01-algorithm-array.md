---
title: "Data Structures & Algorithms Learning Plaza - Array"
author: yyuan
date: 2025-03-01 17:20:14 -0600
categories: [Learning, DSA]
tags: [leetcode, python, algorithm, array]     # TAG names should always be lowercase
media_subpath : /assets/img/in_post/dsa/
---

This series is dedicated to documenting my insights and notes as I study **Data Structures and Algorithms**. I'll primarily be using **Python** to solve problems, and my LeetCode solutions will be stored in my [Learning Plaza repo](https://github.com/yyccPhil/learning_plaza/tree/main/code). For my LeetCode practicing sequence, I'm following the guidance from [代码随想录](https://www.programmercarl.com/) (content in Chinese), while also refer to [Labuladong Algo Notes](https://labuladong.online/algo/en/). As the first post in this series, this article will focus on **Array**.

> P.S. Today marks the 龙抬头 ("Dragon raising its head") Festival in the Chinese Lunar Year 2025, and it's also the first day of March. I'm hoping to kick off this year's learning adventure with a fresh new start! Let's do this!

## Fundamentals of Array

> Excerpt and translate from [代码随想录](https://www.programmercarl.com/%E6%95%B0%E7%BB%84%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html).

### How Arrays are Stored in Memory

Arrays are data structures that store elements of the **same** type in a **contiguous** block of memory.

Some friends might wonder: can't Python's `list` store different data types? Indeed, Python offers multiple array-like data structures, each with distinct characteristics:

- The built-in `list` type, which is dynamic and can hold elements of varying types.
- The `array` type from the `array` module, which restricts elements to a single type.
- The `ndarray` from the `numpy` library, optimized for numerical computations and capable of supporting multi-dimensional arrays.

```terminal
>>> my_lst = [1,2,3]    # list
>>> import array    # built-in library
>>> my_arr = array.array('i', [1, 2, 3])    # array
>>> import numpy as np
>>> np_arr = np.array([1, 2, 3])    # numpy array
>>> print(type(my_lst))
<class 'list'>
>>> print(type(my_arr))
<class 'array.array'>
>>> print(type(np_arr))
<class 'numpy.ndarray'>    # ndarrary -> N-dimensional array
```

### Characteristics of Arrays

Array's contiguous allocation enables efficient access to any element using its index.

Here's an example of a character array, as shown in the figure:

![Array Example](array0.png){: width="972" height="589" .w-75 .normal}

Two important points:

- Array indices always start at 0.
- The memory addresses of array elements are contiguous.

This contiguous allocation makes operations like insertion and deletion more expensive due to the need to shift elements.

For example, when deleting the element at index 3, all elements after that index must be shifted, as shown in the figure:

![Array Deletion](array1.png){: width="972" height="589" .w-75 .normal}

Array elements can't really be deleted, they can only be overwritten. (This doesn't mean that arrays don't support deletion operations, but rather that deletion is implemented by overwriting the data.)

## Binary Search

Since binary search is both classic and crucial, and it involves numerous details, I have dedicated a separate post to explain it in depth and covers some related problems:
- **[704](https://leetcode.com/problems/binary-search)**: classic binary search
- **[35](https://leetcode.com/problems/search-insert-position)**: demonstrates the return value of binary search
- **[34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)**: find left and right boundaries
- **[69](https://leetcode.com/problems/sqrtx/)** and **[367](https://leetcode.com/problems/valid-perfect-square/)**

For more details, please visit: [Data Structures & Algorithms Learning Plaza - Binary Search](https://yyccphil.github.io/posts/algorithm-binary-search/).
