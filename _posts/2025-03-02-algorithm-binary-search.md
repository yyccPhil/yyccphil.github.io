---
title: "Data Structures & Algorithms Learning Plaza - Binary Search"
author: yyuan
date: 2025-03-02 17:20:13 -0600
categories: [Learning, DSA]
tags: [leetcode, python, algorithm, array]     # TAG names should always be lowercase
media_subpath : /assets/img/in_post/dsa/
---

Binary search is arguably one of the most iconic problems in algorithms. Its core idea is pretty straightforward, yet the implementation details are really easy to make mistakes. I view binary search as a variation of the `two-pointer` approach. The objective is to guide two pointers, left and right, toward each other to locate the `target`. And the movement here hinges on comparing the middle element between the pointers to the target, achieving a time complexity of `O(log n)`.

**LeetCode link: [704](https://leetcode.com/problems/binary-search)**

### Description
Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search `target` in `nums`. If `target` exists, then return its index. Otherwise, return `-1`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

Input: `nums = [-1,0,3,5,9,12], target = 9`<br>
Output: `4`<br>
Explanation: `9 exists in nums and its index is 4`

**Example 2:**

Input: `nums = [-1,0,3,5,9,12], target = 2`<br>
Output: `-1`<br>
Explanation: `2 does not exist in nums so return -1`

**Constraints:**

- `1 <= nums.length <= 10^4`
- `-10^4 < nums[i], target < 10^4`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.

### Problem-solving approach
Here are 2 critical choices:
1. Loop Condition: Should we use `while left < right` or `while left <= right`?
2. Pointer Adjustment: Should we set `right = mid` or `right = mid - 1`?

These decisions depend entirely on how we define the search **interval**, the range in which we're checking for the target. There are two common approaches:
> The 'interval' here refers to the range we are checking at each iteration to determine if **the `target` lies within it**.
{: .prompt-tip }

- Closed Interval: `[left, right]`, where both left and right are inclusive.
- Left-Closed, Right-Open: `[left, right)`, where left is included but right is not.

Let's break this down:
1. **The Loop Condition:** First we need to understand the purpose of the loop condition, it ensures that the code inside the loop will be executed meaningfully. "Meaningful" here means there are still elements in the search interval to check. When there are no elements to check, we can exit the loop. And this depends on the type of interval we're using:
- In the `[left, right]`, both left and right boundaries are inclusive. When `left == right`, the pointers point to the same index, and it's still meaningful to check if `nums[left]` (or `nums[right]`) equals the `target`. So we need to use `while left <= right`.
- In the `[left, right)`, 'left == right' signifies an empty range because right is excluded. There's nothing to check, so we need to use `while left < right`.
2. **Adjusting the Pointer:** We adjust the pointers when the middle element doesn't equal to the target. The goal is to redefine the search interval for the next iteration.
- If `nums[mid] < target`: We need to move the left pointer. Since the left boundary is inclusive in both interval types (`[left, right]` or `[left, right)`), setting 'left = mid' would keep `mid` in the next search, which is pointless, we already know `mid` isn't the `target`. Instead, we set `left = mid + 1`.
- If `nums[mid] > target`: We need to move the right pointer. This depends on the interval type:
  - In the `[left, right]`, the logic is same as above, so we set: `right = mid - 1`. It is important to note that the first iteration also adheres to this interval convention. Therefore, when initializing right, we should set `right = len(nums) - 1`.
  - In the `[left, right)`, the right boundary is exclusive. So we need to set `right = mid` and `mid` itself is excluded from the next search (if we set 'right = mid - 1', `mid - 1` will be missed). In this case, we initialize right as `right = len(nums)`.

**Now let's put them together:**
- For closed interval `[left, right]`, use `while left <= right`, with `right = len(nums) - 1` and `right = mid - 1`.
- For left-closed, right-open interval `[left, right)`, use `while left < right`, with `right = len(nums)` and `right = mid`.

Once we've settled on interval convention, coding binary search becomes much clearer.

> In many common solutions, the calculation of `mid` is typically done using `(right - left) // 2 + left`, which helps prevent **integer overflow**. In certain languages such as C++ and Java, if both `left` and `right` are large positive integers, their sum may exceed the maximum limit of the `int` type. In Python, integers do not overflow, so this problem is less relevant. But this approach is still recommended to maintain consistency across different programming environments.

Binary search relies on two key prerequisites:
- The array must be sorted.
- The array contains no duplicate elements (for this basic version).

> When we encounter a problem with a **sorted** array and **no duplicates**, binary search should immediately come to mind as a potential solution.
{: .prompt-tip }

### Solution
```python
# Closed Interval [left, right]
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1

        while left <= right:
            mid = (right - left)//2 + left
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
        return -1

# Left-closed, right-open interval [left, right)
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
        return -1
```

I personally recommend the Left-closed, right-open interval `[left, right)` approach. Because if the `target` isn’t found, the loop exits when `left == right`, which makes it very straightforward when searching for the left and right boundaries, allowing us to return either `left` or `right`. For more details, please see [below](https://yyccphil.github.io/posts/algorithm-binary-search/#problem-solving-approach-2).

## Relevant Problem Recommendations

### 35. Search Insert Position

**LeetCode link: [35](https://leetcode.com/problems/search-insert-position)**

#### Description
Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

Input: `nums = [1,3,5,6], target = 5`<br>
Output: `2`

**Example 2:**

Input: `nums = [1,3,5,6], target = 2`<br>
Output: `1`

**Example 3:**

Input: `nums = [1,3,5,6], target = 7`<br>
Output: `4`

**Constraints:**

- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` contains **distinct** values sorted in **ascending** order.
- `-10^4 <= target <= 10^4`

#### Problem-solving approach
This problem primarily demonstrates a **characteristic** of the binary search:
- In left-closed, right-open search interval `[left, right)`, when the loop exits, if the `target` is not found, the pointers stop at the smallest index corresponding to a value greater than the `target` (if the `target` is greater than the `max(nums)`, the value is the `len(nums)`).

There’s no need to memorize this result, we can simply test it with any example. However, regarding the selection of the search interval, I still recommend to use `[left, right)` interval, so that we don’t need to worry about whether to return `left` or `right`. For detailed differences in return values, please refer to [Labuladong Algo Notes](https://labuladong.online/algo/en/essential-technique/binary-search-framework/#what-to-return-if-target-does-not-exist)[^footnote].

For example, in the array `[1, 4, 10, 11, 15]`:
- Searching for 5 returns 2, corresponding to the value 10.
- Searching for 13 returns 4, corresponding to the value 15.
- Searching for 20 returns 5, which is the length of the array.

#### Solution
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
        return left
```

### 34. Find First and Last Position of Element in Sorted Array

**LeetCode link: [34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)**

#### Description
Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with `O(log n)` runtime complexity.

**Example 1:**

Input: `nums = [5,7,7,8,8,10], target = 8`<br>
Output: `[3,4]`

**Example 2:**

Input: `nums = [5,7,7,8,8,10], target = 6`<br>
Output: `[-1,-1]`

**Example 3:**

Input: `nums = [], target = 0`<br>
Output: `[-1,-1]`

**Constraints:**

- `0 <= nums.length <= 10^5`
- `-10^9 <= nums[i] <= 10^9`
- `nums` is a non-decreasing array.
- `-10^9 <= target <= 10^9`

#### Problem-solving approach
In this problem, we need to find boundaries when the `target` value is not unique. Same as in the classic binary search, we no longer have the constraint that "all integers in `nums` are **unique**." The most straightforward solution is that when we find a `target`, we do not just return its index. Instead, we need to continue searching on both sides to locate the boundaries. A naive approach might be to adjust the code when `if nums[mid] == target`, we easily set `mid += 1` or `mid -= 1`. While this works, it’s not ideal, because it risks breaking the logarithmic time complexity of binary search.

So the key challenge is how to adjust our actions when `if nums[mid] == target` while still maintaining `O(log n)` time complexity. To achieve this, we need to keep moving the left and right pointers to shrink the search interval, just as in standard binary search. For consistency and simplicity, we’ll still use left-open, right-closed interval `[left, right)`, as mentioned [above](https://yyccphil.github.io/posts/algorithm-binary-search/#solution).
- For the left boundary, when we find the `target`, we set `right = mid` to keep searching leftward.
- For the right boundary, we set `left = mid + 1` to continue searching rightward. This process repeats until the loop exits when `left == right`.

Now, the final question is: what do we return after the loop terminates?
- For the left boundary, since we set `right = mid` each time, when the loop exits, `right` equals `mid`, and `right` equals `mid` as it is loop exiting condition, so that `left` equals `mid` as well. Thus, we can return `left` (we can absolutely return `right`, but we’ll stick with `left` for consistency with the right boundary logic).
- For the right boundary, because we set `left = mid + 1` each time, when the loop exits, `left` will be 1 index greater than `mid`. Therefore, we return `left - 1`.

Finally, we just need to check whether `nums[left]` (left boundary) or `nums[left - 1]` (right boundary) is equal to `target`.

> Be sure to check for **out-of-bounds** errors as well.
{: .prompt-warning }
- When searching for the left boundary, based on the experience from [problem 35](https://yyccphil.github.io/posts/algorithm-binary-search/#problem-solving-approach-1): if the `target` is not found, `left` may become `len(nums)`. 
- When searching for the right boundary, if `target` is unique and happens to be the first element in `nums`, `left - 1` could smaller than 0.

For consistency and simplicity, we can always verify whether the index is valid before accessing it.

#### Solution
```python
# Left-closed, right-open interval [left, right)
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return [-1,-1]
        return [self.searchLeft(nums,target),self.searchRight(nums,target)]

    def searchLeft(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] == target:
                right = mid
            elif nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
        
        if left < 0 or left >= len(nums):
            return -1
        return left if nums[left]==target else -1

    def searchRight(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] == target:
                left = mid + 1
            elif nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid
        
        res = left - 1  # store left - 1 to maintain a consistent format.
        if res < 0 or res >= len(nums):
            return -1
        return res if nums[res]==target else -1

# Simplified version
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        if not nums:
            return [-1,-1]
        return [self.searchLeft(nums,target),self.searchRight(nums,target)]

    def searchLeft(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        
        if left >= len(nums):
            return -1
        return left if nums[left]==target else -1

    def searchRight(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)

        while left < right:
            mid = (right-left)//2 + left
            if nums[mid] > target:
                right = mid
            else:
                left = mid + 1
        
        if left - 1 < 0:
            return -1
        return left - 1 if nums[left - 1]==target else -1
```

### 69. Sqrt(x)

**LeetCode link: [69](https://leetcode.com/problems/sqrtx/)**

#### Description
Given a non-negative integer `x`, return _the square root of `x` rounded down to the nearest integer_. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

**Example 1:**

Input: `x = 4`<br>
Output: `2`<br>
Explanation: `The square root of 4 is 2, so we return 2.`

**Example 2:**

Input: `x = 8`<br>
Output: `2`<br>
Explanation: `The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.`

**Constraints:**

- `0 <= x <= 2^31 - 1`

#### Problem-solving approach
This problem is very similar to the classic binary search. We can treat it as searching for the square root of a number in a virtual sorted array ranging from 0 to the number itself. This setup satisfies the two key prerequisites of binary search: the array is **sorted** and contains **no duplicates**. So the only difference is that instead of checking `if nums[mid] == target`, we check `if mid**mid == x`.

However, be careful about what to return if the square root is not found. Based on the experience from [problem 35](https://yyccphil.github.io/posts/algorithm-binary-search/#problem-solving-approach-1): when the loop exits, the pointers stop at the smallest index corresponding to a value greater than the `target`. Since the problem requires us to return _the square root of `x` rounded down to the nearest integer_, we should return the number before the pointer, that is `left - 1`.

#### Solution
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        left, right = 0, x+1

        while left < right:
            mid = (right-left)//2+left

            if mid*mid == x:
                return mid
            elif mid*mid > x:
                right = mid
            elif mid*mid < x:
                left = mid + 1
        return left - 1
```

### 367. Valid Perfect Square

**LeetCode link: [367](https://leetcode.com/problems/valid-perfect-square/)**

#### Description
Given a positive integer num, return `true` _if `num` is a perfect square or `false` otherwise_.

A `perfect square` is an integer that is the square of an integer. In other words, it is the product of some integer with itself.

You must not use any built-in library function, such as `sqrt`.

**Example 1:**

Input: `num = 16`<br>
Output: `true`<br>
Explanation: `We return true because 4 * 4 = 16 and 4 is an integer.`

**Example 2:**

Input: `num = 14`<br>
Output: `false`<br>
Explanation: `We return false because 3.742 * 3.742 = 14 and 3.742 is not an integer.`

**Constraints:**

- `0 <= x <= 2^31 - 1`

#### Problem-solving approach
This problem is even simpler and more straightforward than the previous one. We just need to check whether we can find the square root. If we do, we return `True`; otherwise, once we exit the loop, we simply return `False`.

#### Solution
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left, right = 0, num+1

        while left < right:
            mid = (right-left)//2 + left
            if mid*mid == num:
                return True
            elif mid*mid > num:
                right = mid
            elif mid*mid < num:
                left = mid + 1
        return False
```

## Reference

[^footnote]: [Labuladong Algo Notes: Binary Search Algorithm Code Template](https://labuladong.online/algo/en/essential-technique/binary-search-framework/#what-to-return-if-target-does-not-exist)