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
