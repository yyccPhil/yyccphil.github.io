---
title: "Data Structures & Algorithms Learning Plaza - Binary Search"
author: yyuan
date: 2025-03-02 17:20:13 -0600
categories: [Learning, DSA]
tags: [leetcode, python, algorithm, array]     # TAG names should always be lowercase
media_subpath : /assets/img/in_post/dsa/
---

Binary search is arguably one of the most iconic problems in algorithms. Its core idea is pretty straightforward, yet the implementation details are really easy to make mistakes. I view binary search as a variation of the `two-pointer` solution. The objective is to guide two pointers, left and right, toward each other to locate the `target`. And the movement here hinges on comparing the middle element between the pointers to the target, achieving a time complexity of `O(log n)`.

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

- `1 <= nums.length <= 104`
- `-104 < nums[i], target < 104`
- All the integers in `nums` are **unique**.
- `nums` is sorted in ascending order.

### Problem-solving approach
Here are 2 critical choices:
1. Loop Condition: Should we use `while left < right` or `while left <= right`?
2. Pointer Adjustment: Should we set `right = mid` or `right = mid - 1`?

These decisions depend entirely on how we define the search **interval**, the range in which we're checking for the target. There are two common approaches:
> The 'interval' here refers to the range we are checking at each iteration to determine if **the target lies within it**.
{: .prompt-tip }

- Closed Interval: `[left, right]`, where both left and right are inclusive.
- Left-Closed, Right-Open: `[left, right)`, where left is included but right is not.

Let's break this down:
1. **The Loop Condition:** First we need to understand the purpose of the loop condition, it ensures that the code inside the loop will be executed meaningfully. "Meaningful" here means there are still elements in the search interval to check. When there are no elements to check, we can exit the loop. And this depends on the type of interval we're using:
- In the `[left, right]`, both left and right boundaries are inclusive. When `left == right`, the pointers point to the same index, and it's still meaningful to check if `nums[left]` (or `nums[right]`) equals to the target. So we need to use `while left <= right`.
- In the `[left, right)`, 'left == right' signifies an empty range because right is excluded. There's nothing to check, so we need to use `while left < right`.
2. **Adjusting the Pointer:** We adjust the pointers when the middle element doesn't equal to the target. The goal is to redefine the search interval for the next iteration.
- If nums[mid] < target: We need to move the left pointer. Since the left boundary is inclusive in both interval types (`[left, right]` or `[left, right)`), setting 'left = mid' would keep mid in the next search, which is pointless, we already know mid isn't the target. Instead, we set `left = mid + 1`.
- If nums[mid] > target: We need to move the right pointer. This depends on the interval type:
  - In the `[left, right]`, the logic is same as above, so we set: `right = mid - 1`. It is important to note that the first iteration also adheres to this interval convention. Therefore, when initializing right, we should set `right = len(nums) - 1`.
  - In the `[left, right)`, the right boundary is exclusive. So we need to set `right = mid` and `mid` itself is excluded from the next search (if we set 'right = mid - 1', `mid - 1` will be missed). In this case, we initialize right as `right = len(nums)`.

**Now let's put them together:**
- For a closed interval `[left, right]`, use `while left <= right`, with `right = len(nums) - 1` and `right = mid - 1`.
- For a left-closed, right-open interval `[left, right)`, use `while left < right`, with `right = len(nums)` and `right = mid`.

Once we've settled on interval convention, coding binary search becomes much clearer.

> In many common solutions, the calculation of mid is typically done using `(right - left) // 2 + left`, which helps prevent **integer overflow**. In certain languages such as C++ and Java, if both `left` and `right` are large positive integers, their sum may exceed the maximum limit of the `int` type. In Python, integers do not overflow, so this problem is less relevant. But this approach is still recommended to maintain consistency across different programming environments.

Binary search relies on two key prerequisites:
- The array must be sorted.
- The array contains no duplicate elements (for this basic version).

> When we encounter a problem with a **sorted** array and **no duplicates**, binary search should immediately come to mind as a potential solution.
{: .prompt-tip }

### Solution
```python
# Closed Interval [left, right]
def search(self, nums: List[int], target: int) -> int:
	left = 0
	right = len(nums) - 1

	while left <= right:
		mid = (right - left)//2 + left
		if nums[mid] == target:
			return mid
		elif nums[mid] < target:
			left = mid + 1
		else:
			right = mid - 1
	return -1

# Left-closed, right-open interval [left, right)
def search(self, nums: List[int], target: int) -> int:
	left = 0
	right = len(nums)

	while left < right:
		mid = (right-left)//2 + left
		if nums[mid] == target:
			return mid
		elif nums[mid] < target:
			left = mid + 1
		else:
			right = mid
	return -1
```

I personally recommend the closed-interval approach, as it ensures consistency between the left and right boundaries when adjusting the pointers.


