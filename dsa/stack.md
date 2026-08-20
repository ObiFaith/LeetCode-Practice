# Tasks

## 1. Build an Array With Stack Operations

**Description**: You are given an integer array target and an integer n.

You have an empty stack with the two following operations:

- `"Push"`: pushes an integer to the top of the stack.
- `"Pop"`: removes the integer on the top of the stack.
  You also have a stream of the integers in the range `[1, n]`.

Use the two stack operations to make the numbers in the stack (from the bottom to the top) equal to `target`. You should follow the following rules:

- If the stream of the integers is not empty, pick the next integer from the stream and push it to the top of the stack.
- If the stack is not empty, pop the integer at the top of the stack.
- If, at any moment, the elements in the stack (from the bottom to the top) are equal to target, do not read new integers from the stream and do not do more operations on the stack.

Return the stack operations needed to build target following the mentioned rules. If there are multiple valid answers, return any of them.

**Pattern**: Array Traversal / Stack Simulation

**Inital Thought**: Track the current position in `target`. For each number from `1` to `n`, `Push` it. If it is not the next number needed in `target`, `Pop` it; otherwise, keep it and move to the next target number.

**Better Insight**: Since the target is already ordered, we only need to check whether the current number matches the next target value. This avoids repeatedly searching through target and keeps the traversal O(n).

**Solution**:

```py
count = 0
stack = []

for i in range(1, n+1):
    stack.append("Push")
    if i != target[count]:
        stack.append("Pop")
    else:
        count += 1
    if i == count:
        break

return stack
```

**Time**: O(n)

**Space**: O(n)

**Mistake**: Using i not in target without considering that checking membership in a list takes O(m) time.

**Key Lesson**: When repeatedly checking whether an element exists in a list, consider the cost of the membership check. Tracking the target position avoids repeated searches and keeps the solution O(n).

## 2. Evaluate Reverse Polish Notation

**Description**: You are given an array of strings tokens that represents an arithmetic expression in a [Reverse Polish Notation](https://en.wikipedia.org/wiki/Reverse_Polish_notation).

Evaluate the expression. Return an integer that represents the value of the expression.

**Note** that:

- The valid operators are `'+'`, `'-'`, `'*'`, and `'/'`.
- Each operand may be an integer or another expression.
- The division between two integers always **truncates toward zero**.
- There will not be any division by zero.
- The input represents a valid arithmetic expression in a reverse polish notation.
- The answer and all the intermediate calculations can be represented in a **32-bit** integer.

**Pattern**: Array Traversal / Stack Simulation

**Inital Thought**: Assume every third item is an operator and apply it to the two values before it. This does not work because the position of operators depends on the structure of the expression.

**Better Insight**: Traverse the tokens from left to right. Push numbers onto a stack. When an operator appears, pop the last two numbers, apply the operator, and push the result back onto the stack.

**Solution**:

```py
stack = []

for token in tokens:
    if token in "+-*/":
        b = stack.pop()
        a = stack.pop()

        if token == "+":
            stack.append(a + b)
        elif token == '-':
            stack.append(a - b)
        elif token == '*':
            stack.append(a * b)
        else:
            stack.append(int(a / b))
    else:
        stack.append(int(token))

return stack[-1]
```

**Time**: O(n) — each token is processed once.

**Space**: O(n) — the stack can contain up to n values in the worst case.

**Mistake**: Assuming operators always appear at fixed positions. In Reverse Polish Notation, an operator should be applied to the two most recent values on the stack.

**Key Lesson**: When an expression is evaluated from left to right and each operator uses the most recent values, a stack is the natural data structure.

## 3. Exclusive Time of Functions

**Description**: On a **single-threaded** CPU, we execute a program containing `n` functions. Each function has a unique ID between 0 and `n - 1`.

Function calls are **stored in a [call stack](https://en.wikipedia.org/wiki/Call_stack)**: when a function call starts, its ID is pushed onto the stack, and when a function call ends, its ID is popped off the stack. The function whose ID is at the top of the stack is the **current function being executed**. Each time a function starts or ends, we write a log with the ID, whether it started or ended, and the timestamp.

You are given a list `logs`, where `logs[i]` represents the `ith` log message formatted as a string "`{function_id}:{"start" | "end"}:{timestamp}`". For example, `"0:start:3"` means a function call with function ID 0 **started at the beginning** of timestamp 3, and `"1:end:2"` means a function call with function ID 1 **ended at the end** of timestamp 2. Note that a function can be called **multiple times, possibly recursively**.

A function's **exclusive time** is the sum of execution times for all function calls in the program. For example, if a function is called twice, one call executing for 2 time units and another call executing for 1 time unit, the **exclusive time** is `2 + 1 = 3`.

Return the **exclusive time** of each function in an array, where the value at the `ith` index represents the exclusive time for the function with ID `i`.

**Pattern**: Stack Simulation / Interval Tracking

**Inital Thought**: Calculate each function's execution time independently and use the total time of nested functions to determine the remaining time. This becomes difficult with nested and recursive calls.

**Better Insight**: Use a `stack` to track the currently running function and `previous_time` to track where the last time interval ended. When a function `starts`, give the elapsed time to the function on top of the stack. When a function `ends`, give it the elapsed time including the ending timestamp, then move `previous_time` forward by one.

**Solution**:

```py
stack = []
n_list = [0] * n
previous_time = 0

for log in logs:
    i, status, timestamp = log.split(":")
    idx, timestampx = int(i), int(timestamp)

    if status == "start":
        if stack:
            prev_fn_id = stack[-1]
            n_list[prev_fn_id] += timestampx - previous_time
        stack.append(idx)
        previous_time = timestampx
    else:
        prev_fn_id = stack.pop()
        n_list[prev_fn_id] += timestampx - previous_time + 1
        previous_time = timestampx + 1

return n_list
```

**Time**: O(n) — each log is processed once.

**Space**:O(n) — the stack and result array can each grow up to n.

**Mistake**: Forgetting that an `"end"` timestamp is inclusive. That is why the calculation is `timestamp - previous_time + 1`, and why `previous_time` becomes `timestamp + 1`.

**Key Lesson**: For nested execution problems, use a stack to track active functions and a previous timestamp to track unaccounted time. Always pay attention to whether the start/end timestamps are inclusive.

## 4. Final Prices With a Special Discount in a Shop

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
