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

---

## 5. Set Mismatch

**Description**: You have a set of integers `s`, which originally contains all the numbers from `1` to `n`. Unfortunately, due to some error, one of the numbers in `s` got duplicated to another number in the set, which results in repetition of one number and **loss of another** number.

You are given an integer array `nums` representing the data status of this set after the error.

Find the number that occurs twice and the number that is missing and return them in the form of an array.

**Pattern**: Array Traversal / In-Place Marking

**Inital Thoughts**:

1. Compare adjacent elements to find the duplicate. This only works if the array is sorted and does not directly solve the missing number.

2. Use a `Set` to detect the duplicate, then compare against `1...n` to find the missing number. This works but requires `O(n)` extra space.

**Better Insight**: Because the values should be from 1 to n, track the frequency of each number while traversing the array. A number with frequency 2 is the duplicate, while a number with frequency 0 is missing.

**Solution**:

```py
duplicate = -1
for num in nums:
    idx = abs(num) - 1

    if nums[idx] < 0:
        duplicate = abs(num)
    else:
        nums[idx] = -nums[idx]

missing = -1
for i in range(len(nums)):
    if nums[i] > 0:
        missing = i + 1

return [duplicate, missing]
```

**Time**: O(n) — two linear traversals are still `O(n)`.

**Space**: O(1) — no additional data structure is used; the input array is modified in place.

**Mistake**: Assuming the duplicate values must be next to each other. The array is not necessarily sorted.

**Key Lesson**: When the values are guaranteed to be from 1 to n, use them as array indices. In-place marking can help detect duplicates and missing values while using O(1) extra space.

## 6. How Many Numbers Are Smaller Than the Current Number

**Description**: Given the array nums, for each `nums[i]` find out how many numbers in the array are smaller than it. That is, for each `nums[i]` you have to count the number of valid `j's` such that `j != i` and `nums[j] < nums[i]`.

Return the answer in an array.

**Pattern**: Sorting / Hash Map

**Inital Thoughts**:

1. Use two loops and compare every number with every other number. This works, but takes O(n²) time.

2. Sort the array and use .index() to find each number's position. This gives the correct count, but .index() searches the array each time, making it inefficient.

**Better Insight**: Sort the numbers → store the first index of each unique number → use that index as its count of smaller numbers.

**Solution**:

```py
rank = {}
for index, num in enumerate(sorted(nums)):
    if num not in rank:
        rank[num] = index
return  [rank[num] for num in nums]
```

**Time**: O(n log n)

**Space**: O(n)

**Mistake**: Using .index() for every number. Since .index() searches from the beginning each time, it adds unnecessary O(n) work for each element.

**Key Lesson**: When sorting puts elements in their natural order, an element's first position can represent how many elements are smaller than it. A dictionary lets you store and reuse that information efficiently.

## 7. Find All Numbers Disappeared in an Array

**Description**: Given an array `nums` of n integers where `nums[i]` is in the range `[1, n]`, return an array of all the integers in the range `[1, n]` that do not appear in `nums`.

**Pattern**: Array Traversal / In-Place Marking

**Inital Thoughts**: Use `set(range(1, n + 1)) - set(nums)` to find the missing numbers. This is simple and readable, but it requires O(n) extra space.

**Better Insight**: Use each number as an index by marking `nums[abs(num) - 1]` as negative; after marking, every index with a positive value represents a missing number (`index + 1`).

**Solution**:

```py
for num in nums:
    i = abs(num) - 1
    nums[i] = -abs(nums[i])

return [i + 1 for i in range(len(nums)) if nums[i] > 0]
```

**Time**: 0(n)

**Space**: 0(1)

**Mistake**: Using a set solves the problem but uses extra memory when the input constraints allow an in-place solution.

**Key Lesson**: When values are guaranteed to be in [1, n], use them as indices to mark which numbers have appeared and find the missing ones in O(1) extra space.
