# Task

## 1. Final Prices With a Special Discount in a Shop

**Description**: You are given an integer array prices where `prices[i]` is the price of the ith item in a shop.

There is a special discount for items in the shop. If you buy the `ith` item, then you will receive a discount equivalent to `prices[j]` where `j` is the minimum index such that `j > i` and `prices[j]` <= `prices[i]`. Otherwise, you will not receive any discount at all.

Return an integer array answer where answer[i] is the final price you will pay for the `ith` item of the shop, considering the special discount.

**Pattern**: Monotonic Stack / Array Traversal

**Initial Thought**: For each price, scan all prices to its right until finding the first price that is less than or equal to it. This directly follows the problem description but requires nested loops.

**Better Insight**: Use a monotonic stack to store indices whose discount has not been found yet. When the current price is less than or equal to the price at the top of the stack, it is the first valid discount for that item, so pop the index and calculate its final price.
**Solution**:

```py
stack = []
answers = prices.copy()
for i in range(len(prices)):
    while stack and prices[stack[-1]] >= prices[i]:
        j = stack.pop()
        answers[j] = prices[j] - prices[i]
    stack.append(i)
return answers
```

**Time**: O(n) — each index is pushed and popped at most once.

**Space**: O(n) — the stack can contain up to n indices.

**Mistake**: Using a nested loop makes the solution O(n²) because each price may scan many elements to its right.

**Key Lesson**: When looking for the next smaller or equal element, a monotonic stack can avoid repeated scanning and reduce the solution from O(n²) to O(n).

## 2. Daily Temperatures

**Description**: Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the `ith` day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

**Pattern**: Monotonic Stack / Array Traversal

**Initial Thought**: For each temperature, scan all temperatures to its right until finding the first warmer day. This directly follows the problem description but requires nested loops, resulting in an O(n²) solution.

**Better Insight**: Use a monotonic decreasing stack to store indices of days that are still waiting for a warmer temperature. As we traverse the array, whenever the current temperature is warmer than the temperature at the index on top of the stack, the current day is the answer for that previous day. Pop the index and calculate the number of days waited. Continue until the current temperature is no longer warmer than the stack's top, then push the current index.

**Solution**:

```py
answer = [0] * len(temperatures)
stack = []

for i, temp in enumerate(temperatures):
    while stack and temperatures[stack[-1]] < temp:
        prev = stack.pop()
        answer[prev] = i - prev
    stack.append(i)

return answer
```

**Time**: O(n) — each index is pushed onto the stack once and popped at most once.

**Space**: O(n) — the stack can contain up to n indices.

**Mistake**: Using a nested loop makes the solution O(n²) because each day may scan many future days repeatedly.

**Key Lesson**: When looking for the next greater element, a monotonic stack lets us process each element only once, reducing the solution from O(n²) to O(n).

## 3. Largest Rectangle in Histogram

**Description**: You are given an integer array heights where heights[i] represents the height of the ith bar in a histogram. Each bar has a width of 1. Return the area of the largest rectangle that can be formed within the histogram.

**Pattern**: Monotonic Stack / Array Traversal

**Initial Thought**: Try to calculate the area by comparing each bar with its neighboring bar. This approach assumes that a large rectangle can be identified by combining adjacent bars, but it fails because the largest rectangle can span multiple bars and is limited by the shortest bar in that range.

**Better Insight**: Treat each bar as the potential shortest bar of a rectangle. The maximum rectangle for that bar extends left and right until a smaller bar is encountered. Use a monotonic increasing stack to store indices of bars whose right boundary has not been found yet. When the current height is smaller than the height at the top of the stack, that bar can no longer extend to the right, so pop it and calculate its maximum possible area.

**Solution**:

```py
stack = []
max_area = 0
n = len(heights)

for i in range(n + 1):
h = 0 if i == n else heights[i]

    while stack and heights[stack[-1]] > h:
        height = heights[stack.pop()]
        width = i if not stack else i - stack[-1] - 1
        max_area = max(max_area, height * width)

    stack.append(i)

return max_area
```

**Time**: O(n) — each index is pushed onto the stack once and popped at most once.

**Space**: O(n) — the stack can contain up to n indices.

**Mistake**: Focusing only on neighboring bars misses rectangles that span multiple bars. A brute-force approach that checks every possible range would also take O(n²) or worse.

**Key Lesson**: When finding the largest rectangle in a histogram, think about each bar as the limiting height and find its nearest smaller bars on both sides. A monotonic stack lets us identify these boundaries efficiently and reduce the solution to O(n).
