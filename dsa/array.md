# Tasks

## 1. Contains Duplicate

**Description**: Given an integer array nums, return true if any value appears more than once in the array, otherwise return false.

**Pattern**: Hash Set / Frequency Tracking

**Inital Thought**: Brute Force O(n2)

**Better Insight**: Hash set length

**Solution**: `new Set(nums).size !== nums.length`

**Time**: O(n)

**Space**: O(n)

**Mistake**: Initially, create a list that tries to stores each unique number in the list but if a number is already in the unique list, return True

**Key Lesson**: Use set data strucute for unique items

---

## 2. Concatenation of Array

**Description**: Given an integer array nums of length `n`, you want to create an array `ans` of length `2n` where `ans[i] == nums[i]` and `ans[i + n] == nums[i]` for `0 <= i < n` **(0-indexed)**.

Specifically, `ans` is the concatenation of two nums arrays. Return the array `ans`.

**Pattern**: Array Manipulation / Concatenation

**Inital Thought**: Create a new array of nums and append the array with nums.

**Better Insight**: The required array is simply nums followed by another copy of nums.

**Solution**: `nums + nums`

**Time**: O(n)

**Space**: O(n)

**Mistake**: Initially, I thought there might be a way to reduce the space complexity below O(n), but since the output itself contains 2n elements, O(n) space is unavoidable.

**Key Lesson**: When the output requires a new array containing 2n elements, the minimum space complexity for the returned result is O(n).

---

## 3. Shuffle the Array

**Description**: Given the array nums consisting of 2n elements in the form `[x1,x2,...,xn,y1,y2,...,yn]`. Return the array in the form `[x1,y1,x2,y2,...,xn,yn]`.

**Pattern**: Array Traversal / Two-Part Array

**Inital Thought**: Iterate through the first half of the array and add one element from the first half followed by the corresponding element from the second half.

**Better Insight**: Since the first `n` elements are the `x` values and the remaining n elements are the `y` values, use the same index i and access `nums[i]` and `nums[i+n]`.

**Solution**: Iterate from `0` to `n-1` and append `nums[i]` followed by `nums[i+n]`.

**Time**: O(n)

**Space**: O(n)

**Mistake**: Initially used `len(nums) / 2` in `range()`. In Python 3, `/` returns a float, so `//` should be used for integer division. However, since n is already provided, using `range(n)` is simpler and clearer.

**Key Lesson**: When an array is divided into two equal sections and elements need to be interleaved, use an offset `(i + n)` to access the corresponding element from the second half.


---

## 4. Max Consecutive Ones

**Description**: Given a binary array `nums`, return the maximum number of consecutive `1`'s in the array.

**Pattern**: Array Traversal / Two-Pointer-style Counting

**Inital Thought**: Create two variables `count = 1` and `best = 0`. Start iterating the list from `index 1` and compare the current item with the previous item. If they both `1`, increase `count` by `1`. Update `best` whenever `count > best`.

**Better Insight**: Loop through the list and only increase count if `num[i] == 1` else re-initialize count to 0

**Solution**: Loop through the list, whenever a `1` is encountered, `count` is incremented, else `count` is reset to `0` then update `best` if the current `count` is larger.

**Time**: O(n) — the array is traversed once.

**Space**: O(1) — only a constant number of variables are used.

**Mistake**: The initial approach was more complicated than necessary because it compared the current element with the previous element and initialized count to 1. This can introduce edge cases, especially when the first element is 0 or when the array contains only one element. The improved approach starts with count = 0 and directly checks whether the current element is 1.

**Key Lesson**: When solving consecutive-element problems, maintain a running count and reset it when the condition is broken. Keep a separate variable for the maximum value seen so far. This often leads to a simple O(n) time and O(1) space solution.
