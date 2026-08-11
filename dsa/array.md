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
