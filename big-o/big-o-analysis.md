# Big O Analysis

<iframe src="https://adaacademy.hosted.panopto.com/Panopto/Pages/Embed.aspx?pid=551bc8d2-271a-48ab-8c8b-acd301548fbd&autoplay=false&offerviewer=true&showtitle=true&showbrand=false&start=0&interactivity=all" height="405" width="720" style="border: 1px solid #464646;" allowfullscreen allow="autoplay"></iframe>

## Learning Goals

- Apply time complexity analysis to arrays and operations on arrays
- Apply space complexity analysis to arrays and operations on arrays

## Introduction

When we read code, we can analyze an algorithm and come to a conclusion about its time and space complexity. This skill will let us check the efficiency of our code as we write it or review it!

This lesson is structured this way: We will...

- introduce some guidelines and some steps to take when analyzing complexity
- apply it to a small example, and give explanations
- apply it to a large example, and give explanations

We encourage you all to read through these sections. After each example, re-read the example and follow along the same thought process, so you can come to the same conclusions too.

## Time Complexity: Code to Big O

Recall that time complexity is the measurement of how the amount of time an algorithm takes to run as the size of the input changes. Also, time complexity is usually measured in _number of operations._ Therefore, our goal is to find a formula that captures how many operations an algorithm executes.

### Time Complexity: Generic Steps

1. Read through the code, and identify all lists that have a variable size.
   - Typically, with one list, we say it has _n_ number of elements.
1. Identify all of the operations in the algorithm
1. Recognize which operations are related to the lists of size _n_
   - Typically, the operations inside of a for loop get multipled by _n_ times
   - This relationship will change depending on the kind of loop and the logic in the loop. Consider cases where the loop `break`s.
1. Create an equation that represents how many operations there are
   - This equation can use _n_ as a variable
1. Drop the constants and finding the dominant order (notes below)
1. Match this Big O to the most relevant complexity

### !callout-info

## What's an Operation?

One operation is when one operator is used. There are arithmetic operators (`+`, `-`, `*`, `%`, etc), assignment operators (`=`, `+=`, etc), comparison operators (`==`, `<`, `>=`, etc), logical operators (`and`, `or`, `not`), and more miscellaneous ones (`is`, `is not`, `in`, `not in`, etc)

### !end-callout

### Time Complexity: Small Example

This is one implementation of the linear search algorithm. It takes in a list of _n_ elements, and an item to search for. Its responsibility is to return the index of that item.

```python
def linear_search(input_list, item):
    for i in range(len(input_list)):
        if item == input_list[i]:
            return i
    return False
```

1. Identify all lists with variable size: There is one list with variable size, `input_list`, and it's a size of _n_.

2. Identify all operations in this algorithm:

   1. `if item == input_list[i]`
   1. `i in range(len(input_list))`

3. Recognize which operations have a relationship to list size:

   1. `if item == input_list[i]` can happen _n_ times
   1. `i in range(len(input_list))` can happen _n_ times
      - If this is not obvious, this is a detail that we can leave as an unknown. We may have other clues in the rest of the process that could help us. We can revisit this if we realize that this is crucial to our solution.

4. Create a formula that counts how many operations there are:

   1. Two operations can happen _n_ times. We can express this as _2 \* n_, or _2n_

5. Drop the constants:

   1. In math, when looking at _2n_, `2` is a constant that we can drop
   1. After dropping the constant, our remaining formula is _n_

6. Match this formula with the most relevant complexity:
   1. _n_ matches _O(n)_ the most closely. This algorithm has linear time complexity.

Finished! ✨ Once we've come to this conclusion, we've successfully analyzed the time complexity and found the Big O.

### !callout-info

## What is "Dropping the Constant," or Finding the Dominant Order?

In complexity analysis, programmers care about the dominant order, or the part of the complexity that affect the complexity _the most._

One step to helping us determine the dominant order is to drop the constants.

Mathematical constants are values that have a fixed value. Constants tend to be added, subtracted, multiplied, or divided in our formulas. In Big O, constants _do_ affect our complexity (it's true that O(6n) is different than O(n))... but constants do not affect the curve enough that is meaningful for complexity analysis.

Some examples:

- O(2n) is shortened to O(n)
- O(n/2) (or one half of _n_) is shortened to O(n)
- O(n + n<sup>2</sup>) is shortened to O(n<sup>2</sup>)

There are additional aspects to finding the dominant order; ultimately, Big O is the _largest_ mathematical expression.

### !end-callout

### Time Complexity: Large Example

This is one implementation of reversing the order of items in a list. This should affect the original list.

It loops through a list, and starts looking at the first item (at index `i`) and the last item (at index `j`), and swaps their values, making use of a temporary variable `temp`. It will return the list in reverse order.

```python
def reverse(input_list):
    if len(input_list) <= 1:
        return input_list

    i = 0
    j = len(input_list) - 1

    while i < j:
        temp = input_list[i]
        input_list[i] = input_list[j]
        input_list[j] = temp

        i += 1
        j -= 1

    return input_list
```

1. Identify all lists with variable size: There is one list with variable size, and it's a size of _n_.

2. Identify all operations in this algorithm:

   1. `len(input_list) <= 1`
   1. `i = 0`
   1. `j = ...`
   1. `len(input_list) - 1`
   1. `i < j`
   1. `temp = ...`
   1. `input_list[i] = ...`
   1. `input_list[j] = ...`
   1. `i += 1`
   1. `j -= 1`

3. Recognize which operations have a relationship to list size:

   1. All 4 of the operations outside of the `while` loop will only happen once; they have no relationship to list size
   1. All 6 of the operations inside the `while` loop can happen _n_ times

4. Create a formula that counts how many operations there are:

   1. We can express this as _4 + (6 \* n)_, or _4 + 6n_

5. Drop the constants:

   1. We are able to drop `4 +` and `6 *`
   1. After dropping the constants, our remaining formula is _n_

6. Match this formula with the most relevant complexity:
   1. _n_ matches _O(n)_ the most closely. This algorithm has linear time complexity.

## Space Complexity: Code to Big O

Recall that space complexity is the measurement of how much memory an algorithm uses as the size of the input changes. Space complexity is measured in the _amount of memory allocated in this algorithm._ This amount of memory doesn't include the memory that stores our data set.

Our goal will be to count the number of new values that are stored. We'll look for places where the code creates new values, such as creating new lists, new objects, strings, or primitives.

A great strategy is to count the number of initialized variables, but it will be great to understand that this is only one factor of space complexity.

### !callout-info

## Lists Require More Memory Than A Single Value

We can imagine that a single value `x = 42` will take "one unit" of memory, because it is a variable that holds one value. Following that logic, a list with many values, such as `[42, 43, 44, 45]` will take "four units" of memory. The amount of memory that the list holds is _n_ amount, in terms of space complexity.

### !end-callout

### Space Complexity: Generic Steps

1. Read through the code, and identify all places where a variable is _initially assigned (initialized)._
   - We do not need to count re-assignment of variables, as the value will occupy the same memory as the initial assignment
1. Recognize which variables have a value that could take a variable amount of memory
   - Typically, storing a list will require _n_ amount of memory
1. Create an equation that represents how many values are initialized and stored in memory
1. Drop the constants and find the dominant order
1. Match this Big O to a relevant complexity

### Space Complexity: Small Example

Given an implementation of linear search...

```python
def linear_search(input_list, item):
    for i in range(len(input_list)):
        if item == input_list[i]:
            return i
    return False
```

1. Identify all places where the algorithm initializes a variable. (Don't count the input data set!)

   - `i`
   - `item`
   - `input_list`

### !callout-secondary

## Note: We Don't Need to Count Inputs

For this exercise and the purpose of illustrating space complexity, we'll consider `item` and won't consider `input_list`. However, it is a completely valid interpretation to not consider `item` or `input_list`, as they are "inputs" into this function.

### !end-callout

2. Recognize which variables hold a variable amount of memory

   - Even though we _re-assign_ `i` _n_ number of times, `i` is only one variable holding one value. `i` doesn't hold a variable amount of memory.
   - `item` is never re-assigned

3. Create an equation to represent the amount of memory needed

   - The variable `i` will always occupy "one unit" of memory
   - The variable `item` will always occupy "one unit" of memory
   - We can express this as _1 + 1_, or _2_

4. Drop the constants

   - In this case, _2_ is a constant, which leads us to...

5. Match this to the most relevant Big O complexity
   - This algorithm is O(1), and has constant space complexity

### Space Complexity: Large Example

Using this implementation of reversing the order of a list...

```python
def reverse(input_list):
    if len(input_list) <= 1:
        return input_list

    i = 0
    j = len(input_list) - 1

    while i < j:
        temp = input_list[i]
        input_list[i] = input_list[j]
        input_list[j] = temp

        i += 1
        j -= 1

    return input_list
```

1. Identify all places where the algorithm initializes a variable. (Don't count the input data set!)

   - `i`
   - `j`
   - `temp`

2. Recognize which variables hold a variable amount of memory

   - Even though we _re-assign_ `i`, `j`, and `temp` _n_ number of times, they are each only one variable holding one value.

3. Create an equation to represent the amount of memory needed

   - The variables `i`, `j`, and `temp` will always occupy "one unit" of memory
   - We can express this as _1 + 1 + 1_, or _3_

4. Drop the constants

   - In this case, _3_ is a constant, which leads us to...

5. Match this to the most relevant Big O complexity
   - This algorithm has is O(1), and has constant space complexity

## Comparing Two Algorithms

Of course, Big O analysis is ultimately best used to compare algorithms. Given two algorithms that solve the same problem, we can find the space and time complexity of both, and analyze which is the least complex overall.

As an example, we'll compare two algorithms for reversing a list.

### Reversing a List: Algorithm A

The first implementation for reversing a list is found earlier in this lesson. We discovered it has...

- Time complexity of O(n)
- Space complexity of O(1)

### Reversing a List: Algorithm B

This algorithm takes a different approach. First, we'll make a temporary list to help us. Then, we'll populate the temporary list with the items in reverse order. Finally, we'll re-assign each item in our list with the values from the temporary list.

```python
def reverse(input_list):
    if len(input_list) <= 1:
        return input_list

    i = 0
    j = len(input_list) - 1
    # This syntax creates a list of Nones, and has the length of input_list
    temp_list = [None] * len(input_list)

    while i < len(input_list):
        temp_list[i] = input_list[j]
        i += 1
        j -= 1

    i = 0
    while i < len(input_list):
        print(i)
        input_list[i] = temp_list[i]
        i += 1

    return input_list
```

An analysis of the time complexity might follow this line of thinking:

1. There are a few operations outside of loops that will be added to the Big O
1. There are two loops that iterate over _n_ elements
1. We can start to express this as _(Some Constant Number of Operations Outside the Loop) + 2n_
1. We can drop the constants to O(n)!
1. This algorithm performs on linear time complexity

And we might analyze space complexity like so:

1. `i` and `j` are two variables that get initialized
1. `temp_list` is a list that will always have _n_ elements
1. We can express this as _2 + n_
1. We can drop the constants to O(n)
1. This algorithm performs with linear space complexity

### Constant is More Efficient than Linear

Now, we can confidently compare Algorithm A and Algorithm B:

| -                | Algorithm A | Algorithm B |
| ---------------- | ----------- | ----------- |
| Time Complexity  | O(n)        | O(n)        |
| Space Complexity | O(1)        | O(n)        |

Overall, the efficiency of these two algorithms does not differ by much; they both have a time complexity of O(n).

However, we can reference and recall that O(1) is more performant than O(n)! We can conclude that Algorithm A is more efficient in space complexity.

## Check for Understanding

<!-- There will be a whoooole worksheet of finding complexity after this, so these questions should be small and mechanic -->

<!-- Question about counting operations -->

<!-- Question about counting space -->

<!-- Question about dropping constants -->

<!-- Question about dropping constants -->

<!-- Question Takeaway -->
<!-- prettier-ignore-start -->
### !challenge
* type: paragraph
* id: 6Gqu7d
* title: Big O Analysis
##### !question

What was your biggest takeaway from this lesson? Feel free to answer in 1-2 sentences, draw a picture and describe it, or write a poem, an analogy, or a story.

##### !end-question
##### !placeholder

My biggest takeaway from this lesson is...

##### !end-placeholder
### !end-challenge
<!-- prettier-ignore-end -->
